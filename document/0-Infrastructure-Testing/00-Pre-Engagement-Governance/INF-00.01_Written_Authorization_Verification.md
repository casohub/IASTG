# INF-00.01 — Verification of Written Authorization and Scope

## Summary

This test verifies that formal written authorization exists prior to conducting infrastructure security testing and that the scope of testing has been explicitly documented and approved by authorized organizational representatives.

## Risk

Conducting security testing without proper written authorization exposes the testing organization to legal liability including potential criminal charges for unauthorized access to computer systems. Testing outside approved scope may result in business disruption, contractual breach, regulatory violations, and damage to critical systems. Organizations face reputational and financial harm if testing occurs without proper governance controls.

## Preconditions

- Initial contact with requesting organization has been established
- Point of contact information is available
- No access to target systems is required for this control

## Test Objectives

- Verify existence of signed authorization document prior to any testing activity
- Confirm authorization document specifies scope boundaries including networks, systems, and timeframes
- Validate authorization signature authority is appropriate for the scope of testing
- Ensure authorization document includes explicit permission for testing methods to be employed
- Confirm liability and responsibility terms are documented

## Test Methodology

1. Request provision of written authorization document before conducting any technical testing activities
2. Review authorization document for presence of the following mandatory elements:
   - Explicit permission to conduct security testing
   - Named systems, networks, or IP ranges that are in-scope
   - Testing timeframe with start and end dates
   - Signature from organizational authority with legal capacity to grant permission
   - Date of authorization signature
3. Verify signatory authority by requesting confirmation of role and authorization level
4. Confirm authorization document addresses legal considerations including:
   - Acknowledgment of testing methods
   - Limitation of liability terms
   - Data handling and confidentiality provisions
   - Incident notification procedures
5. Document any scope limitations or constraints specified in the authorization
6. Retain authorization document in secure testing documentation repository
7. If authorization is ambiguous, incomplete, or absent, halt all testing activities and escalate to organizational management
8. Re-verify authorization if scope changes are requested during the engagement

## Expected Secure Configuration

Organizations conducting infrastructure security testing must maintain a written authorization process that includes:

- Documented authorization request and approval workflow
- Template authorization documents that specify all required elements
- Signature authority matrix defining who may authorize testing at various scope levels
- Version control for authorization documents
- Secure storage of executed authorization documents
- Re-authorization requirements for scope changes or time extensions
- Authorization review and approval by legal and technical stakeholders prior to testing commencement

## Evidence to Collect

- Copy of executed authorization document with original signatures
- Email or written correspondence confirming authorization receipt
- Documentation of signatory verification (title, role, authority confirmation)
- Records of any scope clarification communications
- Copy of authorization document retention acknowledgment
- Screenshot or record showing date of authorization verification
- Documentation of authorization review if scope changes occurred

## Impact

**Legal Impact:**
Absence of written authorization may constitute unauthorized access to computer systems under applicable laws including the Computer Fraud and Abuse Act (CFAA) in the United States or equivalent legislation in other jurisdictions. Criminal and civil liability may result.

**Organizational Impact:**
Testing without authorization exposes both the testing organization and the client organization to legal risk, regulatory sanctions, and reputational damage. Insurance coverage for professional liability may be voided if proper authorization was not obtained.

**Operational Impact:**
Clear authorization prevents misunderstandings about scope, protects testing personnel from legal consequences, and provides documented evidence of permission in the event of incidents or investigations.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

## Remediation Guidance

Organizations should implement the following authorization governance practices:

1. **Establish Authorization Process:**
   - Define formal authorization request and approval procedures
   - Create authorization document templates with all required elements
   - Establish signature authority requirements based on testing scope and risk

2. **Legal Review:**
   - Require legal department review of authorization documents
   - Ensure compliance with applicable laws and regulations
   - Include appropriate liability limitation and indemnification language

3. **Authorization Management:**
   - Implement secure document management system for authorization storage
   - Maintain authorization tracking log with unique identifiers
   - Establish authorization expiration and renewal procedures

4. **Scope Change Management:**
   - Require re-authorization for any scope expansions
   - Document and approve scope changes through formal change control
   - Communicate scope changes to all stakeholders and testing personnel

5. **Training:**
   - Train security testing personnel on authorization verification requirements
   - Educate business units on authorization request procedures
   - Conduct periodic reviews of authorization process effectiveness
