# Assessing the Impact of Web Application Vulnerabilities

## Overview

This lab assessed how SQL injection vulnerabilities can affect the confidentiality and integrity of a database-backed web application. In an isolated environment, I reviewed the application's database structure, mapped user-controlled parameters to server-side queries, tested catalog filtering, and evaluated authentication handling.

The assessment demonstrated that unsafely constructed SQL queries could expose records outside the intended scope and allow authentication to be bypassed. This documentation focuses on defensive findings and remediation. Credentials, internal addresses, challenge answers, exploit payloads, and proprietary course instructions are intentionally excluded.

## Lab Environment

- Microsoft Hyper-V
- Isolated Linux security-testing virtual machine
- Apache HTTP Server
- MySQL database
- PHP-based storefront application
- phpMyAdmin database-management interface
- Web browser and developer inspection tools

## Objectives

- Understand the database structure supporting a web application.
- Identify where user-controlled input influences SQL queries.
- Test whether a catalog parameter enforces its intended record scope.
- Evaluate whether the authentication form safely handles input.
- Assess the confidentiality, integrity, and authorization impact of SQL injection.
- Recommend secure development and defense-in-depth controls.

## Reviewing the Database Structure

Before testing the application, I examined the relational database through phpMyAdmin. The schema separated product categories, product records, and user accounts into related tables. Reviewing primary keys, category relationships, product attributes, and account permissions provided context for evaluating the application's queries.

This step also exposed an important risk: administrative database tools and tables can reveal sensitive account and application data. Access to these systems should be restricted to authorized administrators over protected management paths.

## Assessing Application Functionality

I established a functional baseline by navigating the storefront and reviewing how catalog pages filtered products by category. The application exposed the selected category through a user-controlled request parameter and displayed the resulting SQL query.

Understanding intended behavior was essential before testing. A reliable security assessment compares the expected output with the result produced after controlled input manipulation.

## Catalog Query Injection

I modified the category parameter with a basic Boolean-based injection in the isolated lab. The application accepted the input as part of the SQL statement rather than treating it strictly as data. As a result, the filter condition evaluated broadly and returned products from every category instead of only the selected category.

This demonstrated a loss of data confidentiality and query-scope enforcement. In a real application, similar flaws could expose records that a user was not authorized to view and could potentially support more damaging database operations depending on account privileges and query construction.

The exact injection string is intentionally omitted.

## Authentication Bypass Assessment

I next tested the sign-in form with invalid credentials to establish that normal authentication failed. After reviewing how the form values were incorporated into the server-side query, I supplied a controlled Boolean-based input that altered the query's logic.

The application authenticated a different account even though no valid password had been provided. Because the modified condition matched database rows broadly, the application selected an unintended account with elevated application permissions.

This finding demonstrated multiple security failures:

- User input was concatenated directly into a SQL statement.
- Authentication logic trusted the query result without securely verifying a password.
- The query could return an account other than the one requested.
- An injection flaw crossed an authorization boundary and exposed privileged functionality.

No account names, passwords, or injection payloads are published here.

## Potential Impact

A successful SQL injection vulnerability can affect all three elements of the CIA triad:

- **Confidentiality:** Unauthorized disclosure of user, product, financial, or operational data.
- **Integrity:** Modification, insertion, or deletion of application records.
- **Availability:** Destructive queries, resource exhaustion, or disruption of database-backed services.

Additional consequences may include authentication bypass, privilege escalation within the application, credential exposure, administrative takeover, persistent data manipulation, and use of the database server as a path to other systems.

The actual impact depends on the database account's permissions, available query functionality, error handling, network segmentation, and other defensive controls.

## Root Cause Analysis

The primary root cause was insecure query construction. User-controlled values were incorporated into SQL syntax without a safe separation between code and data. Weak authentication query logic increased the severity by allowing a broadly matching condition to produce a valid session.

Contributing issues included:

- Insufficient server-side input handling
- Excessive trust in client-supplied parameters
- Exposure of query details in application output
- Inadequate enforcement of authentication intent
- Potentially excessive database privileges
- Lack of compensating detection or blocking controls

## Recommended Remediation

### Secure Query Construction

- Use parameterized queries or prepared statements for every database operation.
- Keep SQL syntax separate from user-supplied values.
- Avoid dynamic query construction through string concatenation.
- Use well-maintained data-access libraries and safe framework patterns.

### Input and Application Controls

- Validate input on the server against expected type, length, range, and format.
- Treat client-side validation as a usability feature, not a security boundary.
- Enforce authorization independently for every requested object and action.
- Return generic errors to users while recording detailed diagnostics securely.
- Remove SQL statements, stack traces, and internal implementation details from production responses.

### Authentication Security

- Retrieve the intended account through a parameterized identifier lookup.
- Store passwords with a modern adaptive hashing algorithm and unique salts.
- Compare passwords using trusted authentication libraries.
- Prevent a multirow query result from becoming an authenticated session.
- Apply multi-factor authentication to privileged accounts where appropriate.

### Database and Infrastructure Controls

- Grant the application database account only the permissions it requires.
- Separate administrative and application database credentials.
- Restrict database access through network segmentation and host controls.
- Protect database-management interfaces behind dedicated administrative access.
- Monitor anomalous queries, repeated failures, and unexpected data-volume responses.
- Consider a web application firewall as a supplemental control, not a replacement for secure code.

### Secure Development Lifecycle

- Include threat modeling and security requirements during design.
- Use peer review and automated static analysis to detect unsafe query construction.
- Perform dynamic application security testing before release.
- Add negative tests for injection and authorization bypasses.
- Track remediation through verification and regression testing.

## Validation Strategy

After remediation, the same test cases should be repeated to confirm that:

- Catalog parameters accept only expected values.
- Altered input is processed as data rather than executable SQL.
- Catalog results remain limited to the authorized category.
- Invalid passwords cannot create an authenticated session.
- The requested identity always matches the authenticated identity.
- Application responses do not disclose SQL statements or database errors.
- Security logs capture suspicious input without storing sensitive values unnecessarily.

## Security Concepts Demonstrated

- SQL injection
- Boolean-based input manipulation
- Authentication bypass
- Broken authorization boundaries
- Input validation
- Parameterized queries
- Least-privilege database access
- Secure error handling
- Defense in depth
- Secure software development lifecycle
- Vulnerability impact assessment
- Remediation verification

## Results

- Mapped the application's database tables and relationships.
- Established expected catalog and authentication behavior.
- Identified a user-controlled catalog parameter that influenced SQL logic.
- Demonstrated unintended disclosure of records outside the selected category.
- Confirmed that insecure authentication query construction allowed a login bypass.
- Assessed the resulting confidentiality, integrity, and privilege risks.
- Developed layered remediation and validation recommendations.
- Completed the assessment without publishing sensitive lab details or reusable exploit strings.

## Key Takeaways

- User input must never be allowed to alter the structure of a database query.
- Parameterized queries are the primary defense against SQL injection.
- Input validation supports secure design but does not replace prepared statements.
- Authentication and authorization must be enforced independently and fail securely.
- Database least privilege can substantially reduce the impact of an application vulnerability.
- Verbose query output helps attackers understand application internals and should be disabled in production.
- Vulnerability findings should connect technical behavior to business impact and verified remediation.

## Skills Demonstrated

- Web application security assessment
- SQL injection analysis
- Database schema review
- Authentication and authorization testing
- Vulnerability impact analysis
- Root cause analysis
- Secure query design
- Application threat modeling
- Remediation planning
- Security control validation
- Secure development lifecycle principles
- Professional cybersecurity documentation
