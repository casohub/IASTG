# INF-04.03 — Pivoting via Compromised Hosts

## Summary

This test assesses the ability to use compromised systems as pivot points for accessing additional network segments, systems, or resources that are not directly reachable from the attacker's initial position.

## Risk

Pivoting through compromised hosts enables attackers to traverse network boundaries and access segmented environments bypassing perimeter security controls. Single compromised system with multiple network connections provides bridgehead into restricted networks. Attackers leverage pivot hosts to reach critical systems, databases, and management networks otherwise inaccessible. Port forwarding and tunneling through compromised systems circumvents firewall rules and segmentation. Organizations face expanded compromise scope when pivoting capabilities enable access beyond initially compromised network segments.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- At least one system has been compromised or access provided
- Multiple network segments exist in scope
- Testing includes pivoting and network traversal

## Test Objectives

- Identify systems with connectivity to multiple network segments
- Assess pivoting capabilities through compromised hosts
- Test port forwarding and tunneling for network traversal
- Evaluate network segmentation effectiveness against pivoting
- Validate detection of pivoting activities

## Test Methodology

1. Identify potential pivot hosts:
   - Systems with multiple network interfaces
   - Jump hosts and bastion hosts
   - Systems with VPN or remote access connections
   - Dual-homed hosts bridging network segments
   - Systems with both internal and DMZ connectivity
2. Test network connectivity from compromised hosts:
   - Enumerate network interfaces and IP addresses
   - Identify routing tables and gateway configurations
   - Test connectivity to different network segments
   - Discover accessible networks from compromised position
   - Map network topology from pivot host perspective
3. Implement port forwarding techniques:
   - **SSH Port Forwarding:** Local, remote, and dynamic forwarding
   - **Netsh Port Forwarding:** Windows built-in port forwarding
   - **Chisel:** HTTP tunneling tool for port forwarding
   - **Socat:** Socket relay and port forwarding
   - Test accessing restricted services through forwarded ports
4. Establish tunneling for network pivoting:
   - **SSH Tunneling:** Encrypted tunnel through compromised host
   - **HTTP/HTTPS Tunneling:** Tunneling through web proxies
   - **DNS Tunneling:** Data exfiltration and C2 through DNS
   - **ICMP Tunneling:** Covert channels through ICMP
   - **VPN Tunneling:** Establishing VPN through pivot host
5. Test SOCKS proxy capabilities:
   - Deploy SOCKS proxy on compromised host
   - Configure attack tools to use SOCKS proxy
   - Test accessing restricted networks through SOCKS
   - Verify dynamic port forwarding functionality
   - Assess performance and stability of proxy connections
6. Evaluate pivoting through application proxies:
   - Test pivoting through compromised web servers
   - Leverage application-layer proxies for network traversal
   - Assess database server connectivity to backend systems
   - Test pivoting through middleware and application servers
7. Test for detection of pivoting activities:
   - Verify network monitoring detects tunneling traffic
   - Test for detection of unusual network connections from hosts
   - Assess alerting on port forwarding or proxy activities
   - Verify behavioral analytics identify pivoting patterns
   - Test for detection of connection chaining through hosts
8. Assess segmentation bypass through pivoting:
   - Document network segments accessible via pivoting
   - Test accessing critical systems through pivot chains
   - Verify firewall effectiveness against pivoting
   - Assess whether segmentation prevents lateral movement
   - Test multi-hop pivoting through multiple compromised hosts

## Expected Secure Configuration

Organizations implementing network segmentation should maintain:

- Jump hosts and bastions as sole authorized pivot points
- Comprehensive monitoring of all network connections from systems
- Detection of tunneling and port forwarding activities
- Firewall rules preventing workstation-to-workstation pivoting
- Network access control (NAC) enforcing segmentation
- Host-based firewalls preventing unauthorized pivoting
- Egress filtering blocking outbound tunneling protocols
- Behavioral analytics detecting abnormal network connection patterns
- Regular testing of segmentation effectiveness against pivoting
- Incident response procedures for detected pivoting activities

## Evidence to Collect

