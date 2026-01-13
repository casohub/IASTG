# INF-03.06 — Clear-Text Protocol Usage (FTP/Telnet/HTTP)

## Summary

This test identifies services using cleartext protocols that transmit data without encryption, exposing credentials, sensitive information, and communications to interception by network attackers.

## Risk

Cleartext protocols transmit authentication credentials and data without encryption enabling trivial network interception by attackers with network access. Telnet, FTP, and HTTP transmit passwords in cleartext visible to packet capture. Unencrypted protocols expose session content including commands, file transfers, and application data. Man-in-the-middle attacks against cleartext protocols enable credential theft and data modification without detection. Legacy applications requiring cleartext protocols create persistent security vulnerabilities incompatible with modern security standards. Organizations face credential compromise, data breaches, and compliance violations when cleartext protocols remain in use.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Network traffic monitoring or packet capture capabilities available
- Testing includes protocol security evaluation

## Test Objectives

- Identify all services using cleartext protocols
- Detect credentials and sensitive data transmitted without encryption
- Assess availability of encrypted protocol alternatives
- Validate enforcement of encrypted protocol usage
- Evaluate business justification for cleartext protocol requirements

## Test Methodology

1. Enumerate cleartext terminal access protocols:
   - **Telnet:** TCP port 23 for terminal access
   - **rlogin:** TCP port 513 for remote login
   - **rsh (Remote Shell):** TCP port 514 for command execution
   - Test for Telnet service availability and authentication
   - Capture Telnet traffic to verify cleartext credential transmission
2. Identify cleartext file transfer protocols:
   - **FTP (File Transfer Protocol):** TCP ports 20, 21
   - **TFTP (Trivial FTP):** UDP port 69
   - Test for anonymous FTP access
   - Verify FTP credential transmission in cleartext
   - Assess FTP passive mode and data transfer encryption
3. Enumerate cleartext web protocols:
   - **HTTP:** TCP port 80 without encryption
   - HTTP Basic Authentication (Base64-encoded, not encrypted)
   - HTTP Digest Authentication (still vulnerable to replay)
   - Web applications accessible via HTTP instead of HTTPS
   - HTTP forms transmitting credentials or sensitive data
4. Identify cleartext email protocols:
   - **SMTP:** TCP port 25 without STARTTLS
   - **POP3:** TCP port 110 without SSL/TLS
   - **IMAP:** TCP port 143 without SSL/TLS
   - Test for email authentication without encryption
   - Verify availability of encrypted alternatives (SMTPS, POP3S, IMAPS)
5. Enumerate additional cleartext protocols:
   - **LDAP:** TCP port 389 without TLS (LDAPS on 636)
   - **SNMP v1/v2c:** UDP ports 161-162 with community strings
   - **VNC:** TCP ports 5900+ without encryption
   - **RFB (Remote Framebuffer):** Cleartext remote desktop
   - **NFS:** UDP/TCP port 2049 without Kerberos encryption
6. Perform network traffic analysis:
   - Capture network traffic on cleartext protocol ports
   - Identify credentials transmitted in cleartext
   - Extract sensitive data from unencrypted protocols
   - Document session content visibility
   - Verify protocol usage patterns and frequency
7. Test for protocol downgrade vulnerabilities:
   - Verify HTTPS-to-HTTP downgrade prevention (HSTS)
   - Test for SSL/TLS stripping attacks
   - Assess STARTTLS enforcement on email protocols
   - Test for forced plaintext authentication fallback
8. Assess encrypted protocol availability:
   - Verify SSH availability as Telnet replacement
   - Test SFTP or FTPS as FTP alternatives
   - Confirm HTTPS availability for HTTP services
   - Verify LDAPS availability for LDAP services
   - Assess encrypted email protocol support

## Expected Secure Configuration

Organizations securing network protocols should implement:

- Complete elimination of Telnet in favor of SSH
- FTP replaced with SFTP, FTPS, or SCP
- HTTPS enforced for all web applications with HSTS
- HTTP Basic Authentication eliminated or used only over HTTPS
- SMTP with required STARTTLS for email submission
- POP3S and IMAPS instead of cleartext POP3/IMAP
- LDAPS instead of cleartext LDAP
- SNMPv3 with authentication replacing SNMPv1/v2c
- VNC with TLS encryption or VPN encapsulation
- Network policies blocking cleartext protocol usage
- Regular scanning for cleartext protocol presence

## Evidence to Collect

