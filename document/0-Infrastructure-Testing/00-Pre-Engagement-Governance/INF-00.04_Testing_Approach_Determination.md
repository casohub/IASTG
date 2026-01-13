# INF-00.04 — Determination of Testing Approach (Black Box, Grey Box, Assumed Breach)

## Summary

This test verifies that the testing approach and methodology have been explicitly defined, including the level of prior knowledge and access provided to testers, and that the selected approach aligns with organizational security objectives.

## Risk

Misaligned testing approaches result in ineffective security assessments that fail to identify relevant threats. Black box testing without appropriate knowledge may miss critical vulnerabilities accessible through insider threat scenarios. Conversely, excessive information disclosure may bias testing and fail to validate external defenses. Undefined testing approaches create confusion regarding tester privileges, access methods, and expected coverage, leading to incomplete assessments and unmet stakeholder expectations.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Testing scope has been defined (see INF-00.02)
- No technical access is required

## Test Objectives

- Verify explicit documentation of testing approach and methodology
- Confirm alignment between testing approach and organizational threat model
- Validate appropriate information disclosure levels for selected approach
- Ensure testing approach supports identification of relevant vulnerabilities
- Confirm tester access levels and starting positions are defined

## Test Methodology

1. Review testing documentation for explicit definition of one of the following approaches:
   - **Black Box:** No prior knowledge or credentials provided; tester simulates external attacker perspective
   - **Grey Box:** Partial knowledge provided such as network diagrams, application documentation, or user-level credentials
   - **Assumed Breach:** Internal network access and user or system credentials provided; simulates insider threat or post-compromise scenario
2. Verify rationale for selected approach is documented and aligned with organizational objectives:
   - External threat validation (black box)
   - Realistic threat modeling with partial knowledge (grey box)
   - Post-compromise impact and lateral movement assessment (assumed breach)
3. Confirm information disclosure levels are appropriate for selected approach:
   - Network architecture documentation
   - System inventories and asset lists
   - Application documentation or source code
   - User credentials or service account access
   - Administrator credentials or privileged access
4. Validate tester starting position is clearly defined:
   - External network position (Internet-facing)
   - Internal network access point
   - VPN or remote access credentials
   - Physical location or wireless network access
   - Cloud environment access
5. Review testing approach against threat scenarios the organization seeks to evaluate:
   - External attacker penetration
   - Insider threat or malicious employee
   - Compromised user account escalation
   - Supply chain or third-party compromise
6. Confirm testing approach does not create artificial constraints that prevent identification of real-world attack paths
7. Document any hybrid approaches combining elements of multiple methodologies
8. Verify testing timeline and effort allocation align with selected approach complexity

## Expected Secure Configuration

Organizations defining testing approaches should provide:

- Explicit documentation of testing methodology with clear definitions
- Rationale for approach selection based on threat model and objectives
- Information disclosure policy specifying what knowledge is provided to testers
- Definition of tester starting position including network access and credentials
- Alignment of testing approach with organizational risk priorities
- Threat scenario descriptions the testing aims to validate
- Resource allocation and timeline appropriate for approach complexity
- Documentation of any deviations from standard testing methodologies

## Evidence to Collect

- Written definition of testing approach and methodology
- Documentation of rationale for approach selection
- List of information disclosed to testers (network diagrams, credentials, documentation)
- Definition of tester starting position and initial access level
- Threat scenarios or attack narratives the testing aims to validate
- Communication records clarifying approach elements
- Comparison of approach against organizational threat model
- Version and date of approach documentation

## Impact

**Security Assessment Effectiveness:**
Inappropriate testing approaches fail to identify vulnerabilities relevant to organizational threat landscape. Black box testing may miss insider threats while grey box testing may fail to validate external defenses. Testing effectiveness is compromised when approach does not align with real threat scenarios.

**Resource Efficiency:**
Black box testing requires significantly more time for reconnaissance and initial access compared to assumed breach testing. Misaligned approach selection wastes testing effort on irrelevant scenarios while missing critical attack paths. Timeline and budget overruns occur when approach complexity exceeds resource allocation.

**Stakeholder Expectations:**
Undefined or poorly communicated testing approaches create mismatched expectations regarding deliverables. Stakeholders expecting comprehensive external penetration testing may receive assumed breach results focused on post-compromise scenarios. This leads to dissatisfaction and requirement for re-testing.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

Note: Testing approach selection influences which MITRE ATT&CK tactics are evaluated. Black box testing emphasizes Reconnaissance and Initial Access. Assumed breach testing focuses on Privilege Escalation, Lateral Movement, and Impact tactics.

## Remediation Guidance

Organizations should implement the following testing approach definition practices:

1. **Threat Model Alignment:**
   - Conduct threat modeling to identify relevant attack scenarios
   - Select testing approach that validates highest-priority threats
   - Consider multiple testing approaches for comprehensive coverage
   - Document threat scenarios driving approach selection

2. **Approach Definition Standards:**
   - Define organizational standard terminology for testing approaches
   - Create templates specifying information disclosure for each approach
   - Document standard starting positions and access levels
   - Establish approach selection criteria based on objectives

3. **Information Disclosure Management:**
   - Define what information is provided for each testing approach
   - Establish secure methods for credential and documentation delivery
   - Limit information disclosure to minimum necessary for approach
   - Document all information provided to testers

4. **Hybrid Approach Considerations:**
   - Combine approaches for comprehensive coverage (e.g., black box external followed by assumed breach internal)
   - Clearly delineate phases when multiple approaches are used
   - Allocate appropriate timeline and resources for each phase
   - Document rationale for hybrid approach selection

5. **Approach Validation:**
   - Review testing approach against organizational threat intelligence
   - Validate approach identifies vulnerabilities relevant to known threats
   - Conduct periodic approach effectiveness reviews
   - Adjust approach based on changing threat landscape and business priorities
