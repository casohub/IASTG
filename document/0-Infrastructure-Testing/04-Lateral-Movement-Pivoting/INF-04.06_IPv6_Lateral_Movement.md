# INF-04.06 — IPv6-Based Lateral Movement

## Summary

This test assesses the use of IPv6 protocols and addressing for lateral movement, examining whether IPv6 creates security blind spots enabling undetected system-to-system traversal.

## Risk

IPv6 implementations frequently lack security controls equivalent to IPv4 infrastructure creating lateral movement opportunities. Organizations deploy IPv4 firewalls and monitoring without extending protection to IPv6. Attackers leverage IPv6 for lateral movement evading IPv4-focused security controls. IPv6 autoconfiguration enables rogue networks and man-in-the-middle attacks. Link-local IPv6 addressing provides direct system-to-system connectivity bypassing network controls. Organizations face undetected lateral movement when IPv6 operates as shadow network without equivalent security oversight.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- IPv6 implementation has been identified (see INF-01.08)
- Multiple systems exist in testing scope
- Testing includes IPv6 security assessment

## Test Objectives

- Assess IPv6 lateral movement capabilities
- Test security control parity between IPv4 and IPv6
- Evaluate IPv6 autoconfiguration exploitation
- Validate IPv6 monitoring and detection coverage
- Test IPv6-based attack techniques

## Test Methodology

1. Enumerate IPv6 network presence and configuration:
   - Identify IPv6-enabled systems and services
   - Test IPv6 connectivity between systems
   - Verify IPv6 addressing schemes (SLAAC, DHCPv6, static)
   - Assess IPv6 network segmentation
   - Document IPv6 routing and gateway configuration
2. Test IPv6 link-local lateral movement:
   - Enumerate systems via link-local addresses (fe80::/10)
   - Test direct system-to-system connectivity using link-local
   - Assess link-local communication bypassing network controls
   - Verify firewall rules apply to link-local traffic
   - Test lateral movement through link-local addressing
3. Assess IPv6 service accessibility:
   - Scan for services accessible via IPv6
   - Compare IPv6 versus IPv4 service exposure
   - Test for services accessible only through IPv6
   - Verify authentication and access controls on IPv6
   - Assess inconsistent filtering between IPv4 and IPv6
4. Test IPv6 Router Advertisement (RA) attacks:
   - Deploy rogue Router Advertisement messages
   - Test default gateway hijacking through RA
   - Assess DNS server poisoning via RA
   - Verify RA Guard implementation preventing rogue RAs
   - Test man-in-the-middle through rogue IPv6 router
5. Evaluate IPv6 neighbor discovery exploitation:
   - Test Neighbor Solicitation/Advertisement spoofing
   - Assess IPv6 address spoofing capabilities
   - Test Duplicate Address Detection (DAD) denial of service
   - Verify Secure Neighbor Discovery (SEND) implementation
   - Test neighbor cache poisoning attacks
6. Test IPv6 tunneling for lateral movement:
   - Assess Teredo tunneling enabling IPv6 over IPv4
   - Test 6to4 and 6rd tunneling mechanisms
   - Evaluate ISATAP tunneling in enterprise environments
   - Verify tunnel detection and blocking
   - Test lateral movement through IPv6 tunnels
7. Assess IPv6 firewall and monitoring:
   - Verify firewall rules apply to IPv6 traffic
   - Test intrusion detection system (IDS) coverage for IPv6
   - Assess logging of IPv6 network activity
   - Verify SIEM integration of IPv6 logs
   - Test detection of IPv6-based lateral movement
8. Evaluate IPv6 extension header exploitation:
   - Test fragmentation header manipulation
   - Assess routing header attacks (type 0 routing header)
   - Verify extension header filtering at boundaries
   - Test firewall evasion through extension headers
   - Assess detection of malicious extension header usage

## Expected Secure Configuration

Organizations implementing IPv6 should maintain:

- IPv6 firewall rules matching IPv4 rule coverage
- IPv6 traffic monitoring and logging equivalent to IPv4
- Router Advertisement Guard on all switch ports
- Disabled IPv6 tunneling mechanisms (Teredo, ISATAP, 6to4)
- Secure Neighbor Discovery (SEND) or alternative protections
- IPv6 intrusion detection and prevention systems
- Network segmentation enforced for IPv6
- Host-based firewalls configured for IPv6
- Regular IPv6 security assessments
- IPv6 disabled if not required for operations

