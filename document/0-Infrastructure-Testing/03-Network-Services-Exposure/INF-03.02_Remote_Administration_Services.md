# INF-03.02 — Remote Administration Services Exposure (RDP/SSH/VPN)

## Summary

This test identifies remote administration services including Remote Desktop Protocol (RDP), Secure Shell (SSH), and Virtual Private Network (VPN) endpoints, assessing their exposure, authentication security, and vulnerability to attack.

## Risk

Exposed remote administration services provide direct access to systems and infrastructure from untrusted networks. Internet-facing RDP and SSH services suffer constant brute-force attacks attempting credential guessing. Weak authentication on remote access services enables unauthorized administrative access through password spraying or credential stuffing. Vulnerable remote access software provides remote code execution enabling system compromise. VPN misconfigurations expose internal networks to unauthorized external access. Organizations face complete infrastructure compromise when remote administration services lack multi-factor authentication, suffer from weak credentials, or contain exploitable vulnerabilities.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Remote access services have been identified
- Testing includes remote administration service evaluation

## Test Objectives

- Identify all remote administration services and their network exposure
- Assess authentication mechanisms protecting remote access
- Test for vulnerabilities in remote access protocols and implementations
- Evaluate multi-factor authentication deployment on remote services
- Validate network segmentation restricting remote administration access

## Test Methodology

1. Enumerate Remote Desktop Protocol (RDP) services:
   - Scan for TCP port 3389 indicating RDP service
   - Identify RDP gateway and broker services
   - Test RDP accessibility from Internet and untrusted networks
   - Verify Network Level Authentication (NLA) requirement
   - Identify RDP encryption strength and protocol version
2. Enumerate Secure Shell (SSH) services:
   - Scan for TCP port 22 and non-standard SSH ports
   - Identify SSH protocol version and server implementation
   - Test SSH key exchange algorithms and ciphers
   - Assess SSH authentication methods (password, public key, certificate)
   - Identify SSH services accessible from Internet
3. Identify VPN endpoints and remote access gateways:
   - Enumerate VPN protocols (IPsec, SSL VPN, L2TP, PPTP, WireGuard)
   - Identify VPN concentrators and gateway appliances
   - Test VPN endpoint accessibility and authentication
   - Assess VPN encryption and protocol versions
   - Identify split-tunneling configurations
4. Test remote administration authentication:
   - Verify multi-factor authentication (MFA) requirement on all remote access
   - Test for weak password policies on remote access accounts
   - Attempt authentication with common credentials
   - Test for username enumeration vulnerabilities
   - Assess account lockout policies on remote access services
5. Identify additional remote administration services:
   - VNC (Virtual Network Computing) on ports 5900-5909
   - TeamViewer and remote support tools
   - Windows Remote Management (WinRM) on ports 5985/5986
   - PowerShell Remoting
   - Web-based administration consoles
6. Test for known remote access vulnerabilities:
   - BlueKeep (CVE-2019-0708) and related RDP vulnerabilities
   - SSH vulnerabilities based on version identification
   - VPN software vulnerabilities (Pulse Secure, Citrix, Fortinet, Palo Alto)
   - Remote support tool vulnerabilities
   - Man-in-the-middle susceptibility
7. Assess remote access logging and monitoring:
   - Verify authentication attempt logging
   - Test for monitoring of failed login attempts
   - Assess alerting on successful remote access events
   - Verify geographic anomaly detection
   - Test for brute-force attack detection and response
8. Evaluate network segmentation for remote access:
   - Test whether VPN access lands in segmented network
   - Verify jump hosts or bastion hosts for administrative access
   - Assess firewall rules limiting remote administration traffic
   - Test for direct Internet-to-internal-network administrative access
   - Verify privileged access workstation (PAW) requirements

## Expected Secure Configuration

Organizations deploying remote administration services should implement:

- Multi-factor authentication (MFA) required for all remote access without exception
- Network Level Authentication (NLA) required for RDP with certificate validation
- SSH protocol version 2 only with strong ciphers and key exchange algorithms
- VPN with modern protocols (IKEv2, WireGuard) and strong encryption
- Firewall rules restricting remote administration to authorized source networks
- Bastion hosts or jump servers for administrative access
- Certificate-based authentication for remote administration
- Comprehensive logging and monitoring of remote access events
- Account lockout policies preventing brute-force attacks
- Regular vulnerability patching of remote access infrastructure

## Evidence to Collect

