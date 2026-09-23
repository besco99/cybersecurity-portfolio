# SQL Injection and Authentication Bypass Analysis

## Overview

This authorized lab examined the security impact of SQL injection in a deliberately vulnerable e-commerce application. I reviewed the relationship between the web front end and its database, identified unsafe handling of URL and login-form input, demonstrated unauthorized query manipulation, and analyzed an authentication bypass caused by dynamically constructed SQL statements.

The exercise also revealed insecure plaintext password storage and verbose application errors. This portfolio entry focuses on defensive analysis and remediation; exact payloads, credentials, account names, target addresses, database names, file-system paths, and copy-ready exploitation steps are intentionally excluded.

## Lab Environment

- Isolated Linux training environment
- Apache web server
- MySQL database server
- PHP-based e-commerce application
- Browser-based database administration interface
- Deliberately vulnerable catalog and authentication functions

## Objectives

- Examine how a web application retrieves data from a relational database.
- Identify sensitive information stored insecurely in database tables.
- Assess URL parameters and form fields as untrusted input.
- Demonstrate the impact of SQL injection in an authorized environment.
- Analyze how injection can undermine authentication and authorization.
- Recommend secure query, credential-storage, error-handling, and monitoring controls.

## Authorization and Scope

All testing was performed against a purpose-built vulnerable application in an isolated lab. No production systems, public websites, third-party services, real credentials, or personal information were involved.

SQL-injection testing can expose or alter data and disrupt applications. It must be limited to explicitly authorized targets, test accounts, approved techniques, and documented rules of engagement.

## Reviewing the Application Architecture

The application used a conventional three-tier flow:

1. A browser submitted requests to the web application.
2. Server-side PHP code interpreted request parameters and constructed database queries.
3. MySQL returned records used to build catalog and authentication responses.

I reviewed representative tables for product categories, inventory, and user accounts. This established how application input influenced database queries and highlighted the sensitivity of the information available to the web tier.

## Database Security Findings

The user table stored passwords in readable form. Plaintext credential storage creates severe risk because anyone who obtains database access can immediately recover every password. It also increases the impact of backups, logs, exports, administrator compromise, and SQL injection.

Passwords should be processed with a password-specific, salted, adaptive hashing algorithm such as Argon2id, bcrypt, or scrypt. Applications should use established password-hashing libraries, unique salts, an appropriate work factor, and a controlled migration plan for legacy credentials.

Database administration interfaces also require strong protection. They should not be publicly exposed and should be restricted through network controls, strong authentication, least privilege, encryption, logging, and timely patching.

## Parameter and Query Analysis

The catalog accepted a category identifier through a URL parameter. Modifying the identifier changed the records returned by the application, which was expected functionality, but the server inserted user-controlled data into a query without safe parameter binding.

This created a trust-boundary failure: the application treated request data as SQL syntax rather than strictly as a value. A controlled Boolean condition altered the query logic and caused the page to return records outside the intended category.

The result confirmed that client-side restrictions and normal page navigation did not protect the database. Security controls must be enforced on the server for every request.

## Authentication Bypass Analysis

The sign-in form was affected by the same unsafe query-construction pattern. Specially structured input changed the meaning of the authentication query, allowing the application to accept a login without validating the intended password.

The resulting session did not reliably correspond to the requested identity and instead inherited an unintended database record. This demonstrated both authentication failure and potential privilege escalation: an injection flaw in a login workflow may grant access as another user, including a privileged account.

The exact input used in the controlled validation is not published here.

## Error Handling and Information Disclosure

The failed portion of the test produced a verbose database error containing implementation details. Detailed errors can reveal:

- Server-side technologies
- Query behavior
- Database expectations
- Application file locations
- Source filenames and line numbers
- Clues that help refine later attacks

Users should receive a generic error message and a correlation identifier. Detailed diagnostic information belongs in protected server-side logs with appropriate access control, retention, and monitoring.

## Root Cause Analysis

The primary root cause was dynamic SQL construction that combined commands with untrusted input. The database could not reliably distinguish intended query structure from user-supplied values.

Contributing weaknesses included:

- Missing parameterized queries
- Inadequate server-side validation
- Plaintext password storage
- Excessive database privileges
- Verbose error disclosure
- Authentication logic dependent on unsafe query results
- Insufficient separation between application and administrative functions

## Potential Impact

Depending on database permissions and application design, SQL injection can enable:

