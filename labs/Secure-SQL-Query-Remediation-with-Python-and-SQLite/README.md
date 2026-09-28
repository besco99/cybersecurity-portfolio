# Secure SQL Query Remediation with Python and SQLite

## Overview

This authorized secure-coding lab demonstrated how unsafe SQL query construction can create authentication bypass and sensitive-data disclosure. A deliberately vulnerable Python application used formatted user input to build a SQLite login query. Controlled test input altered the query's logic and returned records beyond the intended account. I then replaced the vulnerable construction with a parameterized query and verified that the same input was treated as literal data rather than executable SQL.

The exercise emphasized root-cause remediation and regression testing. This portfolio entry does not include source code, SQL payloads, usernames, passwords, personal identifiers, database records, filenames, commands, or proprietary lesson wording.

## Lab Environment

- Python application
- SQLite in-memory database
- Visual Studio Code
- Deliberately vulnerable login function
- Parameterized secure-login implementation
- Synthetic user and sensitive-data records
- Controlled positive and negative test cases

## Objectives

- Identify unsafe dynamic SQL construction.
- Explain how user input can become executable query logic.
- Demonstrate authentication bypass and excessive data return safely.
- Replace formatted SQL with parameter binding.
- Verify that valid credentials still work after remediation.
- Confirm that invalid and malicious input fail safely.
- Recommend password-storage, data-minimization, and logging improvements.

## Authorization and Scope

All testing was performed against a small purpose-built Python application with an in-memory database and artificial records. No production systems, public services, real credentials, customer information, or personal data were involved.

Injection testing should only be conducted on explicitly authorized code and systems. Test data should be synthetic, results should be protected, and payloads should not be copied into public documentation.

## Application Design

The demonstration application created a temporary users table and inserted sample accounts each time it ran. The table contained authentication fields and a simulated sensitive identifier to demonstrate the confidentiality impact of excessive query results.

The in-memory design made the lab repeatable and prevented persistent storage of test records. It did not make the vulnerable query safe; SQL injection depends on how commands are constructed, not whether the database is temporary or persistent.

## Vulnerable Query Construction

The original login function inserted the supplied username and password directly into a SQL statement through string formatting. This allowed input to change the query's syntax and logical conditions.

The function also printed the complete generated query and returned every column from matching records. These behaviors increased exposure by revealing query structure, authentication values, and fields that the login decision did not need.

## Controlled Vulnerability Validation

The lab first established normal behavior with approved test credentials. A successful login returned the expected record for one account.

Next, controlled injection input altered the query's logical evaluation. Instead of verifying one username and password combination, the database returned multiple rows, including privileged and sensitive synthetic records. This confirmed both authentication bypass and excessive data disclosure.

The exact payload and returned records are intentionally omitted.

## Root Cause Analysis

The primary flaw was mixing SQL instructions and untrusted values in one dynamically constructed string. The database parser could not distinguish intended application logic from attacker-controlled syntax.

Contributing weaknesses included:

- Direct string formatting of user input
- Returning all columns from the user table
- Combining authentication decisions with sensitive profile retrieval
- Printing complete queries and data
- Comparing passwords in a form suitable for direct database queries
- Missing server-side input constraints
- Insufficient negative security tests

## Parameterized Query Remediation

The secure implementation defined the SQL structure separately and supplied the username and password through the database driver's parameter-binding interface. Placeholders marked where values belonged, while the driver transmitted those values as data.

As a result, quotation marks, operators, and other special characters in the supplied value no longer changed the SQL statement's structure. The database compared the entire supplied value literally and rejected the controlled injection input.

Parameterized queries are the primary remediation for SQL injection. Escaping, filtering specific characters, or attempting to detect known payload strings is not an adequate replacement.

## Remediation Validation

The updated function was tested with three categories:

1. The previously successful injection attempt, which was rejected.
2. Valid synthetic credentials, which continued to authenticate correctly.
3. An incorrect password, which failed without exposing record details.

This sequence confirmed that the remediation blocked the vulnerability without breaking intended login behavior.

## Input Validation as Defense in Depth

Server-side validation should still enforce expected input length, type, encoding, and format. It improves reliability and reduces ambiguous input, but it should complement parameterized queries rather than replace them.

For example, an application can reject an excessively long username while still binding the accepted value safely. Character allowlists should reflect legitimate business requirements and should not be designed merely to remove obvious SQL metacharacters.

## Password Storage

A production login flow should not query for a row by matching a plaintext password. A safer design is:

1. Retrieve the minimum account-authentication record by a normalized identifier.
2. Verify the supplied password using a dedicated password-hashing library.
3. Store only a salted, adaptive password hash.
4. Apply a current work factor and controlled migration process.
5. Return a uniform authentication result.

Appropriate password-hashing choices include Argon2id, bcrypt, or scrypt as supported by the platform and organizational standards. General-purpose fast hashes are not suitable for password storage.