- Inventory of systems with multiple network connectivity
- Pivoting capability testing results and success paths
- Port forwarding and tunneling implementation documentation
- Network segments accessible through pivoting
- Segmentation bypass analysis showing reachable restricted networks
- Detection and monitoring testing results for pivoting activities
- Network traffic captures showing tunneling or forwarding
- Multi-hop pivoting path documentation
- Firewall and segmentation effectiveness assessment
- Business impact analysis of pivoting-enabled access

## Impact

**Segmentation Bypass and Expanded Access Impact:**
Pivoting enables attackers to bypass network segmentation and access restricted systems. Single compromised workstation with VPN connection provides access to corporate network from external position. Compromised server bridging production and management networks enables administrative system access. Network segmentation fails when pivoting circumvents firewall controls.

**Critical System Compromise Impact:**
Pivoting provides paths to domain controllers, database servers, backup systems, and management infrastructure otherwise isolated by segmentation. Attackers chain multiple pivot hosts reaching highly restricted networks. DMZ servers provide pivot points into internal corporate networks. Critical asset compromise occurs through multi-hop pivoting chains.

**Covert Communication and Persistence Impact:**
Tunneling through compromised hosts establishes covert command and control channels evading network monitoring. DNS tunneling and ICMP tunneling bypass egress filtering and web proxies. Attackers maintain persistent access through encrypted tunnels appearing as legitimate traffic. Pivoting enables data exfiltration through indirect routes avoiding monitoring.

## MITRE ATT&CK Mapping

- **T1090** - Command and Control: Proxy
- **T1090.001** - Command and Control: Proxy: Internal Proxy
- **T1572** - Command and Control: Protocol Tunneling
- **T1021** - Lateral Movement: Remote Services
- **T1095** - Command and Control: Non-Application Layer Protocol

## Remediation Guidance

Organizations should implement the following pivoting prevention practices:

1. **Network Segmentation Hardening:**
   - Implement strict network segmentation with firewall enforcement
   - Deploy microsegmentation preventing workstation-to-workstation connectivity
   - Restrict lateral movement through network access controls
   - Implement zero trust architecture eliminating implicit trust
   - Deploy host-based firewalls blocking unauthorized connections
   - Regular testing of segmentation effectiveness

2. **Jump Host and Bastion Architecture:**
   - Deploy dedicated jump hosts as sole authorized pivot points
   - Implement privileged access workstations (PAWs) for administrative access
   - Restrict administrative access to flow through controlled bastions
   - Deploy session recording on all jump host activities
   - Implement just-in-time (JIT) access to jump hosts
   - Monitor all connections through jump hosts comprehensively

3. **Tunneling and Port Forwarding Detection:**
   - Deploy deep packet inspection (DPI) detecting tunneling protocols
   - Monitor for SSH port forwarding and SOCKS proxy usage
   - Detect DNS tunneling through query pattern analysis
   - Identify ICMP tunneling through anomalous ICMP traffic
   - Alert on unexpected listening ports or proxy services
   - Deploy network behavior analysis identifying pivoting patterns

4. **Egress Filtering and Protocol Restrictions:**
   - Implement strict egress filtering allowing only required protocols
   - Block outbound SSH from servers and workstations
   - Restrict outbound VPN protocols to authorized systems
   - Deploy web proxy for HTTP/HTTPS traffic inspection
   - Block covert channel protocols (DNS over HTTPS, ICMP data)
   - Whitelist allowed external destinations

5. **Host-Based Security Controls:**
   - Deploy endpoint detection and response (EDR) solutions
   - Implement application whitelisting preventing tunneling tools
   - Enable host-based firewalls with default-deny policies
   - Monitor process execution for tunneling and proxy applications
   - Detect unauthorized port listening and forwarding
   - Implement USB and device control preventing physical pivoting

6. **Monitoring and Detection:**
   - Deploy network detection and response (NDR) solutions
   - Implement behavioral analytics for lateral movement detection
   - Monitor for connection chaining and multi-hop patterns
   - Alert on systems establishing unusual network connections
   - Deploy honeypot systems detecting unauthorized pivoting
   - Integrate network monitoring with SIEM for correlation
   - Implement automated response to detected pivoting activities
