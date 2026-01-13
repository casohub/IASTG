# INF-04.04 — Tunnel and Proxy Creation for Network Pivoting

## Summary

This test assesses the creation and utilization of network tunnels and proxy services on compromised systems to enable communication with otherwise unreachable network segments and systems.

## Risk

Network tunnels and proxies through compromised hosts enable attackers to circumvent network segmentation, firewall rules, and access controls. Encrypted tunnels conceal malicious traffic within legitimate protocols making detection difficult. SOCKS proxies established on compromised systems provide authenticated access to restricted networks. HTTP tunneling through web servers bypasses egress filtering and web proxies. DNS tunneling and covert channels enable command and control communication evading traditional network monitoring. Organizations face expanded attack surface when tunneling capabilities enable access to protected network segments and exfiltration channels bypass security controls.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- At least one system has been compromised or access provided
- Multiple network segments exist requiring traversal
- Testing includes tunneling and proxy assessment

## Test Objectives

- Test tunnel creation capabilities on compromised systems
- Assess proxy deployment for network access
- Evaluate covert channel establishment for communication
- Test detection capabilities for tunneling activities
- Validate effectiveness of egress filtering against tunneling

## Test Methodology

1. Test SSH tunneling capabilities:
   - **Local Port Forwarding:** Forward local port to remote service through SSH (`ssh -L`)
   - **Remote Port Forwarding:** Forward remote port back to local system (`ssh -R`)
   - **Dynamic Port Forwarding:** Create SOCKS proxy through SSH (`ssh -D`)
   - Test SSH tunnel stability and throughput
   - Verify SSH tunneling through restrictive firewalls
2. Implement HTTP/HTTPS tunneling:
   - Deploy HTTP CONNECT tunneling through proxies
   - Test HTTPS tunneling concealing traffic
   - Implement WebSocket tunneling for bidirectional communication
   - Deploy HTTP tunneling tools (Chisel, reGeorg, Neo-reGeorg)
   - Test tunneling through corporate web proxies
3. Establish SOCKS proxy services:
   - Deploy SOCKS4/SOCKS5 proxy on compromised host
   - Configure proxy authentication if required
   - Test tool compatibility with SOCKS proxy (ProxyChains, Proxifier)
   - Verify SOCKS proxy performance and reliability
   - Test SOCKS over SSH for encrypted proxy
4. Test DNS tunneling capabilities:
   - Implement DNS tunneling for command and control
   - Deploy DNS tunneling tools (Iodine, DNSCat2, dnscat)
   - Test data exfiltration through DNS queries
   - Verify DNS tunneling throughput and latency
   - Assess detection evasion through DNS tunneling
5. Evaluate ICMP tunneling:
   - Deploy ICMP tunneling for covert communication
   - Test data encapsulation in ICMP echo requests/replies
   - Implement ICMP tunneling tools (Ptunnel, icmpsh)
   - Assess ICMP tunnel bandwidth and stability
   - Verify ICMP tunneling through firewalls
6. Test VPN tunneling through compromised hosts:
   - Establish VPN connection from compromised system
   - Configure split tunneling accessing both networks
   - Test VPN protocol availability (OpenVPN, WireGuard, IPsec)
   - Deploy VPN server on compromised system
   - Assess VPN tunnel encryption and authentication
7. Assess tunnel and proxy detection:
   - Verify network monitoring detects tunneling traffic
   - Test for deep packet inspection (DPI) identifying tunnels
   - Assess behavioral analysis detecting proxy usage
   - Verify logging captures tunneling establishment
   - Test for automated blocking of tunneling protocols
8. Test covert channel communication:
   - Implement data exfiltration through steganography
   - Test protocol manipulation for covert channels
   - Deploy traffic analysis evasion techniques
   - Verify timing-based covert channels
   - Assess detection capabilities for covert communication

## Expected Secure Configuration

Organizations preventing unauthorized tunneling should implement:

- Egress filtering allowing only required outbound protocols
- Deep packet inspection (DPI) detecting tunneling protocols
- DNS query monitoring identifying tunneling patterns
- ICMP traffic inspection and anomaly detection
- SSH server hardening preventing unauthorized tunneling
- Web proxy enforcement blocking HTTP CONNECT tunneling
- Network behavior analysis detecting unusual connection patterns
- Application control preventing tunneling tool execution
- Comprehensive logging of all outbound connections
- Incident response procedures for detected tunneling

## Evidence to Collect

