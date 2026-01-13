# INF-01.05 — Enumeration of Network Management Interfaces

## Summary

This test identifies network management interfaces including administrative consoles, device management protocols, and infrastructure control planes that provide privileged access to network devices and systems.

## Risk

Exposed network management interfaces provide high-value targets for attackers seeking infrastructure control. Compromise of management interfaces enables modification of routing, firewall rules, switch configurations, and security policies. Weak or default credentials on management interfaces lead to complete network compromise. Management protocols operating without encryption expose credentials and configuration data. Unrestricted management access from untrusted networks enables remote administrative takeover. Organizations face complete infrastructure compromise when management planes are inadequately secured.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Hosts and services have been discovered (see INF-01.02, INF-01.03)
- Network connectivity to infrastructure devices exists
- Authorization includes testing of management interfaces

## Test Objectives

- Identify network device management interfaces and protocols
- Enumerate web-based administrative consoles and control panels
- Detect infrastructure management protocols (SNMP, IPMI, iLO, iDRAC)
- Assess management interface authentication and access controls
- Evaluate encryption and security of management traffic

## Test Methodology

1. Identify network infrastructure devices from discovery data:
   - Routers, switches, and layer 3 devices
   - Firewalls and security appliances
   - Load balancers and application delivery controllers
   - Wireless access points and controllers
   - VPN concentrators and remote access gateways
2. Enumerate common management protocols on infrastructure devices:
   - **SNMP (Simple Network Management Protocol):** Ports 161/UDP, 162/UDP
   - **Telnet:** Port 23/TCP (unencrypted legacy management)
   - **SSH:** Port 22/TCP (encrypted management)
   - **HTTP/HTTPS:** Ports 80/TCP, 443/TCP (web-based management)
   - **NETCONF:** Port 830/TCP (network configuration protocol)
   - **Cisco proprietary:** Ports 8443/TCP, 4786/TCP
3. Identify server management interfaces:
   - **IPMI (Intelligent Platform Management Interface):** Port 623/UDP
   - **HP iLO (Integrated Lights-Out):** Ports 17988/TCP, 17990/TCP
   - **Dell iDRAC (Integrated Dell Remote Access Controller):** Ports 443/TCP, 5900/TCP
   - **Redfish API:** Port 443/TCP (modern management standard)
   - **Virtual machine management:** VMware vCenter, Hyper-V Manager, Proxmox
4. Enumerate SNMP community strings and versions:
   - Test for SNMP v1/v2c with common community strings (public, private)
   - Attempt SNMP read operations (GET requests)
   - Test SNMP write operations if read-only access obtained
   - Identify SNMP v3 implementations with authentication
   - Extract system information, interface details, and configuration via SNMP
5. Test web-based management console access:
   - Identify management web interfaces through banner analysis
   - Test for default credentials on management portals
   - Enumerate administrative login pages and authentication mechanisms
   - Test for information disclosure on login pages
   - Identify management interface software versions
6. Assess out-of-band management (OOBM) infrastructure:
   - Identify dedicated management network segments
   - Test separation between management and production networks
   - Evaluate management VLAN isolation
   - Assess jump host or bastion host access to management interfaces
7. Evaluate management interface security controls:
   - Test for encryption (TLS/SSL for web, SSH for CLI)
   - Identify unencrypted management protocols (Telnet, HTTP, SNMP v1/v2c)
   - Assess source IP or network restrictions on management access
   - Test for certificate validation and warning on HTTPS interfaces
   - Evaluate multi-factor authentication implementation
8. Document management interface exposure:
   - Interfaces accessible from Internet or untrusted networks
   - Management protocols traversing production networks
   - Separation between administrative and user access
   - Default or vendor-assigned credentials discovered

## Expected Secure Configuration

Organizations securing network management interfaces should implement:

- Dedicated out-of-band management (OOBM) network isolated from production
- Network access controls restricting management to authorized administrator sources
- Mandatory encryption for all management protocols (SSH, HTTPS, SNMP v3)
- Elimination of unencrypted management protocols (Telnet, HTTP, SNMP v1/v2c)
- Strong authentication with unique credentials per device or centralized authentication
- Multi-factor authentication for management access
- Certificate-based authentication where supported
- Management interface monitoring and session logging
- Firewall rules blocking management protocols from untrusted networks
- Regular review and rotation of management credentials

## Evidence to Collect

- Complete inventory of network management interfaces with IP addresses and ports
- SNMP enumeration results showing community strings and accessible information
- Web-based management console screenshots and version information
- Out-of-band management interface identification
- Protocol analysis showing encrypted versus unencrypted management traffic
- Access control testing results for management interfaces
- Default credential testing results
- Network diagrams showing management plane architecture
- Documentation of Internet or untrusted network exposure
- Management interface authentication mechanism analysis

## Impact

**Infrastructure Compromise Impact:**
Compromised management interfaces provide complete control over network infrastructure. Attackers modify routing to intercept traffic, alter firewall rules to bypass security controls, change switch configurations to enable traffic monitoring, and disable security appliances. Full network compromise occurs through administrative access to infrastructure.

**Credential Exposure Impact:**
Unencrypted management protocols expose administrative credentials to interception. Telnet and HTTP transmit credentials in cleartext. SNMP v1/v2c community strings provide read or read-write access to device configurations. Default or weak credentials enable trivial administrative access. Credential reuse across devices enables widespread compromise from single credential theft.

**Operational Disruption Impact:**
Unauthorized management access enables denial of service through configuration changes, routing disruptions, or device reboots. Attackers delete configurations, disable interfaces, or modify critical settings causing network outages. Business operations cease during infrastructure failures. Recovery requires physical access to devices with lost configurations.

## MITRE ATT&CK Mapping

- **T1040** - Credential Access: Network Sniffing (unencrypted management protocols)
- **T1557** - Credential Access: Man-in-the-Middle (management traffic interception)
- **T1021.004** - Lateral Movement: Remote Services: SSH
- **T1219** - Command and Control: Remote Access Software (management tools)
- **T1602** - Collection: Data from Configuration Repository

## Remediation Guidance

Organizations should implement the following network management security practices:

1. **Out-of-Band Management Architecture:**
   - Deploy dedicated physical or logical management network
   - Implement VLAN or VRF isolation for management traffic
   - Separate management interfaces from production data interfaces
   - Use dedicated management port on devices supporting dual interfaces
   - Implement jump hosts or bastion hosts for management access

2. **Management Protocol Hardening:**
   - Disable Telnet and enable SSH for command-line management
   - Disable HTTP and enable HTTPS for web-based management
   - Upgrade SNMP from v1/v2c to v3 with authentication and encryption
   - Configure strong SNMP v3 credentials with unique usernames per device
   - Disable SNMP write access unless specifically required

3. **Access Control Implementation:**
   - Implement firewall rules restricting management protocols to authorized sources
   - Configure device-level access control lists (ACLs) limiting management access
   - Deploy network access control (NAC) for management network connection
   - Restrict Internet access to management interfaces
   - Implement time-based access controls for management windows

4. **Authentication Strengthening:**
   - Eliminate default vendor credentials on all devices
   - Implement unique credentials per device or credential rotation
   - Deploy centralized authentication (RADIUS, TACACS+, LDAP)
   - Enable multi-factor authentication for management access
   - Implement certificate-based authentication where supported

5. **Monitoring and Audit:**
   - Enable comprehensive logging for management authentication and sessions
   - Implement real-time alerting for management access events
   - Deploy session recording for administrative activities
   - Monitor for failed authentication attempts and suspicious access patterns
   - Conduct regular audit of management access logs
   - Review and recertify management access permissions quarterly