## Data Minimization

The vulnerable query requested every field from matching rows, including data unrelated to authentication. A secure design should:

- Select only the fields required for the current operation.
- Separate authentication data from sensitive profile data.
- Apply field-level authorization before returning personal information.
- Avoid logging sensitive identifiers or password-related values.
- Mask data in administrative and support interfaces.
- Define retention and deletion requirements.

Reducing accessible data limits the impact of both injection and ordinary authorization failures.

## Error Handling and Logging

The application should not display SQL statements, stack traces, database paths, or raw records to users. Secure handling includes:

- A generic authentication failure message
- Structured server-side errors with correlation identifiers
- Redaction of passwords, tokens, and sensitive fields
- Access-controlled and tamper-resistant logs
- Alerts for repeated malformed input and query failures
- Sufficient context to investigate without retaining unnecessary personal data

Authentication responses should not reveal whether the username or password was incorrect.

## Database Least Privilege

Even with parameterized queries, the application database identity should have only the permissions required for its functions. Defensive measures include:

- Separate read, write, migration, and administrative roles.
- Restrict access to only required tables and operations.
- Prevent the web application from managing database users or schemas.
- Store database secrets in an approved secrets manager.
- Rotate credentials and monitor unusual database actions.
- Use views or stored interfaces to reduce direct access to sensitive fields where appropriate.

Least privilege reduces impact if application logic is bypassed.

## Secure Development Lifecycle

- Establish secure data-access patterns and reusable components.
- Ban unsafe string-based query construction through coding standards.
- Review authentication and authorization code carefully.
- Use static analysis to identify tainted data entering query functions.
- Add dynamic application security testing in controlled environments.
- Include dependency and secret scanning in continuous integration.
- Require tests for injection, authorization, and excessive data exposure.
- Track remediation through code review and release validation.

Frameworks and object-relational mappers can reduce risk when used correctly, but unsafe raw-query features can reintroduce injection.

## Regression Test Strategy

A durable test suite should include:

- Valid credentials
- Incorrect passwords
- Unknown users
- Empty and null values
- Maximum accepted lengths
- Unicode and unexpected encodings
- Quotation marks and control characters
- Previously successful injection cases
- Multiple-row and no-row results
- Database exceptions and unavailable-service conditions
- Verification that sensitive fields are never returned by login functions

Tests should assert both security behavior and normal application functionality.

## Detection Opportunities

Potential indicators of SQL injection include:

- SQL operators or metacharacters in unexpected fields
- Repeated authentication attempts with changing syntax
- Database parser or type-conversion errors
- Unusually large result sets from login workflows
- Authentication success without the expected account lookup
- One request retrieving multiple user records
- Web application firewall or runtime-protection alerts
- Database access outside normal prepared-statement patterns

Application, database, identity, web gateway, and endpoint telemetry should be correlated.

## Potential Impact

If left unresolved, the vulnerability could enable:

- Authentication bypass
- Exposure of sensitive records
- Access to privileged accounts
- Modification or deletion of data
- Broader database compromise
- Privacy and regulatory violations
- Chaining with application authorization flaws

The lab demonstrated read-oriented disclosure and bypass in a controlled application; it did not attempt destructive database actions.

## Security Concepts Demonstrated

- SQL injection
- Unsafe string formatting
- Authentication bypass
- Sensitive-data exposure
- Parameterized queries
- Password hashing
- Data minimization
- Least-privilege database access
- Secure error handling
- Positive and negative testing
- Static and dynamic analysis
- Regression testing

## Results

- Reviewed a vulnerable Python and SQLite login implementation.
- Established normal authentication behavior.
- Demonstrated that crafted input could alter query logic.
- Confirmed unauthorized return of multiple synthetic records.
- Replaced dynamic SQL construction with parameter binding.
- Verified rejection of the original injection case.
- Confirmed valid login behavior remained functional.
- Confirmed invalid credentials failed safely.
- Developed additional password, data, logging, and least-privilege recommendations.

## Key Takeaways

- SQL injection occurs when untrusted data is interpreted as query syntax.
- Parameterized queries separate SQL structure from supplied values and address the root cause.
- Input validation is defense in depth, not a substitute for parameter binding.
- Login functions should return the minimum information needed to establish identity.
- Passwords require dedicated salted, adaptive hashing rather than direct database comparison.
- A remediation is not complete until malicious, valid, and invalid test cases all pass.
- Secure query patterns should be enforced through reusable code, review, and automated tests.

## Skills Demonstrated

- Python secure-code review
- SQLite query analysis
- SQL-injection validation
- Root-cause analysis
- Parameterized-query remediation
- Authentication-flow testing
- Password-storage recommendations
- Data-minimization design
- Database least-privilege analysis
- Regression-test planning
- Secure development lifecycle integration
- Professional cybersecurity documentation