- Tunnel creation testing results showing successful establishment
- Proxy deployment documentation and functionality
- Covert channel testing results and throughput measurements
- Detection capability validation for tunneling activities
- Network traffic captures showing tunnel characteristics
- Egress filtering effectiveness assessment
- DNS tunneling detection testing results
- Tool compatibility testing with created tunnels and proxies
- Performance metrics for established tunnels
- Business impact analysis of tunneling-enabled access

## Impact

**Network Segmentation Bypass Impact:**
Tunnels circumvent network segmentation and firewall rules enabling access to restricted network segments. Attackers establish tunnels through compromised DMZ systems reaching internal corporate networks. SSH tunneling through bastion hosts bypasses network access controls. Organizations lose segmentation effectiveness when tunneling provides unrestricted network traversal.

**Command and Control Concealment Impact:**
Encrypted tunnels conceal malicious traffic within legitimate protocols evading detection. DNS tunneling and ICMP tunneling appear as normal network operations. HTTPS tunneling blends with encrypted web traffic. Covert channels enable persistent command and control bypassing network monitoring and proxy filters.

**Data Exfiltration Through Covert Channels Impact:**
Tunneling enables data exfiltration through protocols allowed by egress filtering. DNS tunneling exfiltrates data through DNS queries bypassing DLP. HTTP tunneling transfers data through corporate proxies. Encrypted tunnels prevent content inspection of exfiltrated data. Organizations face undetected data theft through covert exfiltration channels.

## MITRE ATT&CK Mapping

- **T1572** - Command and Control: Protocol Tunneling
- **T1090.003** - Command and Control: Proxy: Multi-hop Proxy
- **T1071.004** - Command and Control: Application Layer Protocol: DNS
- **T1095** - Command and Control: Non-Application Layer Protocol
- **T1048.003** - Exfiltration: Exfiltration Over Alternative Protocol: Exfiltration Over Unencrypted/Obfuscated Non-C2 Protocol

## Remediation Guidance

Organizations should implement the following tunnel and proxy prevention practices:

1. **Egress Filtering Implementation:**
   - Deploy strict egress filtering allowing only required outbound protocols
   - Whitelist permitted external destinations by IP or domain
   - Block outbound SSH from servers and workstations
   - Restrict VPN protocols to authorized systems only
   - Deploy web proxy requiring authentication for HTTP/HTTPS
   - Block direct Internet access forcing traffic through proxies

2. **Deep Packet Inspection (DPI):**
   - Deploy DPI appliances inspecting encrypted traffic
   - Implement SSL/TLS inspection at network boundaries
   - Detect SSH tunneling through protocol analysis
   - Identify VPN protocols and unauthorized VPN usage
   - Analyze traffic patterns identifying tunneling behaviors
   - Deploy next-generation firewalls with application awareness

3. **DNS Security and Monitoring:**
   - Deploy DNS security solutions (Cisco Umbrella, Infoblox)
   - Monitor DNS queries for tunneling patterns
   - Detect excessive DNS query volumes from single hosts
   - Identify DNS queries with high entropy or unusual patterns
   - Block DNS over HTTPS (DoH) preventing DNS tunneling bypass
   - Implement DNS response policy zones (RPZ) blocking malicious domains

4. **Protocol-Specific Controls:**
   - Limit ICMP traffic rate and size preventing tunneling
   - Disable ICMP where not required for operations
   - Block unusual protocol usage (GRE, SCTP, custom protocols)
   - Deploy protocol whitelisting on network devices
   - Monitor for protocol anomalies and unusual encapsulation
   - Implement application-layer gateways inspecting protocols

5. **SSH Hardening and Restrictions:**
   - Disable SSH port forwarding on servers (`AllowTcpForwarding no`)
   - Restrict SSH tunneling (`PermitTunnel no`)
   - Implement SSH certificate-based authentication
   - Monitor SSH traffic for tunneling characteristics
   - Block outbound SSH from non-administrative systems
   - Deploy SSH bastion hosts as sole authorized SSH endpoints

6. **Network Behavior Analysis:**
   - Deploy network detection and response (NDR) solutions
   - Implement behavioral analytics identifying abnormal traffic
   - Monitor for unusual outbound connections
   - Detect traffic pattern anomalies indicating tunneling
   - Alert on long-duration connections characteristic of tunnels
   - Integrate network monitoring with SIEM for correlation
   - Deploy machine learning models detecting covert channels
