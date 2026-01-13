# INF-03.09 — East-West Traffic Visibility and Filtering Gaps

## Summary

This test assesses internal network traffic visibility, monitoring, and filtering between systems within the network (east-west traffic) to identify gaps in lateral movement prevention and internal threat detection.

## Risk

Lack of east-west traffic visibility creates blind spots enabling undetected lateral movement after initial compromise. Traditional perimeter-focused security ignores internal communications allowing attackers to move freely between systems once inside. Flat networks without internal segmentation permit unrestricted east-west traffic enabling rapid compromise propagation. Missing internal firewall rules fail to prevent or detect lateral movement. Unmonitored internal traffic conceals data exfiltration, command and control, and malicious activities. Organizations face extensive compromise spreading across infrastructure when east-west traffic lacks visibility and filtering.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network segmentation points have been identified (see INF-01.09)
- Network topology understanding from discovery phase
- Testing includes internal traffic analysis

## Test Objectives

- Assess internal network traffic visibility and monitoring coverage
- Identify gaps in east-west traffic filtering and segmentation
- Test for unrestricted lateral movement paths between systems
- Evaluate internal firewall rules and microsegmentation implementation
- Validate detection capabilities for internal threats

## Test Methodology

1. Assess network segmentation and internal boundaries:
   - Identify VLAN segmentation and security zones
   - Map trust boundaries between network segments
   - Test connectivity between different security zones
   - Verify firewall placement at internal boundaries
   - Document flat network segments without segmentation
2. Test east-west traffic filtering:
   - Attempt connections between systems in different zones
   - Test for default-allow versus default-deny internal policies
   - Verify protocol and port filtering between segments
   - Assess application-layer filtering on internal traffic
   - Test for bypass paths avoiding internal firewalls
3. Evaluate lateral movement restrictions:
   - Test workstation-to-workstation direct connectivity
   - Attempt server-to-workstation communications
   - Test for unrestricted administrative protocol access (RDP, SSH, WinRM)
   - Verify database access restrictions from non-application servers
   - Assess internal service accessibility without segmentation
4. Assess internal traffic visibility:
   - Verify network monitoring coverage for internal segments
   - Test for network taps or SPAN ports on internal switches
   - Assess NetFlow or network telemetry collection
   - Verify IDS/IPS coverage for east-west traffic
   - Test for blind spots in network monitoring
5. Test microsegmentation implementation:
   - Verify host-based firewall deployment and configuration
   - Test for consistent firewall policies across systems
   - Assess application-level microsegmentation
   - Verify container network policies (Kubernetes, etc.)
   - Test for microsegmentation bypass through misconfiguration
6. Evaluate detection capabilities:
   - Test whether lateral movement generates alerts
   - Verify detection of port scanning on internal networks
   - Assess alerting on unusual service access patterns
   - Test for detection of credential theft and reuse
   - Verify behavioral analytics for east-west traffic
7. Assess internal DNS and name resolution:
   - Test for split-horizon DNS implementation
   - Verify internal name resolution restrictions
   - Assess DNS monitoring for lateral movement indicators
   - Test for DNS tunneling detection on internal networks
8. Test jump host and bastion host enforcement:
   - Verify administrative access requires jump hosts
   - Test for direct administrative access bypassing bastions
   - Assess jump host logging and monitoring
   - Verify privileged access workstation (PAW) requirements
   - Test for lateral movement through jump hosts

## Expected Secure Configuration

Organizations implementing internal network security should maintain:

- Network segmentation with firewall enforcement between security zones
- Default-deny internal firewall policies with explicit allow rules
- Microsegmentation at host level preventing unrestricted lateral movement
- Comprehensive network monitoring covering all internal segments
- IDS/IPS deployment monitoring east-west traffic
- Zero trust architecture with verification at every boundary
- Jump hosts required for administrative access across segments
- NetFlow or network telemetry collection from all network devices
- Behavioral analytics detecting lateral movement patterns
- Regular testing of internal segmentation and monitoring effectiveness

