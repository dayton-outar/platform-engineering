# Chapter 8

Source: [Web.Security.for.Developers-2020-8.pdf](Web.Security.for.Developers-2020-8.pdf)

## Cross-Site Request Forgery Attacks

A *cross-site request forgery (CSRF or XSRF)* attack causes a victim's browser to submit an unwanted request using credentials the browser attaches automatically. The application mistakes the authenticated request for the user's intent. Unlike [Cross-Site Scripting](CROSS.SITE.SCRIPTING.ATTACKS.md), CSRF does not require injecting script into the target application or reading its response.

![Figure 8-1: Chapter-opening artwork](/.attachments/8-1-Web.Security.for.Developers-2020-8.png)

*Figure 8-1: Chapter-opening artwork*
<!-- Source: Web.Security.for.Developers-2020-8.pdf, PDF page 1; generated artwork number. -->

### Anatomy of a CSRF Attack

A typical attack requires three conditions: the browser can authenticate to the target automatically; the attacker can induce a request with the required parameters; and the server lacks an effective check that binds the request to an authorized user interaction. A logged-in session cookie is a common source of ambient authority. Whether it accompanies a particular request depends on cookie scope, SameSite settings, and browser behavior.

Consider a training application that changes an email address when it receives a form submission. An attacker-controlled page might contain:

```html
<!-- Deliberately malicious example for an isolated training application. -->
<form action="https://accounts.example.test/email" method="post">
  <input type="hidden" name="email" value="attacker@example.test">
  <button>Claim your reward</button>
</form>
```

If the victim submits this form and the browser sends the target's session cookie, an unprotected endpoint could change the address. The attacker does not need to read the response. The source's historical Twitter example demonstrates the same mistake using a state-changing GET link. POST forms are also straightforward attack vectors; GET is not uniquely vulnerable.

### Follow REST Principles

Keep GET and HEAD free of requested business-state changes. A page view must not transfer money, change credentials, or delete an account. Use appropriate mutation methods and reject unsupported methods, including accidental GET aliases. Do not rely on choosing POST alone: browsers can send cross-site form POSTs.

For example, GET `/email` displays a form, while POST `/email` requires authentication, authorization, and CSRF validation before updating the account. This separation also prevents crawlers, previews, and link prefetching from causing business changes.

### Implement Anti-CSRF Protection

Use the framework's maintained CSRF middleware and enable it on every relevant mutation route. With a synchronizer token, the server generates an unpredictable value bound to the session, embeds it in the legitimate form, and validates the submitted value before acting. Missing, invalid, or another session's tokens must fail. Keep tokens out of URLs and logs.

A cookie by itself is insufficient: it may accompany the forged request too. A double-submit design additionally compares a separately submitted value; prefer a framework implementation with a signed token bound to the session over naive cookie equality. XSS can defeat token defenses. See [OWASP CSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).

Framework-neutral processing order:

```text
require an authenticated session
require an allowed mutation method
validate the framework CSRF token
authorize the requested action and target object
validate the submitted data
apply the change and record a redacted audit event
```

The source illustrates a hidden token in a Tornado form. The same principle applies to JavaScript clients: use the framework's supported token/header integration, with server-side enforcement. Token validation establishes a security condition; it does not replace checking whether the user may modify the selected object. See [Permissions](PERMISSIONS.md).

### Use the SameSite Cookie Attribute

Set SameSite explicitly on session cookies. `Strict` restricts cross-site delivery most strongly, but can disrupt incoming links and external sign-in flows. `Lax` permits cookies on cross-site top-level navigation using safe methods; it does not mean all cross-site GET requests carry cookies. `None` allows cross-site delivery and requires `Secure`. Same-site is broader than same-origin, so sibling subdomains deserve attention. These details are specified in [MDN's Set-Cookie reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie).

An illustrative session cookie is:

```http
Set-Cookie: __Host-session=<opaque-random-value>; Path=/; Secure; HttpOnly; SameSite=Lax
```

The placeholder is not a real token. A `__Host-` cookie requires HTTPS, `Path=/`, and no `Domain` attribute. `HttpOnly` blocks JavaScript access to the cookie; it does not stop the browser from sending it with requests. A separate CSRF token cookie may need to be readable by JavaScript, depending on the framework. See [MDN's cookie attributes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie).

Treat SameSite as an additional control, and test legitimate cross-site workflows. Do not assume its presence repairs a state-changing GET endpoint or every same-site attack.

### Require Reauthentication for Sensitive Actions

The source recommends *reauthentication* for actions such as password changes and payments. Bind confirmation to the specific action and display its important details. A recent authenticated session alone may not demonstrate intent to authorize a particular transaction. This also helps when a user leaves a device unlocked. See [Compromising Authentication](COMPROMISING.AUTHENTICATION.md).

### Verification Checklist

These are suggested acceptance checks for an application adopting the guidance:

- A legitimate form and JavaScript request succeed with their expected credentials and tokens.
- Missing, malformed, and another session's tokens fail before any state changes.
- A cross-site form submission cannot change account data.
- GET, HEAD, method overrides, and alternate routes cannot bypass mutation protections.
- Cookie settings preserve intended sign-in and navigation flows in supported browsers.
- Authorization still rejects valid-token requests for another user's resources.

For additional origin validation and Fetch Metadata defenses, consult [OWASP CSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html). These complement the core request-validation design.