## Evidence to Collect

- IPv6 network enumeration results
- Link-local lateral movement testing outcomes
- IPv6 service accessibility comparison with IPv4
- Router Advertisement attack testing results
- Neighbor discovery exploitation validation
- IPv6 tunneling identification and testing
- Firewall and monitoring coverage assessment for IPv6
- Extension header exploitation testing results
- Detection capability validation for IPv6 lateral movement
- Lateral movement path documentation via IPv6

## Impact

**Unmonitored Lateral Movement Impact:**
IPv6 lateral movement proceeds undetected when security monitoring focuses exclusively on IPv4. Attackers move between systems using IPv6 addressing evading network-based detection. Link-local IPv6 provides direct system connectivity bypassing network firewalls. Security operations lack visibility into IPv6 activities preventing incident detection and response.

**Security Control Bypass Impact:**
IPv4 firewall rules fail to restrict IPv6 traffic enabling unrestricted lateral movement. Network segmentation implemented for IPv4 does not apply to IPv6. Intrusion prevention systems monitoring IPv4 miss IPv6-based attacks. Organizations face complete security control bypass when IPv6 operates without equivalent protections.

**Rogue Network and MITM Impact:**
IPv6 Router Advertisement attacks enable network hijacking and man-in-the-middle positioning. Attackers inject malicious routing information through rogue RAs. DNS poisoning via RA redirects traffic to attacker-controlled systems. Neighbor Discovery spoofing enables address resolution attacks and traffic interception.

## MITRE ATT&CK Mapping

- **T1021** - Lateral Movement: Remote Services
- **T1557** - Credential Access: Man-in-the-Middle (IPv6 RA attacks)
- **T1599.001** - Defense Evasion: Network Boundary Bridging: Network Address Translation Traversal
- **T1205** - Defense Evasion: Traffic Signaling

## Remediation Guidance

Organizations should implement the following IPv6 security practices:

1. **IPv6 Security Parity:**
   - Implement firewall rules for IPv6 matching IPv4 coverage
   - Deploy IPv6-aware intrusion detection and prevention
   - Extend network segmentation to IPv6 infrastructure
   - Monitor IPv6 traffic with same rigor as IPv4
   - Apply access controls consistently across IPv4 and IPv6
   - Regular testing of IPv6 security control effectiveness

2. **IPv6 Deployment Strategy:**
   - Disable IPv6 if not required for business operations
   - Plan comprehensive IPv6 security before enabling
   - Deploy IPv6 with full security controls or not at all
   - Avoid partial or unmanaged IPv6 deployment
   - Document IPv6 business requirements and security plan
   - Conduct risk assessment before IPv6 enablement

3. **IPv6 Attack Prevention:**
   - Deploy RA Guard on all switch ports preventing rogue RAs
   - Implement DHCPv6 snooping controlling address assignment
   - Deploy IPv6 Source Guard preventing address spoofing
   - Block IPv6 tunneling mechanisms (Teredo, 6to4, ISATAP)
   - Disable IPv6 router functionality on hosts
   - Implement IPv6 ACLs on network infrastructure

4. **IPv6 Extension Header Security:**
   - Filter or drop IPv6 extension headers at boundaries
   - Block fragmentation headers to prevent attacks
   - Disable IPv6 routing header type 0
   - Implement deep packet inspection for IPv6
   - Monitor for unusual extension header usage
   - Deploy IPv6-aware next-generation firewalls

5. **IPv6 Monitoring and Detection:**
   - Deploy IPv6-capable SIEM and log aggregation
   - Implement IPv6 NetFlow or IPFIX collection
   - Monitor IPv6 Neighbor Discovery traffic for anomalies
   - Alert on rogue Router Advertisements
   - Detect IPv6 tunneling and encapsulation
   - Implement behavioral analytics for IPv6 traffic
   - Create IPv6-specific detection rules and signatures

6. **Host-Based IPv6 Security:**
   - Configure host firewalls for IPv6 traffic
   - Disable IPv6 on hosts where not required
   - Implement endpoint protection monitoring IPv6
   - Deploy host intrusion prevention for IPv6
   - Configure Windows Firewall rules for IPv6
   - Harden IPv6 stack configurations on operating systems
