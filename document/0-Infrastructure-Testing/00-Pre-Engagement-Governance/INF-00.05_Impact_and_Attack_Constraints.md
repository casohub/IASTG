# INF-00.05 — Definition of Allowed Impact and Attack Constraints

## Summary

This test verifies that acceptable testing impact levels and attack technique constraints have been explicitly defined to prevent unacceptable business disruption while maintaining testing effectiveness.

## Risk

Undefined impact tolerances result in testing activities that cause unacceptable service disruptions, data loss, or system damage. Organizations face operational outages, financial losses, regulatory violations, and customer impact when testing exceeds acceptable risk thresholds. Conversely, excessive constraints prevent identification of real-world vulnerabilities and create false assurance of security posture. Lack of clear attack constraints leads to disputes regarding testing methods and unexpected business impacts.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Testing scope has been defined (see INF-00.02)
- Testing approach has been determined (see INF-00.04)
- No technical access is required

## Test Objectives

- Verify explicit documentation of allowed testing impact levels
- Confirm attack technique constraints are clearly defined
- Validate impact definitions are measurable and enforceable
- Ensure constraints do not prevent identification of critical vulnerabilities
- Confirm escalation procedures for impact threshold violations

## Test Methodology

1. Review testing documentation for explicit impact level definitions across the following dimensions:
   - **Availability:** Acceptable service disruption duration and scope
   - **Performance:** Allowed performance degradation thresholds
   - **Data Integrity:** Restrictions on data modification or deletion
   - **System State:** Permitted system configuration changes
   - **Resource Consumption:** Acceptable CPU, memory, disk, or network utilization levels
2. Verify attack technique constraints are documented including:
   - Denial of service testing permissions and limits
   - Social engineering and phishing attack scope
   - Physical security testing authorization
   - Exploitation of vulnerabilities (limited to proof of concept versus full exploitation)
   - Data exfiltration limits (volume, sensitivity)
   - Password cracking parameters (online versus offline, duration limits)
   - Destructive testing permissions (malware deployment, data destruction)
3. Confirm time-based restrictions are defined:
   - Allowed testing hours and blackout periods
   - Holiday or business-critical period exclusions
   - Maximum duration for disruptive tests
4. Validate testing constraints for specific system categories:
   - Production versus non-production environment restrictions
   - Business-critical system specific limitations
   - Shared or multi-tenant infrastructure constraints
   - Systems with regulatory compliance requirements
5. Review escalation and approval procedures for:
   - Testing activities approaching impact thresholds
   - Request to exceed defined constraints
   - Emergency testing suspension procedures
6. Confirm monitoring and communication requirements for high-impact activities:
   - Advance notification periods for disruptive tests
   - Real-time communication channels during active testing
   - Impact threshold breach notification procedures
7. Document rationale for constraints to assess if limitations prevent identification of critical vulnerabilities
8. Verify insurance coverage and liability terms account for allowed impact levels

## Expected Secure Configuration

Organizations defining impact and constraint parameters should provide:

- Quantitative impact thresholds with measurable criteria
- Specific attack technique permissions and restrictions
- Time-based testing windows and blackout periods
- System-specific constraints based on criticality and compliance requirements
- Escalation procedures for constraint modifications
- Real-time communication protocols during high-impact testing
- Risk acceptance documentation for testing constraints
- Rollback and recovery procedures for testing impacts
- Insurance and liability coverage aligned with allowed impact levels

## Evidence to Collect

- Documentation of allowed impact levels across availability, performance, data integrity, and system state
- List of permitted and restricted attack techniques
- Time-based testing restrictions and blackout periods
- System-specific constraint definitions
- Escalation and approval procedures for constraint modifications
- Communication protocols for high-impact testing activities
- Risk acceptance documentation for constraints limiting vulnerability identification
- Insurance and liability documentation
- Version and date of impact and constraint documentation

## Impact

**Business Continuity Impact:**
Undefined or excessive testing impacts cause unplanned service outages, missed service level agreements (SLAs), and customer-facing disruptions. Financial systems, e-commerce platforms, and critical operational technology may experience revenue loss during testing-induced outages. Production data corruption or loss creates business continuity failures.

**Security Validation Impact:**
Overly restrictive constraints prevent realistic vulnerability validation. Denial of service vulnerabilities, resource exhaustion attacks, and exploitation impacts remain unidentified when testing is artificially limited. Organizations develop false confidence in security posture when testing does not reflect real attack techniques.

**Legal and Compliance Impact:**
Testing that modifies or accesses regulated data without appropriate controls violates compliance frameworks such as PCI DSS, HIPAA, GDPR, or SOX. Organizations face regulatory penalties, audit failures, and breach notification requirements when testing exceeds acceptable data handling constraints.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

Note: Attack constraints directly influence which MITRE ATT&CK techniques can be validated. Impact constraints particularly affect techniques in the Impact tactic (T1485-T1565) and resource-intensive Credential Access techniques.

## Remediation Guidance

Organizations should implement the following impact and constraint management practices:

1. **Quantitative Impact Thresholds:**
   - Define measurable availability thresholds (maximum downtime duration)
   - Establish performance degradation limits (maximum response time increase)
   - Set resource consumption ceilings (CPU, memory, network bandwidth percentages)
   - Document acceptable system state changes
   - Specify data modification restrictions with explicit examples

2. **Risk-Based Constraint Setting:**
   - Balance testing effectiveness against business risk tolerance
   - Minimize constraints to allow realistic attack validation
   - Document rationale for each constraint with risk justification
   - Conduct risk-benefit analysis for restrictive constraints
   - Review constraints against organizational threat intelligence

3. **Testing Environment Strategy:**
   - Utilize non-production environments for high-impact testing where possible
   - Establish production-equivalent test environments for realistic validation
   - Define different constraint sets for production versus non-production
   - Implement data masking or synthetic data for testing regulated systems

4. **Communication and Monitoring:**
   - Establish real-time communication channels during high-impact tests
   - Require advance notification for tests approaching impact thresholds
   - Deploy monitoring to detect impact threshold violations
   - Implement automated alerts for resource consumption or availability issues
   - Conduct post-test impact assessments

5. **Constraint Evolution:**
   - Review and update constraints based on testing outcomes
   - Adjust constraints as organizational risk tolerance evolves
   - Incorporate lessons learned from previous testing impacts
   - Align constraints with current threat landscape and attack techniques
   - Conduct annual constraint effectiveness reviews
