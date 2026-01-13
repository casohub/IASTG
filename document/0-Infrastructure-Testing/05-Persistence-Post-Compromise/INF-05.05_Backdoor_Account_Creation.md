# INF-05.05 — Backdoor Account Creation or Usage

## Summary

This test identifies unauthorized user accounts created for persistent access, including hidden accounts, accounts with administrative privileges, and accounts bypassing normal authentication and monitoring.

## Risk

Backdoor accounts provide persistent authenticated access surviving password changes and security updates. Attackers create hidden or inconspicuous accounts avoiding detection. Administrative backdoor accounts enable privileged access across systems. Accounts added to privileged groups provide domain-wide persistence. Organizations face long-term unauthorized access when backdoor accounts operate undetected enabling continuous system access, data theft, and attack staging.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Access to identity management systems and domain controllers
- Ability to enumerate user accounts and group memberships
- Testing includes account security assessment

## Test Objectives

- Identify unauthorized or suspicious user accounts
- Detect backdoor accounts with administrative privileges
- Assess hidden account creation techniques
- Evaluate account monitoring and detection capabilities
- Validate access review and account lifecycle processes

## Test Methodology

1. Enumerate all user accounts:
   - List local user accounts on Windows systems
   - Query Active Directory for domain accounts
   - Enumerate Linux/Unix user accounts from /etc/passwd
   - Identify service accounts and system accounts
   - Review recently created accounts and timestamps
2. Identify suspicious account characteristics:
   - Accounts with generic or system-like names (support, admin, svc)
   - Accounts created outside business hours
   - Accounts with no logon history or recent activity
   - Accounts with passwords that never expire
   - Accounts with blank or weak passwords
3. Test for hidden or disguised accounts:
   - Accounts with names containing special characters or spaces
   - Accounts with dollar sign suffix appearing as system accounts (username$)
   - Accounts with Unicode or homoglyph characters
   - Accounts with AdminSDHolder protection indicator
   - Registry-hidden accounts (Windows)
4. Assess privileged account membership:
   - Identify accounts in Domain Admins or Enterprise Admins
   - List local administrators on systems
   - Enumerate accounts with administrative privileges
   - Review nested group memberships
   - Identify accounts with SID History containing privileged SIDs
5. Test backdoor account functionality:
   - Verify account authentication capabilities
   - Test remote access using backdoor accounts
   - Assess account persistence through password changes
   - Verify account survives through system reboots
   - Test detection of backdoor account usage
6. Analyze account creation and modification:
   - Review account creation logs and audit trails
   - Identify accounts created through non-standard methods
   - Assess account modification history
   - Verify account lifecycle management compliance
   - Test for automated account provisioning bypasses
7. Evaluate account privilege escalation:
   - Test for accounts with delegated administrative rights
   - Identify accounts with GPO modification permissions
   - Assess accounts with write access to critical AD objects
   - Review accounts with powerful extended rights
   - Test for privilege escalation through backdoor accounts
8. Assess account monitoring and detection:
   - Verify logging of account creation events
   - Test alerting on privileged account creation
   - Assess user account anomaly detection
   - Verify account access review processes
   - Test incident response for unauthorized accounts

## Expected Secure Configuration

Organizations managing user accounts should implement:

- Comprehensive user account inventory and tracking
- Regular account access reviews and recertification
- Automated provisioning and deprovisioning processes
- Strong account naming conventions preventing disguises
- Monitoring and alerting on account creation
- Privileged account management (PAM) solutions
- Account lifecycle management integrated with HR processes
- Multi-factor authentication on all accounts
- Regular audits of administrative group memberships
- Incident response procedures for unauthorized accounts

## Evidence to Collect

- Complete user account enumeration with creation dates
- Identification of suspicious or unauthorized accounts
- Privileged group membership analysis
- Hidden or disguised account discoveries
- Account creation and modification audit logs
- Backdoor account authentication testing results
- Account access review documentation
- Detection capability validation for account creation
- Timeline analysis of account lifecycle events
- Screenshots or exports of suspicious accounts

## Impact

**Persistent Authenticated Access Impact:**
Backdoor accounts provide persistent legitimate-appearing authentication bypassing credential theft detection. Accounts survive password resets and security updates maintaining access. Administrative backdoor accounts enable complete system and domain control. Organizations face long-term compromise when backdoor accounts operate as legitimate users.

**Privilege Escalation and Lateral Movement Impact:**
Backdoor accounts with administrative privileges enable immediate privilege escalation. Domain Admin backdoor accounts provide domain-wide access. Local administrator backdoor accounts enable lateral movement across systems. Nested group memberships hide privileged access through indirect membership.

**Detection Evasion and Attribution Loss Impact:**
Backdoor accounts blend with legitimate users evading behavioral analytics. Actions performed through backdoor accounts appear as authorized activities. Attack attribution fails when activities traced to fake or compromised-appearing accounts. Organizations struggle to distinguish backdoor account activity from legitimate user actions.

## MITRE ATT&CK Mapping

- **T1136.001** - Persistence: Create Account: Local Account
- **T1136.002** - Persistence: Create Account: Domain Account
- **T1098** - Persistence: Account Manipulation
- **T1078** - Valid Accounts (using backdoor accounts for access)
- **T1087** - Discovery: Account Discovery

## Remediation Guidance

Organizations should implement the following backdoor account prevention practices:

1. **Account Lifecycle Management:**
   - Implement automated account provisioning integrated with HR
   - Deploy automated deprovisioning when employees leave
   - Conduct regular account access reviews (quarterly minimum)
   - Recertify administrative account necessity annually
   - Remove inactive accounts after defined period (90 days)
   - Document and track all service account creation

2. **Account Creation Monitoring:**
   - Enable comprehensive logging of account creation (Event ID 4720)
   - Deploy real-time alerting on new account creation
   - Monitor privileged group membership additions (Event ID 4728, 4732)
   - Alert on accounts created outside business hours
   - Implement anomaly detection for account creation patterns
   - Deploy security information and event management (SIEM) correlation

3. **Privileged Account Controls:**
   - Implement privileged access management (PAM) solutions
   - Use time-limited administrative privileges
   - Deploy just-in-time (JIT) administrative access
   - Separate administrative accounts from standard user accounts
   - Eliminate standing administrative privileges
   - Regular audit of Domain Admins and Enterprise Admins membership

4. **Account Naming and Standards:**
   - Implement and enforce consistent naming conventions
   - Prohibit generic names (admin, support, test, svc)
   - Block special characters in usernames
   - Implement username uniqueness requirements
   - Document exceptions with business justification
   - Regular scans for naming convention violations

5. **Hidden Account Detection:**
   - Deploy automated scanning for hidden accounts
   - Monitor for accounts with special characters or Unicode
   - Detect accounts with dollar sign suffix disguising as system accounts
   - Search for accounts with AdminSDHolder protection
   - Scan registry for hidden account entries
   - Regular enumeration comparing multiple account sources

6. **Account Security Hardening:**
   - Require multi-factor authentication (MFA) for all accounts
   - Implement strong password policies and enforcement
   - Disable accounts after failed login attempts
   - Require password expiration for human accounts
   - Implement account lockout policies
   - Deploy password breach monitoring
   - Eliminate shared accounts where possible
