# INF-01.08 — IPv6 and Dual-Stack Surface Analysis

## Summary

This test identifies IPv6 network implementation, enumerates IPv6-enabled hosts and services, and assesses whether IPv6 introduces additional attack surface or security control bypasses in dual-stack environments.

## Risk

IPv6 implementations frequently lack equivalent security controls compared to IPv4 networks, creating unmonitored attack vectors. Organizations deploy dual-stack networks without extending firewall rules, intrusion detection, logging, or network segmentation to IPv6 traffic. Attackers exploit IPv6 as bypass mechanism to evade IPv4-focused security controls. Unmanaged IPv6 autoconfiguration enables rogue networks and man-in-the-middle attacks. IPv6 address space size hinders traditional scanning but enables hiding of malicious traffic in vast address ranges. Organizations face breaches through IPv6 vectors while security monitoring focuses exclusively on IPv4.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network connectivity with IPv6 capability exists or can be tested
- IPv4 network discovery has been completed (see INF-01.01, INF-01.02)
- Testing approach permits IPv6 network probing

## Test Objectives

- Identify IPv6 network presence and dual-stack implementations
- Enumerate IPv6-enabled hosts and services
- Assess IPv6 security control parity with IPv4 infrastructure
- Test for IPv6-based security control bypasses
- Evaluate IPv6 network segmentation and isolation

## Test Methodology

1. Determine IPv6 network availability:
   - Test local IPv6 connectivity and addressing
   - Verify IPv6 routing and gateway presence
   - Identify whether environment is IPv4-only, IPv6-only, or dual-stack
   - Check for IPv6 DHCP (DHCPv6) or SLAAC (Stateless Address Autoconfiguration)
2. Enumerate IPv6 address allocations and prefixes:
   - Identify IPv6 subnet prefixes in use
   - Query DHCPv6 servers for address assignment information
   - Analyze Router Advertisement (RA) messages for prefix information
   - Document IPv6 addressing scheme and allocation ranges
3. Perform IPv6 host discovery:
   - Scan link-local addresses (fe80::/10) on directly connected networks
   - Probe multicast addresses (ff02::1 all-nodes, ff02::2 all-routers)
   - Enumerate IPv6 hosts using ICMPv6 neighbor solicitation
   - Attempt IPv6 address range scanning where feasible
   - Query DNS for AAAA records indicating IPv6-enabled hosts
4. Identify IPv6-enabled services:
   - Port scan discovered IPv6 hosts for exposed services
   - Compare IPv6 service exposure with IPv4 service exposure
   - Identify services accessible only via IPv6
   - Test for inconsistent service filtering between IPv4 and IPv6
5. Test IPv6 security control parity:
   - Verify firewall rules apply to both IPv4 and IPv6 traffic
   - Test whether intrusion detection systems monitor IPv6
   - Assess logging coverage for IPv6 network activity
   - Compare network segmentation implementation between IPv4 and IPv6
   - Test access controls on services via IPv6 versus IPv4
6. Evaluate IPv6-specific attack vectors:
   - Test for rogue Router Advertisement (RA) acceptance
   - Attempt DHCPv6 spoofing or server impersonation
   - Test IPv6 tunneling mechanisms (6to4, Teredo, ISATAP)
   - Evaluate fragmentation handling and reassembly security
   - Test for IPv6 extension header manipulation
7. Assess IPv6 network segmentation:
   - Verify VLAN isolation extends to IPv6 traffic
   - Test routing between IPv6 network segments
   - Identify IPv6 connectivity between security zones
   - Compare IPv6 segmentation with documented IPv4 architecture
8. Test transition mechanisms:
   - Identify IPv6 over IPv4 tunneling (6to4, 6rd, 6in4)
   - Test Teredo tunneling if Windows hosts present
   - Evaluate ISATAP implementation in enterprise environments
   - Assess whether transition mechanisms bypass security controls

## Expected Secure Configuration

Organizations deploying IPv6 should implement:

- IPv6 explicitly enabled with configured addressing or disabled network-wide
- Firewall rules covering both IPv4 and IPv6 traffic equivalently
- Network segmentation and VLANs enforced for IPv6
- Intrusion detection and prevention system monitoring of IPv6 traffic
- Centralized logging capturing IPv6 network activity
- IPv6 Router Advertisement (RA) guard on switch ports
- DHCPv6 snooping preventing rogue DHCP servers
- Disabled or controlled IPv6 transition mechanisms (tunneling)
- IPv6 address management and allocation tracking
- Regular security assessment of IPv6 implementations
- Network documentation including IPv6 addressing and architecture