## Evidence to Collect

- Network segmentation documentation and validation results
- East-west traffic filtering test results showing allowed versus blocked communications
- Lateral movement path analysis identifying unrestricted routes
- Network monitoring coverage assessment showing visibility gaps
- Microsegmentation implementation validation
- Internal firewall rule analysis
- Detection capability testing results for lateral movement
- Jump host and bastion host enforcement validation
- NetFlow or network telemetry configuration verification
- Behavioral analytics implementation assessment for internal traffic

## Impact

**Undetected Lateral Movement Impact:**
Lack of east-west visibility enables attackers to move laterally without detection after initial compromise. Flat networks permit unrestricted system-to-system access. Missing internal firewall rules fail to prevent lateral movement. Attackers access domain controllers, file servers, databases, and backup systems spreading throughout networks. Compromises remain undetected for months enabling extensive data theft.

**Rapid Compromise Propagation Impact:**
Unrestricted east-west traffic enables rapid malware spread and ransomware deployment. Worms propagate through flat networks infecting thousands of systems. Automated attacks leverage unrestricted connectivity for mass exploitation. Single compromised system leads to organization-wide infection within hours.

**Data Exfiltration Through Internal Routes Impact:**
Lack of internal traffic monitoring enables data exfiltration appearing as normal traffic. Attackers stage data on internal systems before external exfiltration. Database access from unauthorized systems goes undetected. File server access and bulk downloads lack monitoring and alerting.

## MITRE ATT&CK Mapping

- **T1021** - Lateral Movement: Remote Services
- **T1080** - Lateral Movement: Taint Shared Content
- **T1570** - Lateral Movement: Lateral Tool Transfer
- **T1210** - Lateral Movement: Exploitation of Remote Services
- **T1563** - Lateral Movement: Remote Service Session Hijacking

## Remediation Guidance

Organizations should implement the following east-west traffic security practices:

1. **Network Segmentation Implementation:**
   - Deploy VLAN segmentation separating systems by function and trust level
   - Implement firewalls at security zone boundaries
   - Create dedicated segments for workstations, servers, databases, management
   - Segregate production, development, and test environments
   - Isolate guest and contractor networks
   - Deploy separate VLANs for IoT and operational technology (OT)

2. **Internal Firewall Policy Development:**
   - Implement default-deny policies on internal firewalls
   - Create explicit allow rules for required communications only
   - Block workstation-to-workstation direct communications
   - Restrict administrative protocols (RDP, SSH, WinRM) to jump hosts only
   - Limit database access to application servers exclusively
   - Document and justify each internal firewall rule

3. **Microsegmentation Deployment:**
   - Deploy host-based firewalls on all endpoints and servers
   - Implement application-aware microsegmentation
   - Use cloud security groups or network security groups
   - Deploy Kubernetes network policies for container environments
   - Implement software-defined microsegmentation solutions
   - Enforce microsegmentation through automated policy management

4. **East-West Traffic Monitoring:**
   - Deploy network monitoring covering all internal segments
   - Implement network taps or SPAN ports on critical switches
   - Deploy IDS/IPS systems monitoring internal traffic
   - Enable NetFlow or IPFIX collection from all network devices
   - Implement packet capture for forensic analysis
   - Deploy network detection and response (NDR) solutions

5. **Zero Trust Architecture:**
   - Implement continuous verification of all internal connections
   - Deploy identity-based microsegmentation
   - Require authentication and authorization for all east-west communications
   - Implement least privilege access controls
   - Deploy service mesh for microservices zero trust
   - Verify every connection regardless of source location

6. **Detection and Response:**
   - Deploy behavioral analytics detecting lateral movement
   - Implement user and entity behavior analytics (UEBA)
   - Alert on unusual port scanning or service discovery
   - Monitor for credential theft and pass-the-hash attacks
   - Deploy deception technology (honeypots, honey credentials)
   - Integrate network monitoring with SIEM for correlation
   - Conduct regular purple team exercises testing detection
