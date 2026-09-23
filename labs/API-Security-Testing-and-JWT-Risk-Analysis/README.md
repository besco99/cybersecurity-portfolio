# API Security Testing and JWT Risk Analysis

## Overview

This authorized lab explored common API security risks in modern web applications. Using Postman against a deliberately vulnerable application in an isolated environment, I examined request construction, parameters, headers, environment variables, response data, and repeatable test validation. The hands-on exercise confirmed that untrusted API input could affect backend query behavior.

The lab also covered JSON Web Token (JWT) structure and the security consequences of weak signature verification, unsafe algorithm handling, excessive token lifetime, and authorization decisions based on unverified claims. JWT manipulation was analyzed conceptually; the practical demonstration focused on API request testing and injection behavior.

Exact payloads, tokens, target addresses, credentials, endpoint paths, and reusable abuse instructions are intentionally excluded.

## Lab Environment

- Isolated Kali Linux virtual machine
- OWASP Juice Shop training application
- Locally hosted API
- Postman API development and testing platform
- Saved collections and environment variables
- Automated response-validation scripts

## Objectives

- Explain how API abuse differs from intended application use.
- Build and inspect controlled API requests with Postman.
- Evaluate how parameters influence backend processing.
- Validate suspected injection behavior through API responses.
- Describe JWT structure and appropriate trust boundaries.
- Identify JWT validation and authorization weaknesses.
- Recommend preventive, detective, and operational controls for APIs.

## Authorization and Scope

All activity was performed against a purpose-built vulnerable application running in an isolated lab. No production systems, public APIs, third-party services, real accounts, or personal data were involved.

API security testing can expose or modify data and may disrupt services. It should be performed only with explicit authorization, defined endpoints, approved test cases, rate limits, test identities, and documented rules of engagement.

## Understanding API Abuse

API abuse occurs when a client uses valid or malformed requests in ways the service owner did not intend. An API may expose excessive data, trust client-controlled identifiers, omit object-level authorization, accept unsafe input, or permit high-volume automation without adequate controls.

Potential outcomes include:

- Unauthorized access to records or functions
- Large-scale data collection
- Authentication or authorization bypass
- Injection into downstream interpreters
- Resource exhaustion and denial of service
- Business-logic abuse
- Account takeover or privilege escalation
- Privacy and regulatory impact

Because an API request can be syntactically valid while still violating business intent, authentication alone does not prevent abuse.

## Working with Postman

Postman provided a controlled interface for composing requests and inspecting raw API behavior. I reviewed:

- HTTP methods such as GET, POST, PUT, PATCH, and DELETE
- Query parameters and request bodies
- Request and response headers
- Cookies and authentication fields
- Environment variables for reusable configuration
- Collections for organizing repeatable test cases
- Scripts for automated response validation

Environment variables reduced duplication and helped separate reusable requests from environment-specific configuration. Sensitive values should be stored in protected secret mechanisms, excluded from exported collections, and rotated if exposed.

## API Injection Assessment

The practical exercise targeted a search-related API parameter in the training application. I established a normal response, changed the parameter with approved test input, and compared the returned status, structure, record count, and content.

The altered request caused the backend to return data inconsistent with the intended search behavior. An automated Postman test evaluated the response and confirmed that the input influenced query logic. This supported a finding of unsafe server-side query construction.

No copy-ready injection string or endpoint is published here.

## Repeatable Test Design

A defensible API test should include:

1. A documented baseline request and expected response.
2. One controlled change at a time.
3. Assertions for status code, schema, headers, timing, and result size.
4. Clear pass/fail criteria tied to security requirements.
5. Evidence that avoids retaining unnecessary secrets or personal data.
6. Cleanup steps for any created or modified records.

Automated assertions improve consistency, but unexpected responses still require analyst review. A passing script does not by itself establish exploitability or business impact.

## JSON Web Token Fundamentals

A JWT commonly contains three encoded components:

- **Header:** identifies the token type and signing algorithm.
- **Payload:** contains claims about the subject, issuer, audience, role, and lifetime.
- **Signature:** provides integrity and authenticity when created and verified correctly.

JWT headers and payloads are generally Base64URL-encoded, not encrypted. Anyone possessing a token may be able to read its claims. Secrets and unnecessary personal information therefore should not be placed in an ordinary signed JWT.

The server must verify the signature and validate the token's claims before trusting it. Decoding a token is not the same as validating it.

## JWT Risk Analysis

Weak JWT implementations may accept modified tokens or grant access based on untrusted claims. Important failure modes include:

- Skipping signature verification
- Accepting an unexpected or attacker-selected algorithm
- Using weak or exposed signing keys
- Failing to validate issuer or audience
- Ignoring expiration or not-before constraints
- Trusting a role claim without server-side authorization
- Reusing tokens for excessive periods
- Exposing tokens in URLs, logs, browser storage, or source repositories
- Failing to revoke or rotate compromised credentials

