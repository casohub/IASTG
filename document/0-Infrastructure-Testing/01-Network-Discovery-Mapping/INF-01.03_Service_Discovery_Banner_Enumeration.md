# INF-01.03 — Service Discovery and Banner Enumeration

## Summary

This test identifies network services running on discovered hosts and enumerates service banners, versions, and protocols to understand the attack surface and identify potentially vulnerable software.

## Risk

Unidentified services expose unknown attack vectors that escape vulnerability scanning and security testing. Organizations face exploitation of undocumented services running outdated software, insecure protocols, or vulnerable versions. Banner information disclosure reveals specific software versions enabling targeted attacks against known vulnerabilities. Services running on non-standard ports evade detection by security controls expecting services on default ports. Excessive service exposure increases attack surface unnecessarily when services should be firewalled or disabled.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Active hosts have been discovered (see INF-01.02)
- Network connectivity to discovered hosts exists
- Impact constraints permit port scanning activities (see INF-00.05)

## Test Objectives

- Enumerate all accessible network services on discovered hosts
- Identify service types, protocols, and port numbers
- Extract service version information from banners
- Detect services running on non-standard ports
- Identify potentially vulnerable or outdated services

## Test Methodology

1. Perform comprehensive TCP port scanning on all discovered hosts:
   - Execute full TCP port scan (1-65535) where authorized and technically feasible
   - Use TCP SYN scanning for efficient port state determination
   - Conduct TCP connect scanning if SYN scanning is blocked or unavailable
   - Identify open, closed, and filtered port states
   - Document scan methodology and any constraints applied
2. Execute UDP port scanning for common UDP services:
   - Scan common UDP ports (53, 123, 161, 500, 1434, 1900)
   - Verify UDP service responses to distinguish open from filtered ports
   - Document UDP scanning challenges due to protocol limitations
3. Enumerate service banners and version information:
   - Connect to open ports and capture service banners
   - Send protocol-specific probes to elicit detailed responses
   - Extract software name, version, and patch level from banners
   - Document server type, operating system, and application details
   - Capture full banner text for analysis
4. Identify service types through protocol analysis:
   - Determine application protocols (HTTP, FTP, SSH, SMB, RDP, database protocols)
   - Identify encryption protocols (TLS/SSL versions)
   - Detect authentication mechanisms advertised by services
   - Recognize proprietary or custom protocols
5. Detect services on non-standard ports:
   - Perform service identification beyond default port assumptions
   - Use protocol-specific probes on all open ports
   - Identify administrative interfaces on alternate ports
   - Document services deliberately running on non-default ports
6. Analyze banner information for security indicators:
   - Identify version numbers indicating known vulnerable software
   - Detect outdated or unsupported software versions
   - Identify software reaching end-of-life (EOL)
   - Note verbose banner information disclosing excessive details
7. Document service exposure patterns:
   - Services exposed to Internet versus internal-only
   - Services running across multiple hosts (distributed applications)
   - Administrative services that should be restricted
   - Development or testing services in production networks
8. Cross-reference discovered services with expected service inventory:
   - Identify unexpected services not documented in asset inventory
   - Flag unauthorized or shadow IT services
   - Confirm expected services are present and accessible
   - Document service inventory discrepancies

## Expected Secure Configuration

Organizations managing network services should implement:

- Complete service inventory documenting all authorized services, ports, and protocols
- Default-deny firewall policies limiting service exposure to required endpoints
- Banner suppression or modification to limit version disclosure
- Regular vulnerability scanning against services with version detection
- Software update and patch management ensuring current versions
- Service hardening configurations removing unnecessary features
- Monitoring and alerting for new or unauthorized services
- Network segmentation restricting administrative service access
- Decommissioning procedures for unused or legacy services
- Quarterly service enumeration and inventory validation

## Evidence to Collect

- Port scan results showing all open ports on each host
- Service banner outputs with version information
- Protocol analysis results for service identification
- UDP service enumeration results
- List of services on non-standard ports
- Comparison between discovered services and expected inventory
- Screenshots of banner captures from key services
- Documentation of vulnerable or outdated service versions identified
- Service exposure map showing Internet-facing versus internal services
- Anomalous or unexpected service listings requiring investigation

## Impact

**Exploitation Risk Impact:**
Exposed services running vulnerable versions provide direct attack paths for exploitation. Known CVE-numbered vulnerabilities in identified service versions enable targeted attacks. Outdated software reaches end-of-support and lacks security patches. Remote code execution, authentication bypass, and privilege escalation vulnerabilities in services lead to system compromise.

**Information Disclosure Impact:**
Verbose service banners disclose software names, versions, operating systems, and internal hostnames assisting attacker reconnaissance. Version information enables attackers to precisely select exploits and payloads. Internal service exposure on Internet-facing hosts reveals internal architecture and technology stack.

**Attack Surface Impact:**
Unnecessary service exposure increases attack surface without business justification. Services on non-standard ports evade security monitoring expecting default port usage. Shadow IT services bypass change management and security review. Legacy or development services running in production lack proper security controls.

## MITRE ATT&CK Mapping

- **T1046** - Discovery: Network Service Scanning
- **T1590.002** - Reconnaissance: Gather Victim Network Information: DNS
- **T1595.002** - Reconnaissance: Active Scanning: Vulnerability Scanning

## Remediation Guidance

Organizations should implement the following service management and hardening practices:

1. **Service Inventory and Authorization:**
   - Maintain authoritative service inventory with port, protocol, and version details
   - Implement service authorization process requiring approval before deployment
   - Document business justification for each exposed service
   - Tag services with ownership, criticality, and exposure classification
   - Conduct quarterly service inventory validation scans

2. **Service Minimization:**
   - Disable unnecessary services on all systems
   - Remove unused server roles and features
   - Implement principle of least functionality
   - Restrict services to minimum required port exposure
   - Evaluate and eliminate legacy services

3. **Banner Suppression and Hardening:**
   - Configure services to suppress or modify version banners
   - Remove verbose information disclosure from error messages
   - Customize banners to remove OS and application details
   - Implement web server header modification
   - Harden service configurations following security baselines

4. **Vulnerability and Patch Management:**
   - Deploy vulnerability scanning against all services with version detection
   - Maintain current software versions with security patches applied
   - Track end-of-life dates for software and plan migrations
   - Implement automated patch deployment for critical services
   - Prioritize patching for Internet-facing services

5. **Network Segmentation and Access Control:**
   - Implement default-deny firewall policies
   - Restrict administrative services to management networks only
   - Use network segmentation to limit service exposure
   - Deploy jump hosts or bastion hosts for administrative access
   - Apply microsegmentation for east-west traffic control
   - Monitor for new or unauthorized service deployment
