# INF-01.09 — Identification of Security Appliances and Segmentation Points

## Summary

This test identifies network security appliances, enforcement points, and logical boundaries that implement security controls, monitoring, or traffic filtering within the infrastructure.

## Risk

Unidentified security appliances and segmentation points result in incomplete understanding of security architecture and control placement. Attackers identify and exploit gaps in security appliance coverage, bypass security controls through alternative network paths, or evade detection by avoiding monitored network segments. Misplaced trust in non-existent or misconfigured security controls creates false confidence in security posture. Organizations face breaches when attack paths traverse unprotected network segments or bypass security appliances believed to provide coverage.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network discovery and host enumeration completed (see INF-01.01, INF-01.02)
- Network topology understanding from previous discovery
- Testing approach permits security control identification

## Test Objectives

- Identify firewalls, security appliances, and network enforcement points
- Map network segmentation boundaries and security zones
- Determine security control placement and coverage areas
- Assess effectiveness of traffic filtering and monitoring
- Identify gaps or bypasses in security architecture

## Test Methodology

1. Identify firewall and filtering devices from network discovery:
   - Analyze network traces showing dropped or filtered traffic
   - Identify TTL decrements indicating routing hops
   - Observe TCP reset (RST) responses suggesting stateful filtering
   - Detect ICMP filtering or rate limiting behaviors
   - Identify asymmetric routing or traffic redirection
2. Enumerate specific security appliance types:
   - **Firewalls:** Next-generation firewalls (NGFW), stateful packet filters, web application firewalls (WAF)
   - **Intrusion Prevention Systems (IPS):** Network-based IPS (NIPS), inline security gateways
   - **Load Balancers:** Application delivery controllers (ADC), reverse proxies
   - **VPN Gateways:** SSL VPN, IPsec VPN concentrators
   - **Network Access Control (NAC):** 802.1X authentication points
   - **DDoS Protection:** Traffic scrubbing appliances, rate limiting devices
   - **SSL/TLS Inspection:** HTTPS interception proxies, SSL visibility appliances
3. Map network segmentation boundaries:
   - Identify DMZ (demilitarized zone) network segments
   - Map internal network security zones (corporate, server, database, management)
   - Identify trust boundaries between zones
   - Document VLAN segmentation and isolation
   - Map physical or logical air gaps
4. Test traffic filtering effectiveness:
   - Probe for allowed versus blocked ports and protocols
   - Test protocol-level filtering (application-layer filtering)
   - Identify default-allow versus default-deny policies
   - Test for filter bypass techniques (protocol smuggling, tunneling)
   - Verify bidirectional filtering (ingress and egress)
5. Identify monitoring and visibility points:
   - Detect network taps or SPAN port locations
   - Identify intrusion detection system (IDS) placement
   - Locate network packet brokers or aggregation points
   - Determine SIEM log collection points
   - Identify NetFlow or network telemetry collection
6. Assess lateral movement restrictions:
   - Test connectivity between security zones
   - Identify allowed communication paths between segments
   - Test for microsegmentation implementation
   - Evaluate east-west traffic filtering
   - Identify flat network segments without internal segmentation
7. Identify security control gaps:
   - Map network paths bypassing security appliances
   - Test alternative routes avoiding monitored segments
   - Identify unfiltered communication paths
   - Document network segments lacking security controls
   - Test for inconsistent policy enforcement across redundant devices
8. Analyze security architecture design:
   - Evaluate defense-in-depth implementation
   - Assess single points of failure in security architecture
   - Identify reliance on perimeter-only security
   - Evaluate security control redundancy and failover
   - Compare security architecture to documented designs

## Expected Secure Configuration

Organizations implementing network security architecture should maintain:

- Comprehensive network security architecture documentation with appliance locations
- Network diagrams showing security zones and trust boundaries
- Default-deny firewall policies with explicit allow rules
- Defense-in-depth with multiple security layers
- Network segmentation isolating critical assets and limiting lateral movement
- Security appliances deployed at all trust boundaries
- Consistent policy enforcement across redundant security devices
- Monitoring coverage at all critical network segments
- Regular security architecture reviews validating design effectiveness
- Testing of security control effectiveness and coverage

## Evidence to Collect

- Identified security appliances with types, locations, and IP addresses
- Network security zone map showing segmentation boundaries
- Traffic filtering test results documenting allowed versus blocked communications
- Traceroute outputs showing security appliance positions
- Security control gap analysis documenting unprotected paths
- Network diagrams annotated with security appliance placement
- Policy effectiveness testing results for each security boundary
- Monitoring and visibility point identification
- Comparison of discovered architecture versus documented design
- Alternative network path analysis showing potential bypasses

## Impact

**Security Control Bypass Impact:**
Unidentified gaps in security appliance coverage enable attackers to bypass filtering, monitoring, and access controls. Alternative network paths avoiding security zones provide unmonitored lateral movement. Flat network segments without internal segmentation allow unrestricted attacker movement after initial compromise. Perimeter-only security fails when attackers operate inside network boundaries.

**False Security Posture Impact:**
Organizations develop false confidence in security when documented controls do not match actual implementation. Security appliances believed to provide coverage may be misconfigured, bypassed, or non-functional. Security architecture gaps remain undetected until exploited during attacks. Incident response assumes monitoring coverage that does not exist, hindering forensics and containment.

**Lateral Movement and Escalation Impact:**
Lack of internal segmentation enables rapid lateral movement throughout networks after initial compromise. Attackers access critical systems directly without crossing security boundaries. Privilege escalation occurs when administrative systems lack isolation from general user networks. Data exfiltration succeeds without crossing monitored egress points.

## MITRE ATT&CK Mapping

- **T1590.006** - Reconnaissance: Gather Victim Network Information: Network Security Appliances
- **T1599** - Defense Evasion: Network Boundary Bridging
- **T1021** - Lateral Movement: Remote Services

## Remediation Guidance

Organizations should implement the following network security architecture practices:

1. **Security Architecture Documentation:**
   - Maintain current network security architecture diagrams
   - Document all security appliance locations and purposes
   - Define security zones with clear trust boundaries
   - Map traffic flows between zones with security controls
   - Version control architecture documentation with change tracking

2. **Defense-in-Depth Implementation:**
   - Deploy security controls at multiple layers (network, host, application)
   - Implement network segmentation with firewall enforcement
   - Deploy both perimeter and internal security controls
   - Avoid reliance on single security layer or appliance
   - Implement redundant security controls for critical boundaries

3. **Network Segmentation:**
   - Segregate network into security zones based on trust and function
   - Implement VLAN segmentation with firewall enforcement between VLANs
   - Deploy microsegmentation for critical assets
   - Isolate administrative networks from general user networks
   - Segment production from development and testing environments
   - Use separate networks for guest, contractor, and IoT devices

4. **Security Appliance Deployment:**
   - Deploy firewalls at all trust boundary crossing points
   - Implement intrusion prevention systems (IPS) inline for critical segments
   - Deploy web application firewalls (WAF) protecting web applications
   - Position security controls for both north-south and east-west traffic
   - Implement consistent policies across redundant security devices
   - Regular testing of security appliance effectiveness and coverage

5. **Monitoring and Visibility:**
   - Deploy network monitoring covering all security zones
   - Implement intrusion detection systems (IDS) with comprehensive coverage
   - Collect NetFlow or network telemetry from all segments
   - Deploy SIEM with log collection from all security appliances
   - Implement security analytics for east-west traffic visibility
   - Conduct regular validation that monitoring systems are functioning and providing expected visibility
