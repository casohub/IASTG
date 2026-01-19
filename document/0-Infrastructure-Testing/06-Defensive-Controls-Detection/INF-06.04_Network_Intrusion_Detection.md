# INF-06.04 — Network Intrusion Detection and Alerting (NIDS/NIPS)

## Summary

This test evaluates the deployment, configuration, and effectiveness of network intrusion detection systems (NIDS) and network intrusion prevention systems (NIPS) in detecting and preventing network-based attacks.

## Risk

Absent or ineffective network intrusion detection prevents identification of network-based attacks including exploitation, lateral movement, and data exfiltration. NIDS/NIPS gaps create blind spots enabling undetected malicious network traffic. Misconfigured detection systems generate excessive false positives overwhelming security operations. Bypass techniques evade detection through encryption and protocol manipulation. Organizations face undetected network attacks when intrusion detection is inadequate or absent.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network topology and segmentation understood
- Access to NIDS/NIPS management consoles
- Testing includes network security assessment

## Test Objectives

- Assess NIDS/NIPS deployment coverage
- Evaluate detection rule effectiveness
- Test alerting and response capabilities
- Identify detection blind spots and gaps
- Validate signature and anomaly detection

## Test Methodology

1. Enumerate NIDS/NIPS deployment:
   - Identify deployed solutions (Snort, Suricata, commercial IDS/IPS)
   - Map sensor placement and network coverage
   - Assess monitoring of critical network segments
   - Verify span port or tap deployment
   - Review inline vs passive deployment
2. Test network coverage and blind spots:
   - Verify monitoring of Internet-facing traffic
   - Assess internal network segment monitoring
   - Test coverage of DMZ and critical zones
   - Identify unmonitored network paths
   - Verify encrypted traffic inspection capabilities
3. Evaluate signature-based detection:
   - Test detection of known exploit attempts
   - Verify detection of malware command-and-control traffic
   - Assess detection of vulnerability scanning
   - Test detection of common attack tools (Metasploit, Cobalt Strike)
   - Verify signature update frequency and automation
4. Test anomaly-based detection:
   - Verify baseline traffic profiling
   - Test detection of unusual traffic patterns
   - Assess detection of protocol anomalies
   - Verify detection of traffic volume anomalies
   - Test detection of unauthorized services
5. Assess evasion and bypass techniques:
   - Test fragmentation evasion
   - Verify detection through protocol tunneling
   - Test obfuscation and encoding bypasses
   - Assess encrypted traffic inspection
   - Verify timing-based evasion detection
6. Evaluate alerting and response:
   - Test alert generation for detected threats
   - Verify alert severity classification
   - Assess alert correlation and deduplication
   - Test integration with SIEM
   - Verify automated response capabilities (for NIPS)
7. Test intrusion prevention capabilities:
   - Verify inline blocking functionality
   - Test false positive impact on legitimate traffic
   - Assess blocking performance and latency
   - Verify bypass mechanisms for maintenance
   - Test failopen vs failclosed behavior
8. Assess performance and scalability:
   - Test detection at line-rate throughput
   - Assess performance during peak traffic
   - Verify rule processing efficiency
   - Test sensor capacity and headroom
   - Assess impact on network latency

## Expected Secure Configuration

Organizations deploying NIDS/NIPS should maintain:

- Comprehensive network coverage with minimal blind spots
- Strategic sensor placement at security boundaries
- Current signature and rule sets with daily updates
- Anomaly detection baselines regularly updated
- Encrypted traffic inspection where feasible
- Integration with SIEM for correlation
- Appropriate alerting thresholds minimizing false positives
- Documented tuning and maintenance procedures
- Regular effectiveness validation and testing
- 24/7 security operations monitoring alerts

## Evidence to Collect

- NIDS/NIPS deployment architecture and coverage map
- Sensor placement and network segment coverage
- Detection rule and signature inventory
- Attack detection testing results
- Evasion and bypass testing outcomes
- Alert generation and quality assessment
- False positive and false negative analysis
- Performance and capacity metrics
- Integration verification with SIEM
- Recommendations for detection improvement

## Impact

**Undetected Network Attacks Impact:**
Absent or ineffective NIDS/NIPS prevents detection of network-based exploitation, malware downloads, and command-and-control communications. Attackers exploit vulnerabilities undetected. Malware establishes persistence through network channels. Organizations discover breaches only after significant compromise without network intrusion detection.

**Lateral Movement and Data Exfiltration Impact:**
Lack of internal network monitoring enables undetected lateral movement. Attackers move between systems without triggering alerts. Data exfiltration proceeds unnoticed without network anomaly detection. Organizations lose ability to detect internal threats and compromised credentials enabling lateral movement.

**Alert Fatigue and False Positive Impact:**
Poorly tuned NIDS/NIPS generates excessive false positives overwhelming security operations. Analysts ignore alerts due to alert fatigue. True positives hidden in noise of false alerts. Organizations miss critical attacks when detection systems cry wolf repeatedly.

## MITRE ATT&CK Mapping

Network intrusion detection addresses multiple tactics:
- **TA0001** - Initial Access (detecting exploitation)
- **TA0008** - Lateral Movement (internal network attacks)
- **TA0011** - Command and Control (C2 traffic detection)
- **TA0010** - Exfiltration (data theft detection)

## Remediation Guidance

Organizations should implement the following NIDS/NIPS best practices:

1. **Strategic Deployment and Coverage:**
   - Deploy sensors at all security boundaries
   - Monitor Internet ingress and egress traffic
   - Deploy internal sensors monitoring east-west traffic
   - Cover DMZ and critical network segments
   - Implement network taps or span ports appropriately
   - Ensure redundancy for critical monitoring points

2. **Signature and Rule Management:**
   - Deploy commercial or open-source rule sets (Emerging Threats, Snort rules)
   - Enable automatic daily signature updates
   - Implement custom rules for environment-specific threats
   - Regular rule effectiveness validation
   - Remove obsolete or ineffective rules
   - Tune rules reducing false positives

3. **Anomaly Detection Enhancement:**
   - Implement baseline traffic profiling
   - Deploy behavioral analytics for network traffic
   - Enable protocol anomaly detection
   - Monitor traffic volume and rate anomalies
   - Detect new services and ports
   - Regular baseline updates reflecting environment changes

4. **Encrypted Traffic Handling:**
   - Deploy SSL/TLS inspection where feasible
   - Implement certificate inspection and validation
   - Monitor encrypted traffic metadata (flow analysis)
   - Deploy DNS monitoring for encrypted traffic destinations
   - Use cloud proxy services for inspection
   - Balance privacy concerns with security needs

5. **Alert Management and Integration:**
   - Integrate NIDS/NIPS with SIEM for correlation
   - Implement alert severity classification
   - Deploy alert aggregation and deduplication
   - Configure actionable alerting thresholds
   - Enable automated enrichment with threat intelligence
   - Implement alert escalation procedures
   - Regular tuning reducing false positives

6. **Detection Effectiveness Validation:**
   - Conduct regular detection testing with tools (Atomic Red Team)
   - Test detection against MITRE ATT&CK techniques
   - Simulate attacks validating detection
   - Conduct purple team exercises
   - Measure and track detection rates
   - Continuous improvement based on testing gaps
   - Evaluate emerging detection technologies and techniques
