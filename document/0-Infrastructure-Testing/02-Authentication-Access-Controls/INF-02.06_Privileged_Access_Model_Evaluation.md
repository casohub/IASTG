# INF-02.06 — Privileged Access Model (PAM/PAW) Evaluation

## Summary

This test evaluates organizational implementation of privileged access management (PAM) and privileged access workstation (PAW) controls to ensure administrative activities are isolated, monitored, and protected from credential theft.

## Risk

Lack of privileged access management enables credential theft through compromised administrative workstations used for both privileged and unprivileged activities. Administrators browsing web or checking email from systems with domain administrator credentials expose high-value credentials to phishing, drive-by downloads, and watering hole attacks. Pass-the-hash and credential dumping attacks succeed when administrative credentials reside in memory on vulnerable systems. Uncontrolled privileged access creates persistent compromise risk where single workstation infection yields domain-wide administrative credentials. Organizations face complete infrastructure compromise through administrative credential theft.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Access to identity and access management systems
- Understanding of administrative user accounts and permissions
- Testing includes privileged access evaluation

## Test Objectives

- Assess privileged access workstation (PAW) implementation
- Evaluate privileged access management (PAM) solution deployment
- Verify administrative credential isolation and protection
- Test for privileged credential exposure through mixed-use systems
- Validate monitoring and logging of privileged activities

## Test Methodology

1. Identify privileged users and administrators:
   - Domain Administrators, Enterprise Administrators
   - Local administrators on critical systems
   - Database administrators (DBAs)
   - Cloud administrators (Azure Global Admins, AWS Root, GCP Organization Admins)
   - Network and infrastructure administrators
2. Assess privileged access workstation (PAW) implementation:
   - Verify dedicated workstations exist for administrative tasks
   - Test whether PAWs are isolated from general user activities
   - Assess PAW configuration hardening and security baselines
   - Verify PAWs cannot access Internet or untrusted resources
   - Test for application whitelisting or similar restrictive controls on PAWs
3. Evaluate privileged access management (PAM) solutions:
   - Identify PAM platforms in use (CyberArk, BeyondTrust, Delinea, etc.)
   - Assess credential vaulting and password rotation capabilities
   - Test session recording and monitoring implementation
   - Verify just-in-time (JIT) access and privilege elevation
   - Assess privileged session isolation (jump servers, bastion hosts)
4. Test for credential exposure on mixed-use systems:
   - Identify administrators using personal workstations for privileged access
   - Test whether administrators browse Internet from privileged systems
   - Assess whether administrative credentials are cached on user workstations
   - Verify administrators do not use privileged accounts for email or productivity
5. Analyze privileged authentication methods:
   - Verify administrative access uses smartcards or certificate-based authentication
   - Test for multi-factor authentication (MFA) on all privileged access
   - Assess whether privileged accounts have interactive logon restrictions
   - Verify privileged accounts cannot authenticate to standard user systems
6. Evaluate administrative tiering and segmentation:
   - Verify implementation of administrative tier model (Tier 0, 1, 2)
   - Test for credential separation between administrative tiers
   - Assess restrictions preventing lower-tier systems from managing higher tiers
   - Verify Tier 0 administrators cannot logon to lower-tier systems
7. Test privileged session monitoring:
   - Verify all privileged sessions are logged and recorded
   - Assess real-time monitoring of administrative activities
   - Test for keystroke logging or screen recording of privileged sessions
   - Verify session recordings are retained and tamper-proof
8. Assess just-in-time and just-enough access:
   - Test for time-limited privileged access grants
   - Verify approval workflows for privileged access requests
   - Assess automatic privilege revocation after time expiration
   - Test for scope-limited privileges (just-enough access)

## Expected Secure Configuration

Organizations implementing privileged access management should maintain:

- Dedicated privileged access workstations (PAWs) for all administrative activities
- PAWs isolated from Internet, email, and untrusted resources
- Privileged Access Management (PAM) solution vaulting all administrative credentials
- Just-in-time (JIT) privilege elevation with time-limited access
- Multi-factor authentication (MFA) required for all privileged access
- Administrative tiering (Tier 0, 1, 2) with credential isolation between tiers
- Comprehensive logging and session recording of all privileged activities
- Prohibition of privileged account usage on standard user workstations
- Certificate or smartcard authentication for administrative accounts
- Regular privileged access reviews and recertification