- Complete inventory of services using cleartext protocols
- Network traffic captures showing cleartext credential transmission
- List of web applications accessible via HTTP
- Cleartext email protocol identification and usage
- Protocol downgrade vulnerability testing results
- Availability assessment of encrypted protocol alternatives
- Business justification documentation for required cleartext protocols
- Network policy enforcement validation
- Screenshots or packet captures demonstrating cleartext data exposure
- Encryption migration timeline for remaining cleartext protocols

## Impact

**Credential Theft and Session Hijacking Impact:**
Cleartext protocols expose credentials to passive network monitoring. Telnet and FTP transmit usernames and passwords visible in packet captures. HTTP Basic authentication provides Base64-encoded (not encrypted) credentials. Attackers on shared networks, compromised switches, or VPN connections capture credentials for reuse. Session hijacking succeeds when session tokens are transmitted without encryption.

**Data Exposure and Interception Impact:**
Unencrypted protocols expose all transmitted data including commands, file contents, database queries, and application data. Business documents, customer information, financial data, and intellectual property transmitted via cleartext protocols are visible to network attackers. Man-in-the-middle attacks enable real-time data interception and modification.

**Compliance and Regulatory Violations Impact:**
Regulatory frameworks prohibit transmission of sensitive data without encryption. PCI DSS forbids cleartext transmission of cardholder data. HIPAA requires encryption of ePHI in transit. GDPR mandates appropriate technical measures protecting personal data. Organizations face penalties, audit failures, and breach notification requirements when cleartext protocols expose regulated data.

## MITRE ATT&CK Mapping

- **T1040** - Credential Access: Network Sniffing
- **T1557** - Credential Access: Man-in-the-Middle
- **T1071** - Command and Control: Application Layer Protocol (abuse of legacy protocols)
- **T1048** - Exfiltration: Exfiltration Over Alternative Protocol

## Remediation Guidance

Organizations should implement the following cleartext protocol elimination practices:

1. **Terminal Protocol Migration:**
   - Disable Telnet service on all systems
   - Migrate to SSH for terminal access (port 22/TCP)
   - Configure SSH with strong ciphers and key exchange algorithms
   - Implement SSH key-based authentication replacing passwords
   - Block Telnet port 23/TCP at network firewalls
   - Remove Telnet client software from endpoints

2. **File Transfer Protocol Replacement:**
   - Disable FTP and TFTP services
   - Deploy SFTP (SSH File Transfer Protocol) as primary file transfer method
   - Implement FTPS (FTP over SSL/TLS) for legacy application compatibility
   - Use SCP (Secure Copy Protocol) for command-line file transfers
   - Block FTP ports 20-21/TCP and TFTP port 69/UDP at firewalls
   - Deploy modern file sharing solutions (HTTPS-based, cloud storage)

3. **Web Protocol Encryption Enforcement:**
   - Redirect all HTTP traffic to HTTPS
   - Implement HTTP Strict Transport Security (HSTS)
   - Deploy TLS 1.2 minimum, prefer TLS 1.3
   - Obtain and deploy valid TLS certificates
   - Eliminate HTTP Basic Authentication; use form-based auth over HTTPS or modern methods
   - Configure web servers to refuse HTTP connections
   - Implement Content Security Policy (CSP) preventing mixed content

4. **Email Protocol Security:**
   - Require STARTTLS for SMTP submission (port 587)
   - Deploy SMTPS (port 465) for legacy client compatibility
   - Use POP3S (port 995) and IMAPS (port 993) exclusively
   - Disable cleartext POP3 port 110 and IMAP port 143
   - Implement SPF, DKIM, and DMARC for email authentication
   - Deploy MTA-STS for enforced TLS on email delivery

5. **Legacy Protocol Elimination:**
   - Replace cleartext LDAP with LDAPS (port 636)
   - Migrate from SNMPv1/v2c to SNMPv3 with authentication and encryption
   - Deploy VNC over TLS or restrict to VPN-only access
   - Implement Kerberos authentication for NFS (sec=krb5p)
   - Disable or firewall-block legacy cleartext protocols
   - Document and justify any remaining cleartext protocol requirements

6. **Network Enforcement and Monitoring:**
   - Deploy network access control (NAC) blocking cleartext protocols
   - Implement IDS/IPS rules detecting cleartext credential transmission
   - Monitor for cleartext protocol usage and alert on violations
   - Deploy data loss prevention (DLP) detecting unencrypted sensitive data
   - Regular network scanning identifying cleartext protocol presence
   - Enforce encryption requirements through security policy and compliance checks
