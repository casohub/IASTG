# INF-02.03 — Legacy Protocol and Weak Authentication Identification

## Summary

This test identifies legacy authentication protocols and weak cryptographic implementations that expose credentials to interception, cracking, or exploitation through protocol vulnerabilities.

## Risk

Legacy authentication protocols transmit credentials without adequate encryption enabling network-based credential theft. Weak cryptographic algorithms in authentication protocols allow rapid credential recovery through cryptanalysis or brute-force attacks. Protocols vulnerable to downgrade attacks enable attackers to force use of weaker authentication methods. Protocol-specific vulnerabilities in legacy implementations provide authentication bypass or credential theft attack vectors. Organizations face widespread credential compromise when legacy protocols remain enabled despite availability of secure alternatives.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication mechanisms have been identified (see INF-02.01)
- Network and service discovery completed
- Protocol analysis capabilities available

## Test Objectives

- Identify all legacy authentication protocols in active use
- Detect weak cryptographic algorithms in authentication implementations
- Test for protocol downgrade attack vulnerabilities
- Assess protocol vulnerability to known authentication exploits
- Validate that secure protocol alternatives are available and enabled

## Test Methodology

1. Enumerate legacy Windows authentication protocols:
   - **NTLM v1:** Extremely weak authentication with multiple vulnerabilities
   - **LAN Manager (LM) hashes:** Trivially crackable password representation
   - **NTLM v2:** Legacy but stronger than NTLMv1; still vulnerable to relay attacks
   - Test for NTLM usage on services that should use Kerberos
   - Identify systems negotiating NTLM instead of Kerberos
2. Test for cleartext authentication protocols:
   - **HTTP Basic Authentication:** Base64-encoded (not encrypted) credentials
   - **Telnet:** Cleartext terminal protocol transmitting passwords
   - **FTP:** File Transfer Protocol with cleartext password authentication
   - **TFTP:** Trivial FTP without authentication
   - **SMTP without STARTTLS:** Email submission without encryption
   - **POP3/IMAP without SSL/TLS:** Email retrieval with cleartext passwords
3. Identify weak SSL/TLS implementations:
   - SSL 2.0 and SSL 3.0 (both deprecated and vulnerable)
   - TLS 1.0 (deprecated, vulnerable to BEAST, POODLE attacks)
   - TLS 1.1 (deprecated, lacks modern security features)
   - Weak cipher suites (RC4, DES, 3DES, export ciphers)
   - Null cipher suites allowing unencrypted connections
4. Test for weak SSH configurations:
   - SSH protocol version 1 (obsolete and vulnerable)
   - Weak SSH cipher algorithms (arcfour, blowfish-cbc)
   - Weak key exchange algorithms (diffie-hellman-group1-sha1)
   - CBC mode ciphers vulnerable to attacks
   - Weak HMAC algorithms (md5, sha1)
5. Enumerate weak SNMP configurations:
   - SNMP v1 and v2c with community strings (cleartext)
   - Default community strings ("public", "private")
   - SNMP write access with weak or default community strings
   - Lack of SNMPv3 implementation with authentication
6. Test for protocol downgrade vulnerabilities:
   - Kerberos to NTLM downgrade attacks
   - TLS version downgrade to SSL 3.0 or TLS 1.0
   - SSH protocol version downgrade to SSH-1
   - Forced cleartext authentication fallback
7. Assess SMB protocol security:
   - SMB version 1 usage (vulnerable to EternalBlue and other attacks)
   - SMB signing not required or not enforced
   - SMB encryption not implemented
   - SMB null session access permitted
8. Identify additional weak protocols:
   - LDAP simple bind without TLS (cleartext passwords)
   - VNC with DES encryption only
   - RDP with weak encryption (low, FIPS non-compliant)
   - Weak WPA/WPA2 implementations on wireless networks

## Expected Secure Configuration

Organizations securing authentication protocols should implement:

- Kerberos as primary authentication for Windows environments
- NTLMv1 and LM hashes disabled network-wide
- TLS 1.2 minimum, TLS 1.3 preferred for encrypted protocols
- SSH protocol version 2 only with strong ciphers and key exchange
- SNMPv3 with authentication replacing SNMPv1/v2c
- SMB version 3 with SMB signing and encryption enforced
- LDAPS (LDAP over TLS) instead of cleartext LDAP
- Elimination of all cleartext protocols (Telnet, FTP, HTTP auth)
- Protocol version enforcement preventing downgrades
- Regular protocol security reviews and deprecation planning