## Evidence to Collect

- Privileged user and administrator account inventory
- PAW identification and configuration assessment
- PAM solution implementation documentation and configuration
- Administrative credential exposure testing on non-PAW systems
- Administrative tier model documentation and validation
- Privileged session logging and recording configuration
- MFA implementation status for privileged accounts
- Just-in-time access workflow documentation
- Test results showing privileged access from standard workstations
- Privileged session monitoring and audit log samples

## Impact

**Administrative Credential Theft Impact:**
Compromised workstations with cached administrative credentials provide domain-wide access. Pass-the-hash attacks extract NTLM hashes from memory of systems where administrators authenticated. Credential dumping tools (Mimikatz) retrieve cleartext passwords or Kerberos tickets from LSASS memory. Single compromised administrative workstation yields complete domain compromise.

**Lateral Movement via Privileged Accounts Impact:**
Stolen administrative credentials enable unrestricted lateral movement across infrastructure. Attackers use privileged accounts to access all systems, install backdoors, and exfiltrate data. Domain Administrator credentials provide ability to modify security policies, create accounts, and establish persistence. Administrative credential compromise enables ransomware deployment across entire organizations.

**Undetected Privileged Abuse Impact:**
Lack of privileged session monitoring prevents detection of credential misuse. Attackers using stolen administrative credentials blend with legitimate administrative activities. Insufficient logging hinders forensic investigation of privileged account abuse. Organizations cannot detect or respond to insider threats or compromised administrator accounts.

## MITRE ATT&CK Mapping

- **T1078.002** - Valid Accounts: Domain Accounts (privileged account abuse)
- **T1003** - OS Credential Dumping (administrative credential theft)
- **T1550.002** - Use Alternate Authentication Material: Pass the Hash
- **T1550.003** - Use Alternate Authentication Material: Pass the Ticket
- **T1134.001** - Access Token Manipulation: Token Impersonation/Theft

## Remediation Guidance

Organizations should implement the following privileged access management practices:

1. **Privileged Access Workstation (PAW) Deployment:**
   - Deploy dedicated PAWs for all Tier 0 (domain) and Tier 1 (server) administrators
   - Configure PAWs with hardened security baselines
   - Implement application whitelisting allowing only administrative tools
   - Block Internet access, email, and productivity applications on PAWs
   - Deploy device guard and credential guard on PAWs
   - Separate physical or virtual workstations for user and admin activities

2. **Privileged Access Management (PAM) Solution:**
   - Deploy enterprise PAM platform for credential vaulting
   - Implement automatic password rotation for privileged accounts
   - Enable session recording for all privileged access
   - Deploy just-in-time (JIT) access with approval workflows
   - Implement privilege elevation rather than standing administrative rights
   - Integrate PAM with SIEM for real-time monitoring

3. **Administrative Tiering Implementation:**
   - Implement Microsoft's Tiered Administrative Model or equivalent
   - Separate Tier 0 (domain controllers, identity) from Tier 1 (servers) and Tier 2 (workstations)
   - Use separate administrative accounts per tier
   - Prevent credential flow from higher to lower tiers
   - Restrict Tier 0 accounts from authenticating to any non-Tier 0 systems
   - Implement authentication policies enforcing tier isolation

4. **Privileged Authentication Hardening:**
   - Deploy smartcards or FIDO2 tokens for all administrative accounts
   - Require multi-factor authentication (MFA) for privileged access
   - Implement certificate-based authentication replacing passwords
   - Remove privileged account passwords from Active Directory where possible
   - Configure privileged accounts with "Account is sensitive and cannot be delegated"
   - Enable Protected Users security group for administrative accounts

5. **Privileged Activity Monitoring:**
   - Enable comprehensive logging of all privileged authentications
   - Implement real-time monitoring of administrative activities
   - Deploy User and Entity Behavior Analytics (UEBA) for privilege accounts
   - Alert on privileged account authentication from unusual locations or systems
   - Monitor for credential dumping and pass-the-hash attack indicators
   - Conduct regular privileged access reviews and recertification

6. **Policy and Training:**
   - Establish and enforce privileged access policies
   - Train administrators on PAW usage and security requirements
   - Prohibit privileged account usage for email, browsing, or productivity
   - Implement consequences for privileged access policy violations
   - Conduct regular privileged access policy reviews and updates