- Authentication bypass
- Unauthorized account access
- Disclosure of customer or business data
- Modification or deletion of records
- Privilege escalation
- Exposure of password data
- Administrative-function access
- Application disruption or database corruption
- Further compromise through chained vulnerabilities

Even a read-only injection can create a serious confidentiality breach.

## Secure Remediation

### Query Safety

- Use parameterized queries or prepared statements for all database operations.
- Keep SQL instructions separate from user-supplied values.
- Use a maintained data-access framework correctly and avoid unsafe raw-query construction.
- Apply allowlist validation for expected types, lengths, ranges, and formats.
- Never rely on removing quotation marks or other individual characters as the primary defense.
- Avoid exposing unnecessary internal identifiers when stronger access-control designs are appropriate.

Input validation improves data quality and reduces attack surface, but parameter binding is the essential control that prevents input from becoming executable SQL syntax.

### Authentication and Password Storage

- Store passwords using a salted, adaptive password-hashing algorithm.
- Use constant-time library functions for password verification.
- Regenerate session identifiers after successful authentication.
- Enforce authorization independently on every protected request.
- Add multifactor authentication for privileged accounts.
- Rate-limit login attempts and monitor anomalous failures.
- Prevent user enumeration through consistent responses.

### Database and Infrastructure Controls

- Grant the application database account only the permissions it requires.
- Separate read, write, and administrative duties where practical.
- Restrict database and administration interfaces to authorized management paths.
- Store database secrets in an approved secrets-management system.
- Encrypt database connections where traffic crosses trust boundaries.
- Maintain tested backups and database audit logging.
- Patch the operating system, web server, runtime, libraries, and database platform.

### Error Handling

- Return generic user-facing errors.
- Log technical details only on trusted systems.
- Remove stack traces, queries, and local paths from HTTP responses.
- Attach correlation identifiers for support and incident investigation.
- Alert on repeated database syntax errors and abnormal query failures.

## Detection Opportunities

Potential indicators of SQL-injection activity include:

- SQL metacharacters or Boolean expressions in unexpected input fields
- Repeated requests that vary one parameter at a time
- Sudden increases in database syntax or type-conversion errors
- Login success following repeated malformed authentication attempts
- Atypical result-set sizes or response lengths
- Requests that trigger records outside the expected category or tenant
- Unusual database access by the application's service account
- Web application firewall or runtime-protection alerts
- Database queries inconsistent with normal application workflows

Web, application, authentication, database, and endpoint telemetry should be correlated before determining severity.

## Validation After Remediation

- Retest the original catalog and authentication cases with approved test input.
- Confirm that untrusted values cannot change query structure.
- Verify that malformed input produces a safe, generic response.
- Test authorization separately from authentication.
- Confirm that passwords are migrated to approved hashes.
- Review database privileges and verify denied operations.
- Add automated security tests for parameters and login workflows.
- Use code review, static analysis, dynamic testing, and dependency scanning in the development lifecycle.

## Security Concepts Demonstrated

- SQL injection
- Authentication bypass
- Broken access control
- Plaintext credential exposure
- Parameterized queries
- Server-side input validation
- Password hashing
- Least-privilege database access
- Secure error handling
- Defense in depth
- Security logging and monitoring

## Results

- Mapped the relationship between the web application and database backend.
- Identified plaintext password storage as a critical design weakness.
- Confirmed that a URL parameter could alter database query logic.
- Demonstrated an authentication bypass in the controlled lab.
- Assessed the possibility of unintended privileged access.
- Identified verbose errors that disclosed internal application details.
- Developed remediation, validation, and detection recommendations.

## Key Takeaways

- Untrusted input must never be combined directly with SQL instructions.
- Parameterized queries are the primary defense against SQL injection.
- Input validation is valuable but does not replace safe query construction.
- Authentication success is meaningless without correct identity binding and authorization.
- Passwords must be stored with modern password-specific hashing, never in plaintext.
- Least privilege, safe errors, monitoring, and layered controls reduce both likelihood and impact.

## Skills Demonstrated

- Web application architecture analysis
- Relational database review
- SQL-injection validation
- Authentication and authorization assessment
- Secure password-storage analysis
- Root cause and impact analysis
- Parameterized-query remediation
- Database least-privilege recommendations
- Web and database detection strategy
- Ethical security-testing practices
- Professional cybersecurity documentation
