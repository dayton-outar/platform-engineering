# Injection Attacks

*Injection attacks* occur when untrusted data is interpreted as instructions. The interpreter might be a SQL engine, operating-system shell, template engine, or object deserializer. A browser's form restrictions do not constrain an attacker: scripts can construct requests directly, and request headers such as `User-Agent` do not establish trust.

The central defense is to keep data separate from executable instructions. Validation, least privilege, and monitoring add protection, but cannot make unsafe string construction reliable. This guide follows the source's sequence: SQL injection, command injection, remote code execution, and file uploads.

### SQL Injection

#### What Is SQL?

*Structured Query Language* operates on relational data. Applications commonly use `SELECT`, `INSERT`, `UPDATE`, and `DELETE` through a database driver. If an application builds a SQL statement by joining request values with SQL syntax, the database may interpret those values as part of the program.

#### Anatomy of a SQL Injection Attack

This deliberately vulnerable Java fragment places a request value inside quoted SQL:

```java
String sql = "SELECT id FROM users WHERE email = '" + email + "'";
```

An input such as `x' OR '1'='1` changes the condition into a tautology and can return unintended rows. Other payloads can modify data or bypass checks, depending on the query, database, driver, and account permissions. Stacked statements are not supported in every configuration; their absence does not make concatenated SQL safe.

#### Use Parameterized Statements

Use *bind parameters* for values. The driver treats bound input as data rather than SQL syntax. Do not implement this by manually replacing quotes. Dynamic identifiers such as column names require a separate allowlist; placeholders are for values. See [OWASP SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html).

This corrected JDBC example assumes an existing connection and an untrusted `email` string:

```java
String sql = "SELECT id FROM users WHERE email = ?";
try (PreparedStatement statement = connection.prepareStatement(sql)) {
    statement.setString(1, email);
    try (ResultSet result = statement.executeQuery()) {
        while (result.next()) {
            long userId = result.getLong("id");
            // Apply application logic and authorization separately.
        }
    }
}
```

This is a lookup, not a complete login implementation. Password verification belongs in the authentication subsystem. The source's `Statement.executeQuery(sql, email, password)` example is not a valid JDBC binding API; the snippet above uses `PreparedStatement` as documented by [Oracle's JDBC tutorial](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html).

#### Use Object-Relational Mapping Carefully

An *object-relational mapping* library can generate safe parameterized SQL, but using an ORM does not make raw query strings safe. Review raw SQL, custom expressions, filters, and sorting separately. A query such as `User.find_by(email: email)` uses structured values; interpolating input into a SQL string reintroduces the original risk. Use the library's documented parameter-binding facilities. [OWASP's SQL guidance](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html) also covers query-language injection and allowlisting.

The same data-versus-instructions distinction applies to LDAP and NoSQL queries. Their grammar and APIs differ; do not assume SQL escaping protects them. Validate expected types and use the destination API's structured query mechanisms.

#### Use Defense in Depth

Give the runtime account only the operations and data it needs. Schema migration privileges generally belong to a separate identity. Read-only reporting should not require write permissions. This limits damage if an injection flaw escapes review, although authorized reads can still disclose sensitive data.

Avoid detailed database errors in public responses. Direct output and error messages can assist exploitation; even without them, attackers may infer results from response differences or timing. Generic errors do not fix blind injection. See [Information Leaks](INFORMATION.LEAKS.md).

### Command Injection

#### Anatomy of a Command Injection Attack

A shell interprets operators as well as command arguments. The source's DNS lookup example inserts a domain into a shell command; input containing an operator followed by another command makes the shell do more than a lookup.

![Figure 6-1: Using the URL to inject a malicious command](/.attachments/6-1-Web.Security.for.Developers-2020-6.png)

*Figure 6-1: Using the URL to inject a malicious command*
<!-- Source: Web.Security.for.Developers-2020-6.pdf, PDF page 15; caption on PDF page 16. -->