Changing a visible claim does not create a valid token when signature verification is implemented correctly. The security failure occurs when the service accepts the tampered or improperly issued token.

## Secure JWT Implementation

- Use a maintained identity and JWT library rather than custom cryptography.
- Pin an explicit allowlist of approved algorithms on the server.
- Verify every signature before consuming claims.
- Validate issuer, audience, subject, expiration, not-before, and issued-at values as applicable.
- Use strong keys of the correct type and length.
- Protect keys in a managed key or secrets system and rotate them safely.
- Use short-lived access tokens and controlled refresh-token rotation.
- Minimize claims and never place secrets in unencrypted tokens.
- Enforce authorization on the server for every resource and action.
- Design revocation or rapid-containment mechanisms for higher-risk environments.
- Prevent tokens from appearing in URLs, logs, analytics, or error messages.

Symmetric and asymmetric signing algorithms can both be appropriate when implemented correctly. The selection should match the trust model, key-distribution requirements, and library guidance.

## API Security Controls

### Input and Query Safety

- Use parameterized queries or prepared statements.
- Validate request types, lengths, ranges, formats, and schemas on the server.
- Reject unexpected properties and ambiguous encodings.
- Apply output encoding when data enters another execution context.
- Avoid constructing interpreter commands from client input.

### Authentication and Authorization

- Authenticate clients with protocols appropriate to the use case.
- Enforce object-level and function-level authorization on every request.
- Do not trust client-supplied roles, prices, ownership identifiers, or account states.
- Use least-privilege service identities and scoped permissions.
- Require stronger controls for administrative and destructive actions.

### Abuse Prevention

- Apply rate limits based on identity, resource, behavior, and risk.
- Use pagination and response-size limits.
- Set request size, execution time, and concurrency limits.
- Detect scraping and automation without relying only on IP addresses.
- Maintain an accurate inventory and retire obsolete API versions.
- Avoid returning more fields or records than the client requires.

## Detection Opportunities

Potential indicators of API abuse include:

- High request volume or unusual request velocity
- Sequential access to many object identifiers
- Repeated authorization failures across resources
- Injection metacharacters in unexpected parameters
- Abnormal response sizes or record counts
- Unexpected HTTP methods or content types
- Tokens with invalid signatures, algorithms, issuers, or audiences
- Expired tokens used repeatedly
- One token used from implausible locations or clients
- Sudden access to privileged endpoints
- Database errors correlated with crafted API input

API gateway, application, identity-provider, database, web application firewall, and endpoint telemetry should be correlated during investigation.

## Error Handling and Logging

- Return consistent errors without stack traces, queries, keys, or internal paths.
- Assign correlation identifiers for troubleshooting.
- Record authentication and authorization decisions without logging raw tokens.
- Redact secrets from application, proxy, monitoring, and test logs.
- Log security-relevant request metadata in accordance with privacy requirements.
- Alert on repeated malformed requests and high-confidence validation failures.
- Protect logs from unauthorized alteration and define appropriate retention.

## Validation After Remediation

- Repeat the baseline and controlled negative tests.
- Confirm that malformed input cannot alter backend query structure.
- Verify object-level authorization with multiple test identities.
- Test every supported HTTP method and content type.
- Confirm invalid, modified, expired, and incorrectly scoped tokens are rejected.
- Verify that token claims are not trusted without server-side authorization.
- Test rate limits and response minimization.
- Review exported Postman collections for secrets before sharing.
- Add automated API security tests to the development pipeline.

## Security Concepts Demonstrated

- API abuse
- Injection testing
- HTTP request and response analysis
- Postman collections and environments
- Automated security assertions
- JWT encoding and signing
- Signature and claim validation
- Object-level and function-level authorization
- Rate limiting
- Least privilege
- Secure error handling
- Detection engineering

## Results

- Built and inspected controlled API requests in Postman.
- Used environment variables and collections to improve repeatability.
- Established baseline API behavior before modifying input.
- Confirmed that a search parameter affected backend query logic.
- Automated part of the response-validation process.
- Analyzed JWT structure and common validation weaknesses.
- Developed preventive, detective, and retesting recommendations.

## Key Takeaways

- A valid API request may still represent unauthorized or abusive behavior.
- Server-side validation and parameterized queries are essential for untrusted input.
- JWT payloads are readable unless separately encrypted; encoded does not mean secret.
- JWT security depends on signature verification, claim validation, key management, and authorization.
- Postman supports reproducible security testing when collections and secrets are handled carefully.
- Rate limits, least privilege, logging, and response minimization reduce the impact of API abuse.

## Skills Demonstrated

- API request construction and analysis
- Postman collection and environment management
- Controlled injection validation
- Automated response testing
- JWT security analysis
- Authentication and authorization review
- API abuse-case development
- Secure API remediation planning
- Detection and logging recommendations
- Ethical security-testing practices
- Professional cybersecurity documentation
