# INF-00.08 — Alignment with Monitoring (SOC/IR) Teams for Testing

## Summary

This test verifies that security operations center (SOC), incident response (IR), and monitoring teams have been informed of testing activities and that appropriate coordination procedures are established to distinguish legitimate testing from actual attacks.

## Risk

Failure to coordinate with SOC and IR teams results in testing activities being mistaken for genuine attacks, triggering unnecessary incident response procedures, system isolations, IP blocking, and law enforcement notifications. Wasted IR resources, operational disruptions, and premature testing termination occur when defensive teams respond to testing as hostile activity. Conversely, alerting defensive teams to all testing details may compromise test validity by creating artificial awareness that would not exist during real attacks. Organizations face inadequate validation of detection capabilities when monitoring teams have advance knowledge that prevents realistic detection testing.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Testing scope has been defined (see INF-00.02)
- Testing approach has been determined (see INF-00.04)
- No technical access is required

## Test Objectives

- Verify SOC and IR teams are notified of testing activities
- Confirm coordination procedures balance detection validation with operational continuity
- Validate communication channels for testing-related alerts
- Ensure distinction criteria between testing and genuine attacks are defined
- Confirm procedures exist for handling high-confidence threat detections during testing

## Test Methodology

1. Review documentation for formal notification to security monitoring and incident response teams including:
   - Security Operations Center (SOC)
   - Computer Security Incident Response Team (CSIRT)
   - Network Operations Center (NOC)
   - Managed Security Service Providers (MSSP)
   - Security Information and Event Management (SIEM) operators
2. Verify notification approach is documented with one of the following coordination levels:
   - **Full Disclosure:** SOC/IR teams receive complete testing details including scope, timing, source IPs, and techniques
   - **Partial Disclosure:** Limited information provided such as testing timeframe and general scope without specific TTPs
   - **Purple Team:** Collaborative testing with real-time coordination between attackers and defenders
   - **Red Team (No Disclosure):** Minimal or no advance notification to validate realistic detection capabilities
3. Confirm coordination approach aligns with testing objectives:
   - Detection capability validation requires limited SOC notification
   - Operational continuity testing requires SOC awareness to prevent disruption
   - Purple team exercises require real-time collaboration
4. Validate communication channels are established for testing coordination:
   - Primary contact persons in SOC/IR with 24/7 availability
   - Secure communication methods (phone, encrypted messaging, dedicated channels)
   - Escalation procedures for urgent coordination needs
   - Out-of-band verification methods for authentication of testing communications
5. Review procedures for handling threat detections during testing:
   - SOC alert triage procedures to identify potential testing activity
   - Verification methods to distinguish testing from genuine attacks
   - Defined response actions for high-confidence threat detections during testing windows
   - Escalation criteria requiring testing pause or termination
6. Confirm testing activity identification mechanisms are documented:
   - Source IP address identification methods
   - User agent strings or custom headers identifying testing tools
   - Testing-specific indicators or signatures
   - Timing correlation with documented testing windows
7. Verify post-testing coordination procedures:
   - Testing completion notification to SOC/IR teams
   - Detection capability review and lessons learned process
   - Alert analysis to identify true positives generated during testing
   - Gap analysis for undetected testing activities
8. Document any monitoring or defensive system modifications for testing:
   - Temporary rule adjustments or tuning
   - Whitelisting or suppression of testing-related alerts
   - Baseline modifications or threshold adjustments

## Expected Secure Configuration

Organizations coordinating with monitoring teams should provide:

- Formal notification to all security monitoring and incident response teams
- Documented coordination approach aligned with testing objectives
- Primary and secondary contact information for 24/7 SOC/IR availability
- Secure communication channels with authentication procedures
- Testing activity identification mechanisms and indicators
- Escalation and response procedures for threat detections during testing
- Post-testing review and lessons learned process
- Documentation of any defensive system modifications for testing
- Clear roles and responsibilities for testing coordination
- Version-controlled coordination documentation

## Evidence to Collect

- Notification records showing SOC/IR team awareness of testing
- Documentation of coordination approach and disclosure level
- Contact information for SOC/IR team members with 24/7 availability
- Communication channel setup confirmation
- Testing activity identification indicators or methods
- Escalation and response procedures documentation
- Records of any defensive system modifications for testing
- Post-testing coordination meeting notes or reports
- Detection capability analysis results
- Version and date of coordination documentation

## Impact

**Operational Disruption Impact:**
Uncoordinated testing triggers full incident response procedures including system isolation, IP blocking, forensic preservation, and law enforcement notification. Business operations are disrupted by defensive responses to testing activities. Testing is prematurely terminated before objectives are met. Incident response resources are wasted on non-genuine threats.

**Detection Validation Impact:**
Excessive SOC notification compromises testing validity by creating artificial awareness. Detection capabilities remain unvalidated when monitoring teams have advance knowledge of specific testing techniques. Organizations develop false confidence in detection capabilities that would fail during genuine attacks. Security control gaps remain unidentified.

**Relationship and Trust Impact:**
Inadequate coordination damages relationships between testing and operational security teams. SOC teams lose confidence in testing programs when coordination failures cause operational disruptions. Future testing encounters resistance from operational teams due to past coordination issues.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

Note: Coordination approach directly affects validation of detection for MITRE ATT&CK techniques across all tactics. Full disclosure prevents realistic detection testing while no disclosure risks operational disruption.

## Remediation Guidance

Organizations should implement the following SOC/IR coordination practices:

1. **Coordination Level Selection:**
   - Select coordination approach based on testing objectives
   - Balance detection validation needs with operational continuity
   - Document rationale for coordination level selection
   - Consider phased approach with varying disclosure levels
   - Align coordination level with organizational risk tolerance

2. **Communication Infrastructure:**
   - Establish dedicated secure communication channels for testing coordination
   - Implement out-of-band authentication methods for testing notifications
   - Create 24/7 contact roster with primary and backup personnel
   - Deploy encrypted messaging or secure collaboration platforms
   - Test communication channels before testing commencement

3. **Testing Identification Methods:**
   - Define technical indicators for SOC to identify testing activity
   - Implement user agent strings or custom headers in testing tools
   - Document source IP ranges for testing infrastructure
   - Establish timing correlation procedures
   - Create playbooks for SOC testing activity verification

4. **Escalation and Response Procedures:**
   - Define criteria for testing pause or termination
   - Establish escalation paths for high-confidence threat detections
   - Document response actions for different alert severity levels
   - Create decision trees for SOC analysts handling testing-window alerts
   - Implement rapid communication procedures for urgent escalations

5. **Detection Capability Validation:**
   - Conduct post-testing detection analysis with SOC team
   - Identify true positive alerts generated during testing
   - Analyze testing activities that evaded detection
   - Document detection capability gaps
   - Implement detection tuning and rule improvements based on findings
   - Schedule follow-up testing to validate detection improvements
