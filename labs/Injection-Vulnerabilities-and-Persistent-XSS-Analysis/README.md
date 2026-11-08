# Injection Vulnerabilities and Persistent XSS Analysis

## Overview

This student case study reviews a narrated lesson on injection and request-forgery risks, followed by an alert-only XSS demonstration in Google's intentionally vulnerable XSS Game, Level 2. It is a demonstration review, not an independently performed assessment or discovery of a real-world vulnerability.

## Environment and Objectives

The demonstration uses a browser-based training message board. The objectives are to trace untrusted input into browser execution, distinguish persistence from storage location, and match defenses to the interpreter or trust boundary involved.

## Demonstration Reviewed

1. The instructor submits an ordinary status message and reloads the page to show that the message persists.
2. The instructor submits image markup with a deliberately invalid image source and a JavaScript error handler that displays an alert.
3. The lesson reports an alert after submission and another after reloading the page.
4. The visible message appears as a broken image, illustrating that active content need not appear as a script block.

The demonstration is confined to an educational target. No destructive commands, reverse-shell instructions, credential collection, or challenge-answer payload is reproduced.

## Evidence and Interpretation

| Reported behavior | Supported conclusion | Not established |
| --- | --- | --- |
| Message remains after reload | Content persists in that browser context | Server-side database storage |
| Alert after submission | Submitted content reaches an executable browser context | Account takeover or data theft |
| Alert after reload | Persisted content is rendered unsafely again | Impact on another user or browser |

Stored content can originate in browser storage as well as server-side storage. Persistence and DOM-based execution describe different dimensions and can coexist. A reload in the same browser does not establish cross-user impact. No source code, storage inspection, or network trace was supplied with the lesson.

## Vulnerability Concepts Reviewed

- **Command injection:** Untrusted input changes an operating-system command. Prefer APIs that avoid a shell and enforce validated arguments and least privilege; stripping text after a particular domain suffix is not a reliable defense.
- **Code injection:** Untrusted input becomes executable application code. Its consequences can extend to the operating system under the application's permissions, so it is not inherently confined to application behavior.
- **SQL injection:** Input changes query structure. Parameterized queries separate data from SQL syntax.
- **XSS:** Untrusted content reaches an executable browser context. Reflected or stored delivery and client-side DOM processing can overlap. The presence of a particular JavaScript property alone does not establish the subtype.
- **CSRF:** A browser is induced to send an unwanted authenticated request. This is distinct from code injection and does not require guessing or stealing a session token.
- **SSRF:** An application makes attacker-influenced requests across an unintended trust boundary.
- **Unsafe deserialization:** Processing untrusted serialized objects can trigger dangerous behavior, depending on the serializer, types, and application logic; not every structured-data parser executes code.

Only the XSS training example was demonstrated. The other categories were conceptual discussion.

## Defensive Analysis

For XSS, apply context-appropriate output encoding, use safe text-rendering APIs, and use a maintained HTML sanitizer when rich content is genuinely required. Content Security Policy is defense in depth, not a replacement for safe rendering. These recommendations follow [OWASP's XSS prevention guidance](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).

For CSRF, use framework-supported anti-CSRF controls, validate tokens for relevant requests, and consider origin checks and appropriate SameSite cookie settings. Session authentication alone does not establish user intent. See [OWASP's CSRF prevention guidance](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).

For a follow-up XSS remediation exercise, verify that ordinary messages still render correctly, malicious markup is treated safely, previously persisted content is handled, and behavior is checked after reload. These are proposed validation steps, not completed fixes.

## Results and Limitations

The transcript reports repeatable alert execution in the training browser after submission and reload. No independent run, original screenshots, remediation, regression tests, server-side persistence evidence, or cross-user impact was verified. An alert demonstrates script execution in that context, not a complete exploitation chain.

## Skills and Takeaways

- Trace input, persistence, rendering, and execution boundaries.
- Distinguish injection from request forgery and avoid overclassification from payload keywords.
- Match defenses to context rather than relying on generic input sanitization.
- Bound findings by the available evidence.

## Sources

- User-supplied injection-vulnerabilities lesson, summarized in original language.
- [Google XSS Game, Level 2](https://xss-game.appspot.com/level2) — intentionally vulnerable training environment.
- [OWASP: XSS Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- [OWASP: CSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
