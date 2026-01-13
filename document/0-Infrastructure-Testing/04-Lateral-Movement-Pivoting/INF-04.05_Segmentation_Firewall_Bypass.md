# INF-04.05 — Segmentation or Firewall Bypass Techniques

## Summary

This test identifies techniques and vulnerabilities enabling attackers to bypass network segmentation boundaries and firewall rules, undermining perimeter and internal security controls.

## Risk

Network segmentation and firewalls form critical security boundaries protecting sensitive systems and data. Bypass techniques circumvent these controls enabling unauthorized network access. Application-layer attacks traverse firewalls inspecting only network-layer traffic. Protocol manipulation exploits firewall rule misconfigurations. IPv6 tunneling bypasses IPv4-only firewall rules. Misconfigured firewall rules create unintended access paths. Organizations face complete segmentation failure when bypass techniques enable unrestricted network traversal despite deployed security controls.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network segmentation architecture has been identified (see INF-01.09)
- Firewall rules and security zones are documented
- Testing includes segmentation bypass assessment

## Test Objectives

- Identify firewall bypass techniques and vulnerabilities
- Test application-layer attacks bypassing network controls
- Assess protocol manipulation for segmentation bypass
- Evaluate IPv6 as bypass vector for IPv4 firewalls
- Validate defense-in-depth against bypass attempts

## Test Methodology

1. Test application-layer bypass techniques:
   - **HTTP/HTTPS Tunneling:** Encapsulate protocols within allowed HTTP/HTTPS
   - **DNS Tunneling:** Data transfer through DNS queries and responses
   - **Web-based Protocols:** Abuse WebSocket, WebRTC for real-time communication
   - Test application proxies bypassing network firewalls
   - Verify web application firewalls (WAF) can be circumvented
2. Assess IPv6 bypass of IPv4 controls:
   - Test IPv6 connectivity when IPv4 is firewalled
   - Verify IPv6 firewall rule parity with IPv4 rules
   - Test IPv6 tunneling mechanisms (6to4, Teredo, ISATAP)
   - Assess dual-stack systems for inconsistent filtering
   - Verify monitoring coverage for both IPv4 and IPv6
3. Test protocol manipulation and fragmentation:
   - **IP Fragmentation:** Fragment packets evading inspection
   - **TCP Segmentation:** Split payloads across multiple packets
   - **Protocol Misidentification:** Disguise protocols to bypass rules
   - Test firewall evasion through packet manipulation
   - Assess deep packet inspection (DPI) bypass techniques
4. Evaluate firewall rule misconfiguration exploitation:
   - Test overly permissive "any-any" rules
   - Identify unintended rule interactions creating access
   - Test firewall rule order and shadowing
   - Assess NAT and port forwarding misconfigurations
   - Verify firewall exception and whitelist abuse
5. Test network address translation (NAT) traversal:
   - **STUN (Session Traversal Utilities for NAT):** Discover public IP and port
   - **TURN (Traversal Using Relays around NAT):** Relay traffic through external server
   - **ICE (Interactive Connectivity Establishment):** Establish peer connections
   - Test NAT hole punching techniques
   - Assess firewall effectiveness against NAT traversal
6. Assess VPN and encrypted tunnel bypass:
   - Test VPN establishment from restricted networks
   - Assess SSL VPN bypass through allowed HTTPS
   - Verify detection of unauthorized VPN usage
   - Test anonymization networks (Tor, I2P) for bypass
   - Assess firewall effectiveness against encrypted tunneling
7. Test physical and wireless bypass vectors:
   - Assess wireless network access bypassing wired segmentation
   - Test rogue wireless access points bridging networks
   - Verify physical port security and access controls
   - Test USB and removable media as segmentation bypass
   - Assess visitor and guest network isolation
8. Evaluate multi-path and redundant link exploitation:
   - Test backup network connections bypassing primary controls
   - Assess redundant WAN links with inconsistent security
   - Verify failover paths maintain security controls
   - Test load balancers and redundant firewalls for bypass
   - Assess asymmetric routing creating bypass opportunities

## Expected Secure Configuration

Organizations implementing network segmentation should maintain:

- Firewall rules applied consistently to IPv4 and IPv6
- Deep packet inspection (DPI) detecting protocol tunneling
- Application-layer filtering beyond network-layer controls
- Default-deny firewall policies with explicit allow rules
- Regular firewall rule reviews eliminating unnecessary access
- Monitoring and alerting on bypass attempts and anomalies
- Egress filtering preventing outbound tunneling and proxying
- Defense-in-depth with multiple security layers
- Wireless network isolation from corporate networks
- Physical security preventing unauthorized network access

## Evidence to Collect

- Application-layer bypass testing results
- IPv6 bypass capability assessment
- Protocol manipulation and fragmentation testing results
- Firewall rule misconfiguration identification
- NAT traversal testing outcomes
- VPN and encrypted tunnel bypass validation
- Physical and wireless bypass vector analysis
- Multi-path exploitation testing results
- Detection capability validation for bypass attempts
- Defense-in-depth effectiveness assessment

## Impact

**Complete Segmentation Bypass Impact:**
Successful bypass techniques nullify network segmentation enabling unauthorized access to protected systems. Attackers access databases, domain controllers, and critical infrastructure despite firewall protections. Application-layer bypasses traverse network firewalls inspecting only packets. Organizations lose primary security boundary when segmentation can be circumvented.

**Undetected Network Traversal Impact:**
Encrypted tunneling and protocol manipulation evade traditional network monitoring. IPv6 bypass operates in blind spots of IPv4-focused security. Application-layer attacks appear as legitimate traffic. Security operations lack visibility into bypass activities preventing detection and response.

**Defense-in-Depth Failure Impact:**
Single-layer security fails when firewalls can be bypassed. Organizations relying exclusively on network segmentation face complete exposure. Lack of host-based controls enables unrestricted access once network boundaries are crossed. Defense-in-depth absence magnifies bypass impact.

## MITRE ATT&CK Mapping

- **T1599** - Defense Evasion: Network Boundary Bridging
- **T1090** - Command and Control: Proxy
- **T1572** - Command and Control: Protocol Tunneling
- **T1095** - Command and Control: Non-Application Layer Protocol
- **T1205** - Defense Evasion: Traffic Signaling

## Remediation Guidance

Organizations should implement the following segmentation bypass prevention practices:

1. **Defense-in-Depth Architecture:**
   - Deploy multiple security layers beyond network firewalls
   - Implement host-based firewalls on all endpoints
   - Deploy application-layer security controls
   - Use network access control (NAC) supplementing firewalls
   - Implement zero trust architecture eliminating implicit trust
   - Deploy microsegmentation at host and application levels

2. **IPv4 and IPv6 Parity:**
   - Implement identical firewall rules for IPv4 and IPv6
   - Deploy IPv6-aware security appliances
   - Monitor IPv6 traffic with same rigor as IPv4
   - Disable IPv6 if not required for operations
   - Block IPv6 tunneling mechanisms (Teredo, 6to4, ISATAP)
   - Regular testing of IPv6 segmentation effectiveness

3. **Application-Layer Security:**
   - Deploy web application firewalls (WAF) for HTTP/HTTPS traffic
   - Implement SSL/TLS inspection at network boundaries
   - Deploy application-aware next-generation firewalls
   - Use application control and whitelisting
   - Implement API gateways with authentication and authorization
   - Monitor application-layer protocols for anomalies

4. **Protocol Inspection and Filtering:**
   - Deploy deep packet inspection (DPI) analyzing payloads
   - Implement protocol validation preventing manipulation
   - Block fragmented packets at security boundaries
   - Deploy intrusion prevention systems (IPS) detecting evasion
   - Use stateful firewalls tracking connection states
   - Regular updates to inspection signatures and rules

5. **Egress Filtering and Control:**
   - Implement strict egress filtering on outbound traffic
   - Whitelist permitted external destinations
   - Block VPN and tunneling protocols from workstations
   - Deploy web proxy requiring authentication
   - Monitor DNS for tunneling and exfiltration
   - Block anonymization networks (Tor, I2P, VPN services)

6. **Firewall Rule Governance:**
   - Implement firewall rule review process (quarterly minimum)
   - Remove unused and outdated firewall rules
   - Document business justification for each rule
   - Implement change management for firewall modifications
   - Test firewall rules for unintended access paths
   - Deploy automated firewall rule analysis tools
   - Eliminate overly permissive "any-any" rules