- Complete inventory of remote administration services with ports and protocols
- Network exposure assessment showing Internet vs internal accessibility
- Authentication testing results including MFA verification
- RDP Network Level Authentication (NLA) configuration status
- SSH configuration analysis with cipher and algorithm assessment
- VPN protocol and encryption strength evaluation
- Vulnerability scanning results for remote access services
- Remote access logging and monitoring configuration
- Brute-force attack testing and detection validation
- Network segmentation validation for remote access paths

## Impact

**Brute-Force and Credential Stuffing Impact:**
Internet-exposed remote administration services face continuous automated attack attempts. Weak passwords fall to brute-force attacks within days or weeks. Credential stuffing using breached passwords provides immediate access. Lack of account lockout enables unlimited authentication attempts. Successful authentication yields administrative access to systems and infrastructure.

**Remote Code Execution Vulnerability Impact:**
Unpatched remote access software contains critical vulnerabilities enabling remote code execution without authentication. BlueKeep RDP vulnerability allows wormable attacks. VPN vulnerabilities (Pulse Secure, Citrix) provide pre-authentication remote code execution. Exploitation yields complete system or network compromise from Internet.

**Lateral Movement and Privilege Escalation Impact:**
Compromised remote access credentials enable attackers to authenticate from anywhere. RDP access provides interactive desktop sessions on systems. SSH access enables command execution and privilege escalation. VPN access provides internal network connectivity from external locations. Administrative remote access credentials yield immediate infrastructure control.

## MITRE ATT&CK Mapping

- **T1021.001** - Lateral Movement: Remote Services: Remote Desktop Protocol
- **T1021.004** - Lateral Movement: Remote Services: SSH
- **T1133** - Initial Access: External Remote Services
- **T1110** - Credential Access: Brute Force
- **T1190** - Initial Access: Exploit Public-Facing Application

## Remediation Guidance

Organizations should implement the following remote administration security practices:

1. **Multi-Factor Authentication Enforcement:**
   - Require MFA for all remote access without exception
   - Deploy phishing-resistant MFA (FIDO2, certificates, smart cards)
   - Eliminate password-only remote authentication
   - Implement conditional access requiring MFA for remote connections
   - Remove MFA bypass mechanisms and legacy protocol exemptions
   - Monitor and alert on remote access without MFA

2. **RDP Security Hardening:**
   - Require Network Level Authentication (NLA) on all RDP services
   - Deploy RDP Gateway for Internet-facing RDP access
   - Implement certificate-based RDP authentication
   - Restrict RDP to privileged access workstations (PAWs)
   - Block direct Internet-to-RDP connections with firewall rules
   - Enable RDP encryption at "High" level minimum
   - Disable RDP on systems not requiring remote desktop access

3. **SSH Hardening:**
   - Enforce SSH protocol version 2 only
   - Configure strong ciphers (aes256-gcm, chacha20-poly1305)
   - Use strong key exchange algorithms (curve25519, ecdh-sha2-nistp256)
   - Implement SSH certificate-based authentication
   - Disable password authentication in favor of public key authentication
   - Use SSH bastion hosts for administrative access
   - Implement SSH port knocking or single packet authorization where appropriate

4. **VPN Security Enhancement:**
   - Deploy modern VPN protocols (IKEv2, WireGuard) with strong encryption
   - Require MFA for all VPN authentication
   - Implement certificate-based VPN authentication
   - Disable legacy VPN protocols (PPTP, L2TP without IPsec)
   - Configure VPN access to land in restricted network segments
   - Disable split tunneling requiring all traffic through VPN
   - Deploy per-application VPN or zero trust network access (ZTNA)

5. **Network Segmentation and Access Control:**
   - Deploy jump hosts or bastion hosts for administrative access
   - Implement network segmentation isolating administrative systems
   - Use firewall rules restricting remote administration to authorized sources
   - Block direct Internet access to administrative interfaces
   - Require VPN for remote administrative access
   - Implement privileged access workstation (PAW) requirements
   - Deploy just-in-time (JIT) access for remote administration

6. **Monitoring and Threat Detection:**
   - Enable comprehensive logging of remote access authentication
   - Deploy real-time monitoring and alerting on remote access events
   - Implement geographic anomaly detection for remote logins
   - Monitor for brute-force attacks and implement automated blocking
   - Deploy User and Entity Behavior Analytics (UEBA) for remote access
   - Alert on remote access from unusual locations, devices, or times
   - Integrate remote access logs with SIEM for correlation
