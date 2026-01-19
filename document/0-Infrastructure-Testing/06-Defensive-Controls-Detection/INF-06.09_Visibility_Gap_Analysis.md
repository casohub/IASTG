# INF-06.09 — Visibility Gap Analysis (Blind Spots in Monitoring)

## Summary

This test identifies blind spots and gaps in security monitoring coverage where malicious activities could occur undetected, performing comprehensive visibility assessment across technical, procedural, and organizational dimensions.

## Risk

Monitoring blind spots create opportunities for undetected malicious activities enabling extended compromise. Network segments without visibility allow covert lateral movement and data exfiltration. Unmonitored endpoints enable malware execution and credential theft. Monitoring gaps in applications and cloud services conceal unauthorized access. Organizations face extended dwell time and catastrophic breaches when blind spots prevent detection of attacker activities.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Security monitoring infrastructure assessed (see INF-06.01 through INF-06.08)
- Network and system inventory available
- Testing includes comprehensive visibility assessment

## Test Objectives

- Identify blind spots in network monitoring coverage
- Assess endpoint visibility gaps
- Evaluate application and service monitoring gaps
- Test cloud environment visibility
- Validate comprehensive threat detection coverage

## Test Methodology

1. Assess network visibility gaps:
   - Identify network segments without monitoring
   - Test for east-west traffic blind spots
   - Verify encrypted traffic visibility
   - Assess wireless network monitoring
   - Identify out-of-band management visibility gaps
2. Evaluate endpoint visibility gaps:
   - Identify endpoints without EDR or antivirus
   - Test for unmanaged BYOD device visibility
   - Assess IoT and OT device monitoring
   - Verify contractor and temporary system coverage
   - Identify systems exempt from monitoring
3. Test application monitoring gaps:
   - Assess web application logging and monitoring
   - Verify database activity monitoring
   - Test SaaS application visibility
   - Assess custom application logging
   - Identify unmonitored API endpoints
4. Assess cloud environment visibility:
   - Verify cloud infrastructure monitoring (AWS, Azure, GCP)
   - Test cloud workload visibility
   - Assess serverless and container monitoring
   - Verify cloud storage access monitoring
   - Identify shadow IT and unapproved cloud services
5. Evaluate authentication and access blind spots:
   - Identify unmonitored authentication services
   - Assess VPN and remote access visibility
   - Test federated authentication monitoring
   - Verify privileged access monitoring gaps
   - Assess service account usage visibility
6. Test temporal blind spots:
   - Verify monitoring during off-hours
   - Assess weekend and holiday monitoring
   - Test for maintenance window visibility gaps
   - Verify monitoring during disaster recovery
   - Assess monitoring backup and redundancy
7. Assess log retention blind spots:
   - Identify logs with inadequate retention
   - Verify historical investigation capabilities
   - Test for log rotation gaps
   - Assess long-term archival accessibility
   - Identify compliance retention gaps
8. Evaluate detection coverage:
   - Map monitoring to MITRE ATT&CK techniques
   - Identify undetected attack techniques
   - Assess coverage across attack lifecycle
   - Verify defense-in-depth monitoring
   - Test for evasion technique blind spots

## Expected Secure Configuration

Organizations eliminating visibility gaps should implement:

- Comprehensive network monitoring across all segments
- Universal endpoint visibility with EDR deployment
- Application and database activity monitoring
- Complete cloud environment visibility
- Authentication and access monitoring coverage
- 24/7 monitoring without temporal gaps
- Adequate log retention for investigation
- Defense-in-depth monitoring layers
- MITRE ATT&CK technique coverage mapping
- Regular blind spot assessments and remediation

## Evidence to Collect

- Network segment visibility assessment
- Endpoint monitoring coverage inventory
- Application and service monitoring gaps
- Cloud environment visibility evaluation
- Authentication monitoring coverage
- Temporal monitoring gap analysis
- Log retention adequacy assessment
- MITRE ATT&CK coverage mapping
- Blind spot prioritization and risk analysis
- Remediation roadmap for identified gaps

## Impact

**Undetected Attack Activities Impact:**
Blind spots enable attackers to operate undetected in monitored environments. Network segments without visibility allow covert lateral movement. Unmonitored endpoints enable malware execution and data staging. Organizations discover breaches only after significant damage when blind spots conceal attacker activities.

**Extended Dwell Time Impact:**
Visibility gaps extend attacker dwell time enabling prolonged reconnaissance, data theft, and attack preparation. Attackers identify and exploit blind spots for persistent access. Organizations face massive breaches resulting from extended undetected presence in blind spots.

**Incomplete Incident Investigation Impact:**
Monitoring gaps prevent complete incident investigation and attack timeline reconstruction. Investigators cannot determine full breach scope without comprehensive visibility. Organizations struggle with containment and remediation when blind spots hide attacker activities and persistence mechanisms.

## MITRE ATT&CK Mapping

- **Not Applicable** - Defensive assessment identifying detection coverage

However, blind spot analysis addresses detection across all MITRE ATT&CK tactics.

## Remediation Guidance

Organizations should implement the following blind spot elimination practices:

1. **Comprehensive Network Visibility:**
   - Deploy network monitoring across all segments
   - Implement east-west traffic visibility
   - Deploy SSL/TLS inspection where feasible
   - Monitor wireless networks comprehensively
   - Include out-of-band management in monitoring
   - Deploy NetFlow or IPFIX universally
   - Implement network packet capture capabilities

2. **Universal Endpoint Monitoring:**
   - Deploy EDR on 100% of endpoints
   - Include servers, workstations, and virtual machines
   - Monitor BYOD devices with mobile threat defense
   - Deploy specialized monitoring for IoT and OT
   - Include contractor and temporary systems
   - Eliminate monitoring exceptions where possible
   - Document and risk-assess any remaining gaps

3. **Application and Service Monitoring:**
   - Implement web application firewall (WAF) with logging
   - Deploy database activity monitoring (DAM)
   - Monitor SaaS application usage (CASB)
   - Enable comprehensive logging on custom applications
   - Monitor API gateways and endpoints
   - Implement application performance monitoring (APM)
   - Deploy cloud workload protection platforms (CWPP)

4. **Cloud Environment Visibility:**
   - Enable CloudTrail, Activity Log, Audit Logs universally
   - Deploy cloud security posture management (CSPM)
   - Monitor serverless and container environments
   - Implement cloud workload protection
   - Deploy cloud access security broker (CASB)
   - Monitor cloud storage access comprehensively
   - Detect and monitor shadow IT

5. **Temporal Coverage Enhancement:**
   - Implement 24/7 security operations center (SOC)
   - Deploy automated monitoring and alerting
   - Ensure monitoring during maintenance windows
   - Verify monitoring redundancy and failover
   - Test monitoring during disaster recovery scenarios
   - Eliminate scheduled monitoring downtimes
   - Deploy follow-the-sun SOC operations

6. **Blind Spot Assessment Program:**
   - Conduct quarterly visibility gap assessments
   - Map monitoring to MITRE ATT&CK framework
   - Identify and prioritize blind spots by risk
   - Develop remediation roadmap for gaps
   - Test detection against realistic attack scenarios
   - Conduct red team exercises identifying blind spots
   - Implement continuous monitoring improvement program
