# Simulated Password Cracking

## Overview

This lab demonstrated how weak passwords can be identified during an authorized security assessment. In an isolated Kali Linux environment, I compared offline dictionary-based hash recovery with controlled online credential testing against a local FTP service.

The exercise used Hashcat, John the Ripper, Hydra, and Medusa to illustrate different password-auditing workflows. This documentation focuses on defensive lessons and risk reduction. Passwords, usernames, hash values, exact commands, wordlist paths, and reusable attack instructions are intentionally excluded.

## Lab Environment

- Isolated Kali Linux system
- Local test service
- Hashcat
- John the Ripper
- Hydra
- Medusa
- A commonly used breach-derived password dictionary
- Test-only MD5 password hash

## Objectives

- Distinguish offline password recovery from online authentication testing.
- Demonstrate why predictable passwords are vulnerable to dictionary attacks.
- Compare two offline password-auditing tools.
- Compare two network authentication-testing tools.
- Assess the defensive value of secure password storage and login protections.
- Translate test results into practical remediation recommendations.

## Authorization and Scope

All activity was limited to locally controlled test data and services in an authorized lab. Credential testing should be performed only with explicit permission, documented scope, defined rate limits, and safeguards against service disruption or unintended account lockout.

No real user credentials or third-party systems were involved.

## Password Dictionary Review

I reviewed a large breach-derived password dictionary included with the testing environment. The list contained millions of passwords observed in historical data exposures, including many short and predictable values.

This demonstrated why dictionary attacks are effective against weak passwords: attackers do not need to search every possible character combination when users repeatedly select common words, patterns, and previously compromised credentials.

## Creating a Controlled Test Hash

I generated an MD5 hash from a deliberately weak test password and saved only the hash for offline analysis. This provided a known, non-production target for comparing password-recovery tools.

MD5 is unsuitable for password storage because it is fast, unsalted by default, and designed for general-purpose hashing rather than resisting password guessing. Its speed allows an attacker to test large numbers of candidate passwords efficiently.

## Offline Recovery with Hashcat

I configured Hashcat for the correct hash type and a dictionary-based attack. The tool hashed each candidate from the selected wordlist and compared the result with the target hash.

The weak test password was recovered quickly because it appeared in the dictionary. This showed that storing a password as a fast cryptographic hash does not adequately protect it from offline recovery.

## Offline Recovery with John the Ripper

I repeated the same controlled recovery exercise with John the Ripper. After identifying the hash format and supplying the dictionary, John also recovered the weak test password.

Using two tools demonstrated that the vulnerability was not tool-specific. The underlying weakness was the predictable password combined with an unsuitable password-storage algorithm.

## Controlled Online Credential Testing

The lab then shifted from offline hash recovery to authorized online authentication testing against a local FTP service. I used Hydra and Medusa separately to evaluate the same controlled account with a password dictionary.

Both tools identified the intentionally weak credential. Unlike offline recovery, online testing generated authentication requests against a running service and was therefore constrained by network latency, service behavior, logging, rate limits, and potential lockout controls.

Exact target details, account names, credentials, and command syntax are not included.

## Offline vs. Online Password Attacks

| Characteristic | Offline recovery | Online testing |
| --- | --- | --- |
| Target | Captured or obtained password hash | Live authentication service |
| Network required | No | Yes |
| Speed | Often limited mainly by hardware and hash cost | Limited by latency and service controls |
| Visibility | May produce no authentication logs | Produces login attempts that can be monitored |
| Lockout risk | None for the original account | May trigger lockout or service disruption |
| Primary defense | Strong salted password hashing and strong passwords | MFA, rate limiting, lockout controls, monitoring, and strong passwords |

## Security Findings

- Predictable passwords can be recovered rapidly when they appear in common dictionaries.
- Fast legacy hashes provide inadequate resistance to offline password guessing.
- Exposed network services with weak credentials are vulnerable to automated login attempts.
- Multiple tools can exploit the same underlying password weakness.
- A successful password policy must address both password selection and password storage.
- Online attempts are more detectable, but detection alone does not prevent compromise.

## Recommended Mitigations

### Password Storage

- Use a modern adaptive password-hashing algorithm such as Argon2id, bcrypt, scrypt, or PBKDF2.
- Apply a unique, cryptographically random salt to every password.
- Select work factors appropriate for current hardware and review them periodically.
- Consider a securely managed pepper as an additional control where appropriate.
- Never store passwords with fast general-purpose hashes such as MD5 or SHA-1.

### Authentication Controls

- Require long, unique passwords and permit password-manager-generated values.
- Screen new passwords against known-compromised password datasets.
- Enforce multi-factor authentication, especially for administrative and remote access.
- Rate-limit repeated authentication attempts.
- Apply carefully designed lockout or progressive-delay controls.
- Disable unused accounts and insecure legacy services.
- Prefer encrypted protocols over plaintext authentication services.

### Monitoring and Response

- Alert on repeated failures, password spraying patterns, and unusual source behavior.
- Centralize authentication logs and retain them for investigation.
- Correlate failed attempts with successful logins and account changes.
- Establish response procedures for suspected credential compromise.
- Rotate exposed credentials and invalidate active sessions when compromise is confirmed.

## Security Concepts Demonstrated

- Password auditing
- Dictionary attacks
- Offline hash recovery
- Online authentication testing
- Weak password identification
- Secure password storage
- Adaptive password hashing
- Salting and key stretching
- Multi-factor authentication
- Rate limiting and account lockout
- Authentication monitoring
- Authorized penetration-testing scope

## Results

- Reviewed a breach-derived dictionary for common password patterns.
- Created a controlled test hash from a deliberately weak password.
- Recovered the test password with Hashcat.
- Recovered the same test password with John the Ripper.
- Identified a weak local FTP credential with Hydra.
- Confirmed the result independently with Medusa.
- Compared offline and online password-testing risks and defenses.
- Developed layered recommendations for password storage and authentication security.

## Key Takeaways

- Password complexity alone is less valuable than sufficient length, uniqueness, and resistance to known-compromised values.
- Fast hashes make offline guessing inexpensive; password storage requires a deliberately slow adaptive algorithm.
- Multi-factor authentication reduces the impact of a compromised password.
- Rate limiting, lockout policies, and monitoring increase resistance to online guessing.
- Disabling plaintext and unnecessary services reduces credential exposure.
- Password-auditing tools are valuable for authorized validation, but their use requires strict scope and operational safeguards.

## Skills Demonstrated

- Kali Linux
- Hashcat
- John the Ripper
- Hydra
- Medusa
- Password hash analysis
- Dictionary-based security testing
- Authentication-service assessment
- Password-policy evaluation
- Defensive control recommendations
- Ethical penetration-testing practices
- Professional cybersecurity documentation
