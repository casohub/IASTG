# INF-02.01 — Identification of Authentication Mechanisms in Use

## Summary

This test identifies all authentication mechanisms, protocols, and methods deployed across infrastructure systems to understand authentication architecture and identify potential weaknesses in authentication implementation.

## Risk

Unidentified authentication mechanisms create blind spots in security architecture where weak or legacy authentication protocols operate without oversight. Organizations face credential theft, authentication bypass, and unauthorized access when authentication methods lack modern security controls. Mixed authentication environments with inconsistent security policies enable attackers to target weakest authentication methods. Lack of centralized authentication visibility prevents detection of authentication-based attacks and credential compromise. Organizations cannot enforce consistent authentication standards when authentication mechanisms remain undocumented.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.01 through INF-01.03)
- Access to systems for authentication mechanism enumeration
- Testing approach determined (see INF-00.04)

## Test Objectives

- Identify all authentication mechanisms and protocols in use
- Enumerate both network and local authentication methods
- Categorize authentication by strength and modern security standards
- Map authentication architecture and centralized versus local authentication
- Identify legacy or deprecated authentication methods requiring remediation

## Test Methodology

1. Enumerate network authentication protocols:
   - **Kerberos:** Active Directory and domain authentication (ports 88/TCP, 464/TCP)
   - **LDAP/LDAPS:** Lightweight Directory Access Protocol (ports 389/TCP, 636/TCP)
   - **RADIUS:** Remote Authentication Dial-In User Service (ports 1812/UDP, 1813/UDP)
   - **TACACS+:** Terminal Access Controller Access-Control System (port 49/TCP)
   - **SAML:** Security Assertion Markup Language (web-based SSO)
   - **OAuth 2.0 / OpenID Connect:** Modern authorization and authentication frameworks
   - **NTLM/NTLMv2:** Legacy Windows authentication
   - **NTLM v1:** Extremely weak legacy authentication
2. Identify service-specific authentication methods:
   - **SSH:** Public key, password, certificate-based authentication
   - **RDP:** Network Level Authentication (NLA), password, smart card
   - **Web Applications:** Form-based, HTTP Basic/Digest, certificate-based, token-based
   - **Databases:** SQL authentication, Windows authentication, certificate authentication
   - **Email:** SMTP AUTH, POP3/IMAP authentication methods
   - **VPN:** Pre-shared keys, certificate-based, username/password, token-based
3. Enumerate local authentication mechanisms:
   - Local user accounts on Windows, Linux, network devices
   - Service accounts with local authentication
   - Application-specific local databases or user stores
   - Hardcoded credentials in applications or scripts
4. Identify multi-factor authentication (MFA) implementation:
   - MFA coverage across services and user populations
   - MFA methods in use (SMS, authenticator apps, hardware tokens, biometrics)
   - Services lacking MFA protection
   - MFA bypass mechanisms or legacy fallback authentication
5. Assess single sign-on (SSO) implementation:
   - Centralized SSO solutions (Active Directory, SAML Identity Providers)
   - Services integrated with SSO versus standalone authentication
   - SSO federation and trust relationships
   - Legacy applications not integrated with SSO
6. Test for weak or legacy authentication protocols:
   - Anonymous authentication (guest access, null sessions)
   - Cleartext protocols (Telnet, FTP, HTTP Basic authentication)
   - NTLMv1 or LM hash usage
   - Weak cryptographic algorithms in authentication protocols
7. Document authentication architecture:
   - Centralized authentication servers and directories
   - Authentication protocol flow and trust relationships
   - Primary authentication domain or realm
   - Federated authentication relationships
   - Segregation of authentication by security zones
8. Cross-reference authentication mechanisms with service inventory:
   - Map each service to its authentication method
   - Identify inconsistent authentication policies across similar services
   - Document services using deprecated authentication
   - Identify critical services lacking strong authentication

## Expected Secure Configuration

Organizations implementing authentication architecture should maintain:

