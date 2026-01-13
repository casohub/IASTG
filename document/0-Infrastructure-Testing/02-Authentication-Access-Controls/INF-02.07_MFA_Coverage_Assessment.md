# INF-02.07 — Multi-Factor Authentication Coverage Assessment

## Summary

This test assesses the deployment coverage and effectiveness of multi-factor authentication (MFA) across infrastructure, applications, and remote access to validate protection against credential-based attacks.

## Risk

Absence of multi-factor authentication enables account compromise through stolen passwords, phishing, or credential stuffing attacks. Single-factor authentication relies solely on password secrecy which fails when credentials are obtained through data breaches, social engineering, or keyloggers. Partial MFA deployment creates authentication gaps where attackers target unprotected systems to gain initial access. Weak MFA implementations (SMS-based) vulnerable to interception or bypass provide insufficient protection. Organizations face account takeover, unauthorized data access, and infrastructure compromise when MFA coverage gaps exist.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication mechanisms have been identified (see INF-02.01)
- Inventory of systems, applications, and access methods available
- Testing includes MFA evaluation

## Test Objectives

- Assess MFA deployment coverage across systems and applications
- Identify critical systems lacking MFA protection
- Evaluate MFA method strength and security
- Test for MFA bypass vulnerabilities
- Validate MFA enforcement and fallback authentication

## Test Methodology

1. Enumerate systems and applications with MFA implementation:
   - **Remote Access:** VPN, RDP gateway, SSH bastion hosts
   - **Web Applications:** Intranet portals, cloud applications, SaaS platforms
   - **Email:** Outlook Web Access (OWA), webmail interfaces
   - **Cloud Platforms:** Azure, AWS, GCP administrative consoles
   - **Administrative Interfaces:** Domain controllers, network device management, PAM solutions
2. Identify MFA methods in use and assess security strength:
   - **Strong Methods:** FIDO2/WebAuthn hardware tokens, certificate-based authentication, smart cards
   - **Moderate Methods:** Time-based one-time passwords (TOTP) via authenticator apps
   - **Weak Methods:** SMS-based one-time passwords, email-based codes, push notifications
   - **Deprecated Methods:** Phone call verification, security questions
3. Assess MFA coverage for user populations:
   - Administrative and privileged user accounts
   - Remote workers and VPN users
   - Standard employee accounts
   - External contractors and partners
   - Service accounts and non-human identities
4. Test MFA coverage for access methods:
   - Interactive web browser authentication
   - Mobile application authentication
   - Legacy application authentication (Outlook, thick clients)
   - API and programmatic access
   - Command-line tools and automation scripts
5. Identify critical systems lacking MFA:
   - Domain controllers and identity infrastructure
   - Database management systems
   - File servers and storage systems
   - Network infrastructure management
   - Backup and recovery systems
   - Cloud administration consoles
6. Test for MFA bypass techniques:
   - Legacy authentication protocol fallback (IMAP, POP3, SMTP Auth)
   - Application passwords or app-specific passwords
   - OAuth token generation without MFA verification
   - MFA reset or recovery processes bypassing verification
   - Conditional access policy exceptions
7. Evaluate MFA enrollment and provisioning:
   - Self-service enrollment processes
   - Administrator-assisted enrollment
   - MFA token distribution and lifecycle management
   - User experience and adoption rates
   - Enrollment verification and validation
8. Assess MFA reliability and backup authentication:
   - Fallback authentication when MFA unavailable
   - Support for multiple MFA methods per user
   - Token loss or replacement procedures
   - Emergency access procedures and break-glass accounts
   - MFA service availability and redundancy

## Expected Secure Configuration

Organizations implementing multi-factor authentication should maintain:

- Universal MFA deployment for all remote access (VPN, RDP, SSH)
- MFA required for all administrative and privileged accounts
- Phishing-resistant MFA methods preferred (FIDO2, certificates, smart cards)
- MFA coverage for cloud service access and administrative consoles
- Elimination of SMS-based MFA in favor of authenticator apps minimum
- No MFA bypass through legacy protocols or application passwords
- Conditional access policies enforcing MFA based on risk
- MFA enrollment monitoring ensuring coverage for all users
- Backup authentication methods for MFA failure scenarios
- Regular MFA coverage audits and gap remediation

## Evidence to Collect