The screenshot uses a harmless `echo` demonstration. In the same vulnerable construction, an attacker could request operations available to the application's operating-system identity. The resulting access is limited by actual privileges, not by the web form's intended purpose.

#### Avoid the Shell and Validate Arguments

Prefer a library operation to a command. If a process is necessary, use a fixed executable and a structured argument list without a shell. Validate argument semantics too: a single argument can still be interpreted as an option or a dangerous filename. Shell escaping alone does not address that risk. See [OWASP OS Command Injection Defense](https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html).

This repository example restricts a diagnostic lookup to predefined targets:

```python
import subprocess

TARGETS = {"documentation": "example.com", "test": "example.org"}

def lookup(target_name):
    domain = TARGETS[target_name]  # Unknown names are rejected.
    return subprocess.run(
        ["/usr/bin/nslookup", domain],
        shell=False,
        check=True,
        capture_output=True,
        text=True,
        timeout=3,
    ).stdout
```

The executable path depends on the deployment. Handle failures with generic responses, authorize and rate-limit the endpoint, and escape output if it is embedded in HTML. Run the worker with restricted filesystem and network access. An argument list prevents shell parsing; it does not sandbox the called program.

### Remote Code Execution

Remote code execution describes the outcome of executing attacker-controlled code in a server process. The source illustrates a historical Rails request-parsing issue involving XML and YAML: converting external bytes into rich runtime objects can invoke behavior the application never intended.

Avoid deserializing untrusted input into arbitrary runtime types. Prefer simple data formats with explicit schemas and bounded sizes, and use safe parser modes. Disable unnecessary request parsers, track dependency advisories, and update vulnerable components. These controls are detailed in [OWASP Deserialization](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html). Code execution has other causes too; unsafe deserialization is one example, not a complete definition.

### File Upload Vulnerabilities

An upload becomes especially dangerous when the server routes it through an interpreter. The source describes a web shell uploaded in place of a profile picture. Later requests execute the uploaded code with the server's privileges.

![Figure 6-2: If your file upload function is vulnerable, a hacker could use a web shell to access your database credentials.](/.attachments/6-2-Web.Security.for.Developers-2020-6.png)

*Figure 6-2: If your file upload function is vulnerable, a hacker could use a web shell to access your database credentials.*
<!-- Source: Web.Security.for.Developers-2020-6.pdf, PDF page 23. -->

The screenshot is an illustrative source example, not deployment configuration.

#### Host Files on a Secure System

Store uploads outside application code and preferably outside the web root. A dedicated storage service can help separate responsibilities, but still requires correct access policy and serving configuration. A CDN does not automatically make an upload safe or private.

#### Ensure Uploaded Files Cannot Be Executed

Disable script handlers for upload storage and prevent executable routing. Generate storage names rather than trusting supplied paths. Removing an operating-system execute bit alone is insufficient: an interpreter can read and run a script without that bit.

#### Validate the Content of Uploaded Files

Allow only required formats, inspect content rather than trusting the declared MIME type, and enforce size and processing limits. Isolate parsers used to inspect or transform uploads. A valid-looking header is not proof that the full file is harmless.

#### Scan Uploaded Files

Use malware scanning or content reconstruction where appropriate, and isolate pending files. These supplement storage and validation controls; they are useful across platforms and cannot guarantee detection. The upload controls above follow [OWASP File Upload](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html).

### Verification and Related Guidance

For an application adopting this guide, trace request values to interpreter calls and review every raw query or command construction. In isolated tests, confirm that quote characters remain data, shell operators cannot create extra commands, and uploads cannot invoke script handlers. Exercise authorization and resource limits as well as successful requests.

Continue with [Untrusted Input](UNTRUSTED.INPUT.md), [Permissions](PERMISSIONS.md), and [Security Testing](SECURITY.TESTING.md). [Cross-Site Scripting](CROSS.SITE.SCRIPTING.ATTACKS.md) examines the related case where the browser interprets attacker-controlled content.