- Centralized authentication using modern protocols (Kerberos, LDAP, SAML, OAuth)
- Comprehensive documentation of all authentication mechanisms in use
- Elimination of legacy authentication protocols (NTLMv1, LM hashes, cleartext protocols)
- Multi-factor authentication deployed for privileged and remote access
- Single sign-on integration for all compatible applications
- Consistent authentication policies across all services
- Regular authentication mechanism inventory and security assessment
- Monitoring and logging of authentication events across all mechanisms
- Automated detection of weak or deprecated authentication usage
- Documented authentication architecture with protocol flows and trust relationships

## Evidence to Collect

- Complete inventory of authentication protocols identified across infrastructure
- Service-to-authentication method mapping matrix
- MFA implementation coverage assessment
- SSO integration status for applications
- Identification of weak or legacy authentication protocols in use
- Authentication architecture diagram showing protocols and trust relationships
- Local authentication versus centralized authentication analysis
- Network traffic captures showing authentication protocol exchanges
- Authentication server identification (domain controllers, RADIUS servers, IdPs)
- Documentation of services lacking modern authentication controls

## Impact

**Credential Theft and Exploitation Impact:**
Weak authentication protocols transmit credentials in cleartext or weakly encrypted formats enabling interception. Legacy protocols (NTLMv1, LM hashes) vulnerable to rapid cracking provide easy credential recovery. Pass-the-hash attacks succeed against NTLM authentication. Lack of MFA enables single-factor compromise through phishing or password attacks. Stolen credentials provide persistent access without detection.

**Authentication Bypass Impact:**
Anonymous or guest authentication enables unauthenticated access to resources. Default credentials on services using local authentication provide trivial access. Application-specific authentication stores with weak password policies enable brute-force attacks. Authentication protocol vulnerabilities enable bypass of access controls.

**Inconsistent Security Posture Impact:**
Mixed authentication mechanisms prevent consistent policy enforcement. Critical services using weak authentication coexist with modern authentication systems. Users maintain multiple sets of credentials reducing password quality. Lack of SSO integration creates authentication silos preventing unified monitoring and access control.

## MITRE ATT&CK Mapping

- **T1078** - Valid Accounts (use of legitimate authentication)
- **T1110** - Credential Access: Brute Force
- **T1557** - Credential Access: Man-in-the-Middle (credential interception)
- **T1187** - Credential Access: Forced Authentication
- **T1550** - Credential Access: Use Alternate Authentication Material

## Remediation Guidance

Organizations should implement the following authentication architecture practices:

1. **Centralized Authentication Migration:**
   - Migrate all services to centralized authentication (Active Directory, LDAP, SSO)
   - Integrate applications with organizational identity provider
   - Eliminate local authentication on systems where possible
   - Federate authentication across organizational boundaries using SAML or OAuth
   - Document services requiring local authentication with justification

2. **Legacy Protocol Elimination:**
   - Disable NTLMv1 authentication network-wide
   - Upgrade to NTLMv2 minimum, preferably Kerberos
   - Eliminate cleartext authentication protocols (HTTP Basic, FTP, Telnet)
   - Replace anonymous/guest access with authenticated accounts
   - Disable legacy cryptographic algorithms in authentication protocols

3. **Multi-Factor Authentication Deployment:**
   - Implement MFA for all administrative and privileged access
   - Deploy MFA for all remote access (VPN, RDP, SSH)
   - Extend MFA to high-value applications and data access
   - Use phishing-resistant MFA methods (FIDO2, certificate-based)
   - Eliminate SMS-based MFA in favor of stronger methods

4. **Single Sign-On Implementation:**
   - Deploy SSO solution with SAML or OAuth/OpenID Connect
   - Integrate all compatible applications with SSO
   - Establish federated authentication for cloud services
   - Implement identity governance and access review workflows
   - Maintain inventory of SSO-integrated versus standalone authentication

5. **Authentication Monitoring and Governance:**
   - Centralize authentication logging from all mechanisms
   - Implement real-time alerting for failed authentication attempts
   - Monitor for legacy protocol usage and generate alerts
   - Deploy User and Entity Behavior Analytics (UEBA) for authentication anomalies
   - Conduct quarterly authentication architecture security reviews
   - Maintain authentication mechanism inventory with security classifications
