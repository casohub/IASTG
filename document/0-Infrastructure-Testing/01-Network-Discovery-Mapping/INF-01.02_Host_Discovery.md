# INF-01.02 — Host Discovery (Unauthenticated)

## Summary

This test identifies active hosts within in-scope network ranges using unauthenticated network scanning techniques to build a comprehensive inventory of discoverable systems.

## Risk

Incomplete host discovery results in unidentified systems that escape security testing, leaving vulnerabilities and misconfigurations undetected. Organizations face exposure from unknown or forgotten systems, shadow IT, rogue devices, and legacy infrastructure that lack security controls. Undetected hosts may serve as entry points for attackers or pivot positions within the network. Compliance violations occur when inventory discrepancies exist between documented and actual systems.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network ranges have been identified and validated (see INF-01.01)
- Network connectivity to in-scope ranges exists
- Testing approach and impact constraints are defined (see INF-00.04, INF-00.05)

## Test Objectives

- Identify all active and responsive hosts within in-scope network ranges
- Determine host operating system and basic characteristics without authentication
- Enumerate both IPv4 and IPv6 active hosts if dual-stack infrastructure exists
- Validate discovered hosts against expected inventory
- Identify unexpected or undocumented systems requiring investigation

## Test Methodology

1. Perform ICMP-based host discovery:
   - Execute ICMP echo request (ping) sweeps across all in-scope network ranges
   - Test ICMP timestamp requests as alternative discovery method
   - Send ICMP address mask requests where applicable
   - Document hosts that respond to ICMP
   - Note hosts that may be blocking ICMP (silent but potentially active)
2. Conduct TCP-based host discovery:
   - Perform TCP SYN scans to common ports (80, 443, 22, 3389, 445) across all ranges
   - Use TCP ACK scans to detect hosts behind stateful firewalls
   - Test TCP connection attempts to identify responding hosts
   - Document TCP-responsive hosts that did not respond to ICMP
3. Execute UDP-based host discovery:
   - Send UDP probes to common services (53, 161, 123, 137)
   - Analyze UDP responses and ICMP port unreachable messages
   - Identify hosts responding to UDP but not ICMP or TCP
4. Perform ARP-based discovery for local network segments:
   - Execute ARP ping scans on directly connected networks
   - Analyze ARP cache entries from accessible systems
   - Identify hosts that respond at layer 2 but not layer 3
5. Enumerate IPv6 hosts if dual-stack infrastructure exists:
   - Scan link-local IPv6 addresses (fe80::/10)
   - Probe known IPv6 prefix ranges
   - Test multicast addresses for IPv6 host discovery
   - Use ICMPv6 neighbor solicitation messages
6. Perform OS fingerprinting using passive and active techniques:
   - Analyze TCP/IP stack behaviors and responses
   - Examine TTL values in responses
   - Review TCP window sizes and options
   - Identify operating system families (Windows, Linux, BSD, network devices)
7. Cross-reference discovered hosts with documented asset inventory:
   - Identify hosts present in inventory but not discovered
   - Identify discovered hosts not present in inventory
   - Flag discrepancies for investigation
8. Document all discovered hosts with the following attributes:
   - IP address (IPv4 and IPv6 if applicable)
   - Hostname if resolvable through reverse DNS
   - MAC address if available
   - Operating system fingerprint or best guess
   - Discovery method(s) that identified the host
   - Response characteristics (ICMP, TCP, UDP)

## Expected Secure Configuration

Organizations maintaining discoverable infrastructure should implement:

- Complete and current asset inventory with all system IP addresses
- Host-based firewalls configured to respond to or block discovery traffic consistently
- Network-level access controls limiting discovery traffic to authorized sources
- Monitoring and alerting for network scanning activities
- Regular comparison of asset inventory against network discovery results
- Documentation of expected discovery patterns and normal scanning behavior
- Asset naming conventions and DNS records for all infrastructure
- Network segmentation limiting broad discovery capabilities
- Decommissioning procedures ensuring removed systems are powered off or disconnected
- Quarterly asset inventory validation through discovery scanning

## Evidence to Collect

- Complete list of discovered active hosts with IP addresses
- Discovery scan command outputs and raw results
- Host discovery tool logs showing methods used
- OS fingerprinting results for identified systems
- Comparison report between discovered hosts and asset inventory
- Documentation of discovery discrepancies
- Network response characteristics for each host
- Screenshots or outputs from scanning tools
- Ping sweep, TCP scan, and UDP scan result summaries
- IPv6 host discovery results if applicable

## Impact

**Security Visibility Impact:**
Undiscovered hosts represent blind spots in security posture with unknown vulnerabilities and configurations. Shadow IT, rogue devices, forgotten systems, and unauthorized network connections escape security controls. Unmanaged endpoints lack patching, monitoring, and protection. Organizations cannot secure assets they do not know exist.

**Attack Surface Impact:**
Each undiscovered host represents potential attack surface including vulnerable services, weak authentication, and misconfigured systems. Attackers exploit forgotten or unmanaged systems as initial access points or lateral movement positions. Compliance violations occur when inventory inaccuracies prevent proper security control application.

**Operational Risk Impact:**
Incomplete host inventories hinder incident response, forensics, and security operations. Unknown systems may be compromised without detection. Network troubleshooting and capacity planning suffer from inaccurate infrastructure understanding. Decommissioned systems remaining online consume resources and create security liabilities.

## MITRE ATT&CK Mapping

- **T1046** - Discovery: Network Service Scanning
- **T1018** - Discovery: Remote System Discovery

## Remediation Guidance

Organizations should implement the following host discovery and inventory management practices:

1. **Automated Asset Discovery:**
   - Deploy network scanning tools for continuous asset discovery
   - Schedule regular automated host discovery scans
   - Implement network access control (NAC) for endpoint visibility
   - Use DHCP logs and network device ARP tables for discovery
   - Deploy endpoint agents for inventory visibility

2. **Asset Inventory Management:**
   - Maintain Configuration Management Database (CMDB) with all assets
   - Implement automated asset inventory reconciliation processes
   - Tag assets with ownership, business function, and criticality
   - Track asset lifecycle from deployment to decommissioning
   - Integrate asset discovery with change management

3. **Discovery Response Controls:**
   - Configure host-based firewalls with consistent discovery response policies
   - Implement network-level rate limiting for scanning activities
   - Deploy monitoring for unexpected discovery or scanning traffic
   - Create baselines for normal network discovery patterns
   - Alert on discovery activity from unauthorized sources

4. **Inventory Validation Procedures:**
   - Conduct quarterly validation of asset inventory accuracy
   - Investigate and resolve discovery discrepancies
   - Require documentation for all newly discovered systems
   - Implement procedures for handling unknown or rogue devices
   - Audit inventory accuracy as part of security assessments

5. **Decommissioning and Hygiene:**
   - Establish formal system decommissioning procedures
   - Verify removed systems are powered off and disconnected
   - Clean up DNS records and inventory entries for decommissioned systems
   - Audit for abandoned or forgotten systems periodically
   - Implement automated detection of inactive systems for review