- MFA implementation inventory by system and application
- MFA coverage matrix showing protection by user population and access method
- MFA method strength assessment (FIDO2 vs TOTP vs SMS)
- Identification of critical systems lacking MFA
- MFA bypass testing results (legacy protocols, app passwords)
- Conditional access policy documentation and configuration
- MFA enrollment status and coverage percentage by user group
- Fallback authentication procedures and emergency access documentation
- User experience feedback on MFA implementation
- MFA-related incident and support ticket analysis

## Impact

**Account Takeover Through Credential Compromise Impact:**
Absence of MFA enables account takeover once passwords are stolen through phishing, data breaches, or credential stuffing. Single-factor authentication provides no protection against compromised passwords. Attackers gain full access to systems and data using only stolen credentials. Widespread credential breaches expose organizations without MFA to mass account compromise.

**Privileged Access Compromise Impact:**
Administrative accounts without MFA become high-value targets for attackers. Single stolen administrator password yields domain-wide or infrastructure-wide access. Phishing campaigns targeting administrators succeed without MFA protection. Pass-the-hash attacks against administrative accounts lacking MFA provide persistent privileged access.

**Weak MFA Bypass Impact:**
SMS-based MFA vulnerable to SIM swapping attacks and interception. Legacy protocol exemptions allow MFA bypass through IMAP, POP3, or SMTP authentication. Application-specific passwords circumvent MFA providing single-factor authentication backdoors. Attackers exploit MFA bypass routes to gain access despite MFA deployment.

## MITRE ATT&CK Mapping

- **T1078** - Valid Accounts (using compromised credentials without MFA)
- **T1110** - Credential Access: Brute Force (attacks successful without MFA)
- **T1556** - Modify Authentication Process (MFA bypass techniques)
- **T1621** - Multi-Factor Authentication Request Generation (MFA fatigue attacks)

## Remediation Guidance

Organizations should implement the following multi-factor authentication practices:

1. **Comprehensive MFA Deployment:**
   - Implement MFA for all remote access without exception (VPN, RDP, SSH)
   - Require MFA for all administrative and privileged account authentication
   - Deploy MFA for all cloud service and SaaS application access
   - Extend MFA to internal applications and intranet access
   - Implement MFA for email access (OWA, ActiveSync, mail clients)
   - Deploy MFA for workstation and endpoint authentication

2. **Phishing-Resistant MFA Implementation:**
   - Deploy FIDO2/WebAuthn hardware security keys for administrative accounts
   - Implement smart card or certificate-based authentication for privileged access
   - Migrate from SMS-based MFA to authenticator apps minimum
   - Prioritize passwordless authentication methods (Windows Hello for Business, Apple Touch ID)
   - Eliminate security questions and phone-based verification
   - Document timeline for SMS MFA phase-out

3. **MFA Bypass Elimination:**
   - Disable legacy authentication protocols (IMAP, POP3, SMTP Auth, Basic Auth)
   - Eliminate application-specific passwords and app passwords
   - Require MFA for OAuth token generation
   - Implement conditional access policies without MFA exemptions
   - Block authentication from unsupported clients lacking MFA capability
   - Regular auditing of authentication methods detecting MFA bypass

4. **Risk-Based and Adaptive MFA:**
   - Implement conditional access policies adjusting MFA based on risk
   - Require MFA for authentication from unknown locations or devices
   - Deploy risk-based authentication considering user behavior and context
   - Implement impossible travel detection triggering additional MFA
   - Use device compliance as MFA factor (managed device requirement)
   - Deploy adaptive authentication stepping up requirements based on sensitivity

5. **MFA Enrollment and Lifecycle:**
   - Implement self-service MFA enrollment with identity verification
   - Support multiple MFA methods per user for redundancy
   - Provide secure token distribution and onboarding processes
   - Implement automated MFA enrollment monitoring and reminders
   - Establish secure MFA reset procedures preventing social engineering
   - Deploy helpdesk authentication for MFA support requests

6. **MFA Monitoring and Management:**
   - Monitor MFA enrollment coverage by user population
   - Alert on authentication attempts bypassing MFA
   - Track MFA failure rates and user experience issues
   - Monitor for MFA fatigue attacks and excessive prompts
   - Analyze MFA effectiveness through authentication metrics
   - Conduct quarterly MFA coverage audits and gap remediation