## Evidence to Collect

- Inventory of legacy protocols identified across infrastructure
- Network traffic captures showing legacy protocol authentication
- SSL/TLS configuration scan results with weak cipher identification
- SSH server configuration analysis with weak algorithm detection
- SNMP version enumeration and community string testing results
- SMB version detection and configuration analysis
- Protocol downgrade attack testing results
- List of services using legacy protocols with mitigation status
- Timeline for legacy protocol deprecation and removal
- Documentation of systems requiring legacy protocols with justification

## Impact

**Credential Interception Impact:**
Cleartext protocols transmit credentials without encryption enabling trivial network interception. HTTP Basic authentication, Telnet, FTP, and weak SNMP expose passwords to passive network monitoring. Man-in-the-middle attacks capture credentials during authentication exchanges. VPN, wireless, or compromised network positions provide credential theft opportunities.

**Rapid Credential Cracking Impact:**
LM hashes crack in seconds or minutes using standard tools. NTLM v1 authentication vulnerable to pass-the-hash attacks without password knowledge. Weak cryptographic algorithms in legacy protocols enable accelerated credential recovery. Captured authentication handshakes yield credentials through offline attacks.

**Protocol Exploitation Impact:**
Legacy protocol implementations contain known vulnerabilities enabling authentication bypass or remote code execution. SMBv1 vulnerabilities (EternalBlue) provide complete system compromise. SSL/TLS weaknesses (BEAST, POODLE, CRIME) downgrade encryption or leak session data. SSH-1 vulnerabilities enable MITM attacks and session hijacking.

## MITRE ATT&CK Mapping

- **T1040** - Credential Access: Network Sniffing
- **T1557** - Credential Access: Man-in-the-Middle
- **T1003** - Credential Access: OS Credential Dumping (LM/NTLM hash theft)
- **T1071** - Command and Control: Application Layer Protocol (abuse of legacy protocols)
- **T1212** - Exploitation for Credential Access

## Remediation Guidance

Organizations should implement the following legacy protocol remediation practices:

1. **Windows Authentication Modernization:**
   - Disable NTLMv1 authentication via Group Policy network-wide
   - Disable LM hash storage via Group Policy
   - Configure clients to refuse NTLMv1
   - Enable Kerberos as exclusive authentication method where possible
   - Implement Extended Protection for Authentication (EPA)
   - Deploy SMBv2/3 only configurations disabling SMBv1

2. **Protocol Encryption Enforcement:**
   - Replace Telnet with SSH for terminal access
   - Replace FTP with SFTP or FTPS
   - Enforce HTTPS for all web applications, disable HTTP
   - Require SMTPS, POP3S, IMAPS for email protocols
   - Implement LDAPS replacing cleartext LDAP binds
   - Deploy RDP with Network Level Authentication (NLA) and TLS

3. **TLS/SSL Hardening:**
   - Disable SSL 2.0, SSL 3.0, TLS 1.0, and TLS 1.1 completely
   - Configure TLS 1.2 as minimum, prefer TLS 1.3
   - Remove weak cipher suites (RC4, DES, 3DES, export ciphers)
   - Enforce forward secrecy cipher suites (ECDHE, DHE)
   - Implement certificate pinning where appropriate
   - Regular SSL/TLS configuration scanning and hardening

4. **SSH Hardening:**
   - Enforce SSH protocol version 2 only
   - Configure strong ciphers only (aes256-gcm, chacha20-poly1305)
   - Use strong key exchange algorithms (curve25519, ecdh-sha2-nistp256)
   - Implement strong HMAC algorithms (hmac-sha2-256, hmac-sha2-512)
   - Disable weak algorithms in sshd_config
   - Regular SSH configuration auditing

5. **SNMP Security Migration:**
   - Migrate from SNMPv1/v2c to SNMPv3 with authentication and encryption
   - Configure SNMPv3 with strong authentication (SHA-256) and privacy (AES-256)
   - Disable SNMP completely on systems not requiring management
   - Change default community strings if SNMPv1/v2c temporarily required
   - Restrict SNMP access to management networks only
   - Implement SNMP monitoring for unauthorized protocol versions
