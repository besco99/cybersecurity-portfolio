# Healthcare Penetration Testing Engagement Plan Review

## Overview

I evaluated a supplied penetration testing engagement plan for the fictional Western View Hospital as it prepared to launch a modernized medical and patient records system. This scenario-based planning and analysis lab examined whether the proposed engagement would provide useful security evidence while protecting patient care and sensitive information.

My conclusion was that the plan provided a useful starting point but needed clearer scope, authorization, safety boundaries, evidence handling, application coverage, and completion criteria. These are analysis findings about the written proposal, not confirmed weaknesses in a real hospital.

I did not conduct a penetration test, run the named tools, access systems or patient records, contact employees, or discover actual vulnerabilities.

## Lab Environment and Materials

- Fictional rural hospital preparing a patient-records system for production
- Supplied engagement-plan scenario
- My completed assessment report
- NIST SP 800-115 assessment guidance
- HIPAA Security Rule guidance from HHS
- PCI DSS applicability guidance from the PCI Security Standards Council

The work consisted of document review, risk analysis, and recommendation development. Nmap, Burp Suite, and the Social-Engineer Toolkit were named in the proposed plan; I reviewed their intended roles without using them.

## Objectives

- Connect the proposed testing to patient care, privacy, and system-readiness goals.
- Review internal, external, and phone-based social engineering coverage.
- Identify missing authorization and operational safeguards.
- Compare the plan with assessment guidance and applicable security requirements.
- Recommend safer validation methods and clear engagement completion criteria.
- Communicate limitations and distinguish planned activity from verified results.

## Hospital Scenario

Western View Hospital's scenario centers on protecting medical and associated financial information before a new records system goes live. The engagement should help management understand exposure, evaluate relevant controls, and decide whether important risks have been addressed.

Clinical availability matters alongside confidentiality and integrity. A test that interrupts essential care or weakens endpoint protection could undermine the hospital's goals even if it demonstrates an attack path. Exact clinical workflows, payment arrangements, and technical dependencies were not established in the supplied material and would need confirmation before execution.

## Proposed Engagement

| Proposed activity | What the plan described | What the review needed to resolve |
| --- | --- | --- |
| Internal testing | An internal sensor supporting discovery, enumeration, scanning, and attempted compromise of a central endpoint-security server | Approved targets, permitted impact, clinical dependencies, and limits on post-compromise access |
| External testing | Assessment of publicly accessible hospital assets from the testing provider's network | Exact assets, third-party permissions, application coverage, and authorized techniques |
| Phone-based social engineering | Support impersonation intended to obtain employee domain credentials | Approved participants, privacy limits, safe stopping points, and response to accidental disclosure |

The proposal involved coordination with hospital IT and some supplied environmental knowledge. It did not clearly define a black-box, gray-box, or white-box approach, so I treated that classification as unresolved.

## Review Method

I compared the proposed activities with the hospital's business goals, examined what authority and safeguards would be needed, and assessed whether the resulting evidence could support a launch decision. I then developed two recommendations for the overall engagement and two solutions for specific risky testing methods.

Where the plan omitted a control, I recorded a documentation gap. I did not infer that the fictional hospital lacked that control in practice.

## Analysis Findings

| Area | Gap in the proposed plan | Planning implication |
| --- | --- | --- |
| Scope | Broad references to hosts and employees without an approved asset, application, exclusion, or participant inventory | Test boundaries would be difficult to enforce or audit |
| Authorization | Coordination was described, but detailed approvals, third-party permission, and prohibited actions were not defined | Testers would have insufficient written authority for sensitive decisions |
| Patient safety | No defined testing windows, service-health thresholds, stop conditions, rollback process, or clinical escalation path | Intrusive activity could affect care dependencies without an agreed response |
| Evidence handling | Searches for patient information were not bounded by collection, access, storage, retention, or deletion rules | The assessment itself could create unnecessary exposure |
| Application coverage | Network-host compromise received more attention than authentication, authorization, sessions, interfaces, and audit behavior in the records application | Network results alone would not answer application-access questions |
| Reporting | No required findings deliverable, business-impact explanation, remediation ownership, or deadlines | Management would lack a consistent basis for prioritizing action |
| Retesting | No verification process or launch acceptance criteria | The engagement could end without confirming whether important corrections worked |

### Technique and Evidence Clarity

Naming a tool does not establish a valid exploitation method. Each technique should be connected to an approved objective, relevant preconditions, expected evidence, and acceptable impact. Tool versions also need validation before an actual engagement.

The plan's reference to directory traversal needed clarification: exploiting a path-traversal flaw differs from browsing files already accessible after a compromise. Likewise, obtaining information by telephone would not establish that physical access controls could be bypassed. Physical testing would require its own method and authorization.

## Comparison with Guidance and Security Requirements

### NIST SP 800-115

NIST SP 800-115 provides a basis for assessment planning, execution, analysis, data handling, and post-testing activities. Appendix B supplies a rules-of-engagement template. These elements informed my review of the proposal's missing boundaries and closure process. [NIST SP 800-115](https://csrc.nist.gov/pubs/sp/800/115/final)

The plan's internal and external perspectives were useful, but general coordination did not replace written target approval, impact limits, evidence procedures, or reporting arrangements. I recommended bounded validation and a documented route from findings to remediation and retesting. NIST guidance supports assessment design; it is not a compliance certification.

### HIPAA Security Rule

For the hospital scenario, the HIPAA Security Rule was the principal regulatory reference for electronic protected health information. It addresses administrative, physical, and technical safeguards, including risk analysis, access and audit controls, and contingency planning. [HHS Security Rule summary](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html)