## Evidence to Collect

- IPv6 network presence confirmation and configuration details
- Identified IPv6 address prefixes and allocation schemes
- List of IPv6-enabled hosts with addresses
- IPv6 service enumeration results and port scans
- Comparison matrix of IPv4 versus IPv6 service exposure
- Firewall rule analysis for IPv4 and IPv6 parity
- IDS/IPS monitoring capability assessment for IPv6
- IPv6 network segmentation validation results
- IPv6 transition mechanism identification
- Router Advertisement and DHCPv6 security testing results
- Logging and monitoring coverage analysis for IPv6 traffic

## Impact

**Security Control Bypass Impact:**
IPv6 implementations without equivalent security controls enable attackers to bypass IPv4-focused firewalls, IDS, access controls, and monitoring. Services blocked on IPv4 remain accessible via IPv6. Network segmentation enforced for IPv4 fails to isolate IPv6 traffic. Attacks conducted over IPv6 evade detection systems monitoring only IPv4.

**Unmonitored Attack Vector Impact:**
Lack of IPv6 logging and monitoring creates blind spot for security operations. Malicious activity over IPv6 proceeds undetected. Command and control channels, data exfiltration, and lateral movement using IPv6 escape SIEM correlation. Incident response cannot reconstruct IPv6-based attacks due to missing logs.

**Rogue Network and MITM Impact:**
IPv6 autoconfiguration mechanisms without protection enable rogue Router Advertisements and DHCPv6 servers. Attackers inject malicious routing or DNS information. Man-in-the-middle attacks succeed through routing manipulation. Network availability suffers from routing disruption.

## MITRE ATT&CK Mapping

- **T1590.005** - Reconnaissance: Gather Victim Network Information: IP Addresses
- **T1557** - Credential Access: Man-in-the-Middle (IPv6 RA/DHCPv6 attacks)
- **T1599.001** - Defense Evasion: Network Boundary Bridging: Network Address Translation Traversal (IPv6 tunneling)

## Remediation Guidance

Organizations should implement the following IPv6 security practices:

1. **IPv6 Policy and Planning:**
   - Develop organizational IPv6 deployment policy
   - Decide whether to enable IPv6 or explicitly disable it
   - If disabling IPv6, do so comprehensively across all systems and network devices
   - If enabling IPv6, plan full security control implementation
   - Avoid partial or unmanaged IPv6 deployment

2. **Security Control Parity:**
   - Extend all IPv4 firewall rules to IPv6 equivalents
   - Configure stateful firewalls for IPv6 with equivalent policies
   - Deploy IDS/IPS monitoring for IPv6 traffic
   - Enable logging for all IPv6 network activity
   - Implement network segmentation for IPv6 matching IPv4 architecture
   - Apply access controls consistently across IPv4 and IPv6

3. **IPv6-Specific Protections:**
   - Deploy RA Guard on all switch ports preventing rogue Router Advertisements
   - Implement DHCPv6 snooping preventing unauthorized DHCP servers
   - Configure IPv6 Source Guard preventing address spoofing
   - Disable unnecessary IPv6 extension headers at network boundaries
   - Implement IPv6 ingress and egress filtering

4. **Transition Mechanism Control:**
   - Disable IPv6 transition mechanisms if not required (Teredo, 6to4, ISATAP)
   - If transition mechanisms required, implement strict controls and monitoring
   - Block Teredo tunneling at firewalls (UDP port 3544)
   - Monitor for unauthorized IPv6 tunneling protocols
   - Disable IPv6 autoconfiguration on enterprise networks; use DHCPv6

5. **IPv6 Monitoring and Management:**
   - Deploy IPv6 address management (IPAM) tracking assignments
   - Implement IPv6 network discovery and asset inventory
   - Enable comprehensive IPv6 logging in SIEM
   - Create IPv6-specific monitoring dashboards and alerts
   - Conduct regular IPv6 security assessments
   - Train security operations staff on IPv6 threat landscape and investigation techniques
