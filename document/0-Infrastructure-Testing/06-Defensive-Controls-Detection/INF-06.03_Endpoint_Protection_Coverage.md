# INF-06.03 — Endpoint Protection Coverage (AV, EDR Deployment)

## Summary

This test assesses the deployment and effectiveness of endpoint protection solutions including antivirus, anti-malware, and endpoint detection and response (EDR) systems across workstations, servers, and other endpoints.

## Risk

Inadequate endpoint protection coverage creates vulnerability to malware, ransomware, and advanced persistent threats. Endpoints without protection enable malware execution and lateral movement. Legacy signature-based antivirus fails against modern threats. Absent EDR prevents detection of sophisticated attacks and behavioral threats. Organizations face rapid compromise propagation when endpoint protection gaps enable malware spread and advanced attack techniques.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Endpoint inventory available
- Access to endpoint protection management consoles
- Testing includes endpoint security assessment

## Test Objectives

- Assess endpoint protection deployment coverage
- Evaluate endpoint protection effectiveness
- Test detection capabilities for modern threats
- Identify unprotected or vulnerable endpoints
- Validate endpoint protection configuration

## Test Methodology

1. Enumerate endpoint protection deployment:
   - Identify deployed endpoint protection solutions
   - Verify antivirus/anti-malware coverage on workstations
   - Assess server endpoint protection deployment
   - Identify EDR solution deployment and coverage
   - Review mobile device endpoint protection
2. Assess deployment coverage and gaps:
   - Calculate percentage of endpoints with protection
   - Identify unprotected systems and assets
   - Verify protection on critical servers and workstations
   - Assess coverage on virtual machines and cloud instances
   - Identify systems with outdated or expired protection
3. Test malware detection capabilities:
   - Test detection of known malware samples (EICAR, test files)
   - Verify detection of ransomware indicators
   - Test detection of living-off-the-land (LOLBins) attacks
   - Assess detection of fileless malware
   - Verify detection of web shells and backdoors
4. Evaluate EDR capabilities:
   - Test behavioral detection and analytics
   - Verify process monitoring and visibility
   - Assess network connection monitoring
   - Test file creation and modification detection
   - Verify registry modification monitoring
5. Test endpoint protection configuration:
   - Verify real-time protection is enabled
   - Test automatic signature update functionality
   - Assess scanning schedule and frequency
   - Verify quarantine and remediation settings
   - Test tamper protection preventing disabling
6. Assess detection bypass and evasion:
   - Test obfuscation bypassing signature detection
   - Verify detection of packed or encrypted malware
   - Test memory-only malware detection
   - Assess detection of script-based attacks (PowerShell, WMI)
   - Verify anti-exploitation feature effectiveness
7. Evaluate centralized management:
   - Verify centralized policy management
   - Test real-time visibility and reporting
   - Assess alert escalation and investigation
   - Verify endpoint isolation capabilities
   - Test remote remediation functionality
8. Test performance and compatibility:
   - Assess system performance impact
   - Verify compatibility with business applications
   - Test protection during system updates
   - Assess resource utilization
   - Verify stability and reliability

## Expected Secure Configuration

Organizations deploying endpoint protection should maintain:

- 100% endpoint protection coverage on all systems
- Modern EDR deployed on workstations and servers
- Real-time protection enabled continuously
- Automatic signature and definition updates
- Behavioral detection and analytics enabled
- Cloud-assisted protection for threat intelligence
- Centralized management and monitoring
- Tamper protection preventing disabling
- Application control (whitelisting) where feasible
- Regular effectiveness validation and testing

## Evidence to Collect

- Endpoint protection inventory and coverage analysis
- Deployment gap identification
- Malware detection testing results
- EDR capability validation
- Configuration assessment per endpoint
- Detection bypass testing outcomes
- Centralized management verification
- Performance impact assessment
- Coverage statistics and metrics
- Remediation recommendations for gaps

## Impact

**Malware and Ransomware Infection Impact:**
Unprotected endpoints enable malware execution and propagation. Ransomware spreads rapidly across unprotected systems encrypting data. Legacy antivirus fails to detect modern threats. Organizations face operational disruption and data loss from malware infections on unprotected endpoints.

**Advanced Persistent Threat (APT) Impact:**
Absence of EDR prevents detection of sophisticated attacks using living-off-the-land techniques. Behavioral threats evade signature-based detection. Memory-only malware operates undetected without EDR. Organizations suffer extended compromise when endpoint protection cannot detect advanced threats.

**Lateral Movement and Propagation Impact:**
Endpoint protection gaps enable lateral movement across infrastructure. Malware spreads from unprotected to protected systems. Worms propagate through networks lacking comprehensive protection. Organizations face infrastructure-wide compromise when endpoint protection coverage is inconsistent.

## MITRE ATT&CK Mapping

Endpoint protection defends against techniques across multiple tactics:
- **TA0002** - Execution (malware execution)
- **TA0003** - Persistence (malicious software persistence)
- **TA0004** - Privilege Escalation (exploitation)
- **TA0005** - Defense Evasion (anti-detection techniques)

## Remediation Guidance

Organizations should implement the following endpoint protection practices:

1. **Universal Endpoint Protection Deployment:**
   - Deploy endpoint protection on 100% of endpoints
   - Include workstations, servers, virtual machines, cloud instances
   - Protect mobile devices with mobile threat defense (MTD)
   - Deploy protection on Linux and macOS endpoints
   - Implement protection on IoT and embedded devices where possible
   - Regular deployment coverage audits

2. **Modern EDR Solution Deployment:**
   - Deploy enterprise EDR across all endpoints
   - Implement behavioral detection and analytics
   - Enable continuous monitoring and visibility
   - Deploy cloud-connected threat intelligence
   - Implement automated response capabilities
   - Select EDR with strong detection and low false positives

3. **Endpoint Protection Configuration:**
   - Enable real-time protection continuously
   - Configure automatic signature updates
   - Implement behavioral detection features
   - Enable cloud-assisted protection
   - Deploy application control (whitelisting)
   - Enable tamper protection preventing disabling
   - Configure aggressive detection sensitivity for servers

4. **Next-Generation Antivirus (NGAV) Features:**
   - Deploy machine learning-based detection
   - Enable exploit prevention
   - Implement anti-ransomware capabilities
   - Deploy script control blocking malicious PowerShell
   - Enable network protection and firewall
   - Implement device control (USB, removable media)

5. **Centralized Management and Monitoring:**
   - Implement centralized endpoint protection console
   - Deploy real-time monitoring and alerting
   - Enable automated threat response
   - Implement endpoint isolation capabilities
   - Deploy remote investigation and remediation
   - Integrate with SIEM for correlation

6. **Effectiveness Validation:**
   - Conduct regular endpoint protection testing
   - Validate detection with MITRE ATT&CK emulation
   - Test detection against latest threat samples
   - Conduct purple team exercises
   - Measure and track detection rates
   - Continuous improvement based on testing
   - Regular evaluation of emerging endpoint protection technologies