I compared the proposed work with those safeguard areas and identified missing evaluation of role-based access, audit evidence, availability protections, and handling of collected records. These were gaps in the plan's ability to support a safeguard evaluation. Neither this document review nor a penetration test would establish complete HIPAA compliance.

### PCI DSS Applicability

PCI DSS scope must be confirmed if the hospital processes, stores, or transmits cardholder data, or operates systems that could affect the security of the cardholder data environment. General billing or financial information alone does not establish that scope. [PCI DSS overview](https://www.pcisecuritystandards.org/standards/pci-dss/)

The plan did not establish whether payment-card processing was present or identify its boundaries. If applicable, the engagement should map relevant systems and tests to PCI DSS requirements. If no applicable payment-card scope exists, that conclusion should be documented rather than assuming PCI DSS applies to every hospital system.

## Recommendations

### Recommendation 1: Establish Detailed Rules of Engagement

Require a signed scope and authorization package covering approved hosts, applications, networks, social engineering participants, exclusions, and third-party permissions. Clinical and IT representatives should approve testing windows, service-health monitoring, emergency contacts, and stop conditions.

The rules should prohibit destructive production activity and deliberate disabling of production endpoint protection. Higher-risk demonstrations should use an approved isolated replica where feasible. Define cleanup responsibilities and evidence access, encryption, retention, and disposal before testing begins.

### Recommendation 2: Expand Coverage with Clear Completion Criteria

Add the records application and its interfaces to the approved coverage, using test accounts representing relevant staff roles. Proposed checks should address authentication, authorization boundaries, session handling, and audit generation. Map coverage to applicable HIPAA safeguards and resolve PCI DSS applicability before adding payment-card tests.

Require a report containing validated evidence, business impact, prioritized recommendations, remediation owners, and deadlines. Agree on launch criteria with management, such as verified correction of critical access issues and documented decisions on residual risk. Retesting should verify agreed corrections under comparable conditions.

These are proposed improvements, not implemented controls or achieved remediation outcomes.

## Proposed Solutions

### Solution 1: Use Synthetic Patient Records for Bounded Proof of Access

Replace unrestricted patient-file searches with synthetic records placed in an approved test location. A future authorized check could record a redacted synthetic identifier, test account, access path, and related audit event to demonstrate an access boundary without collecting actual medical information.

If real patient information is encountered, stop further collection and notify the designated contact. Protect retained evidence with encryption and restricted access. If excessive access is confirmed during a future test, the owner would correct the relevant permissions and the tester would repeat the same bounded check. No such access or correction occurred in this lab.

### Solution 2: Redesign Phone Exercises to Avoid Real Password Collection

Retain the support-impersonation scenario but end the exercise when a participant indicates willingness to disclose a password, before the secret is captured. Record the behavioral outcome without storing credentials.

If a real credential is accidentally disclosed, follow the approved notification and reset procedure. Debrief participants on verifying support requests through a known contact method. A later comparable, authorized exercise could measure caller verification and reporting behavior after training. No calls, password collection, or follow-up exercise occurred in this review.

## Results

- Evaluated the proposed engagement against the fictional hospital's goals and launch-readiness needs.
- Identified planning gaps across scope, authorization, patient safety, evidence, application coverage, reporting, and retesting.
- Compared the proposal with NIST guidance, HIPAA safeguards, and conditional PCI DSS applicability.
- Developed two engagement recommendations and two safer testing solutions.
- Produced a planning analysis that distinguishes missing documentation from confirmed technical weaknesses.

## Limitations

This was a scenario-based document assessment. I did not validate the hospital's architecture, controls, workflows, or payment-card scope. No scans, exploits, tool execution, patient-record access, social engineering calls, or remediation verification were performed.

The findings apply to the supplied proposal's completeness and alignment, not to the security or compliance of an actual hospital. The analysis is not a legal determination or a substitute for an authorized technical assessment.

The original engagement plan, assessment template, rubric, and full report are not published here. Personal identifiers and course submission details have been removed. No commands, screenshots, tool output, or exploit results were created for this portfolio entry.

## Lessons Learned

- An engagement plan must connect technical activities to the client's operational goals.
- Patient safety and service availability belong in authorization and test-design decisions.
- Listing tools does not establish an assessment method or adequate coverage.
- Synthetic evidence can demonstrate risk while limiting sensitive-data exposure.
- Social engineering outcomes can be measured without collecting real passwords.
- Reporting, ownership, retesting, and acceptance criteria make findings useful for decisions.
- Missing plan detail should be reported as uncertainty rather than assumed to be a real-world vulnerability.

## Skills Demonstrated

- Penetration testing engagement-plan review
- Scope and authorization analysis
- Rules-of-engagement design
- Healthcare operational-risk assessment
- Patient-safety and evidence-handling planning
- Application coverage analysis
- NIST SP 800-115 comparison
- HIPAA safeguard mapping and PCI DSS scope evaluation
- Ethical social engineering exercise design
- Remediation, retesting, and acceptance planning
- Clear technical and business-risk communication

## Source References

- My completed assessment of the supplied fictional hospital engagement plan served as the primary source for this portfolio summary. The report and proprietary scenario materials are retained privately.
- Scarfone, K., Souppaya, M., Cody, A., and Orebaugh, A. (2008). [Technical Guide to Information Security Testing and Assessment, NIST SP 800-115](https://doi.org/10.6028/NIST.SP.800-115). Relevant portions include Sections 6–8 and Appendix B.
- U.S. Department of Health and Human Services. [Summary of the HIPAA Security Rule](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).
- PCI Security Standards Council. [PCI Data Security Standard overview and applicability](https://www.pcisecuritystandards.org/standards/pci-dss/).
