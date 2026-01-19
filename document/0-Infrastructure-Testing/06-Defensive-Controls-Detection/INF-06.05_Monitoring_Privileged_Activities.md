# INF-06.05 — Monitoring of Privileged Activities

## Summary

This test evaluates the monitoring and auditing of privileged account usage and administrative activities to detect unauthorized access, credential theft, and abuse of elevated privileges.

## Risk

Inadequate monitoring of privileged activities enables undetected abuse of administrative access. Compromised administrative credentials operate without detection. Insider threats leverage privileged access for unauthorized data access and system modification. Privilege escalation goes unnoticed without comprehensive monitoring. Organizations face extended compromise and insider threats when privileged activities lack visibility and monitoring.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Privileged accounts and administrative groups identified
- Access to logging and monitoring systems
- Testing includes privileged access assessment

## Test Objectives

- Assess monitoring coverage of privileged account usage
- Evaluate detection of suspicious administrative activities
- Test alerting on privilege escalation attempts
- Identify gaps in privileged activity visibility
- Validate privileged access management controls

## Test Methodology

1. Enumerate privileged accounts and activities:
   - Identify domain administrator accounts
   - List local administrator accounts
   - Enumerate service accounts with elevated privileges
   - Identify cloud administrative accounts
   - Review privileged security groups and roles
2. Test privileged authentication monitoring:
   - Verify logging of administrator logons (Event ID 4672, 4624)
   - Test detection of privileged account usage
   - Assess monitoring of service account authentication
   - Verify tracking of privileged group membership changes
   - Test detection of credential theft (pass-the-hash, ticket theft)
3. Assess administrative action monitoring:
   - Verify logging of user account creation and modification
   - Test monitoring of group membership changes
   - Assess logging of security policy modifications
   - Verify tracking of service installations
   - Test monitoring of scheduled task creation
4. Test privilege escalation detection:
   - Verify detection of privilege escalation attempts
   - Test monitoring of sudo usage on Linux
   - Assess detection of UAC bypass techniques
   - Verify monitoring of token manipulation
   - Test detection of service privilege abuse
5. Evaluate privileged access management (PAM):
   - Verify PAM solution deployment and coverage
   - Test privileged session recording
   - Assess just-in-time (JIT) privilege elevation
   - Verify credential vaulting for privileged accounts
   - Test approval workflows for privileged access
6. Test monitoring of critical system changes:
   - Verify logging of firewall rule modifications
   - Test monitoring of Active Directory changes
   - Assess logging of security tool configuration changes
   - Verify tracking of backup and restore operations
   - Test monitoring of GPO modifications
7. Assess privileged access from unusual locations:
   - Test detection of privileged access from new locations
   - Verify geographic anomaly detection
   - Assess detection of privileged access outside hours
   - Test monitoring of privileged remote access
   - Verify detection of impossible travel scenarios
8. Evaluate alert quality and response:
   - Test alerting on suspicious privileged activities
   - Verify alert severity and prioritization
   - Assess alert investigation procedures
   - Test integration with incident response
   - Verify privileged access reviews and audits

## Expected Secure Configuration

Organizations monitoring privileged activities should implement:

- Comprehensive logging of all privileged account usage
- Real-time monitoring and alerting on administrative activities
- Privileged access management (PAM) solution deployment
- Session recording for privileged access
- Behavioral analytics detecting anomalous privileged usage
- Regular privileged access reviews and audits
- Just-in-time (JIT) privilege elevation
- Multi-factor authentication (MFA) required for privileged access
- Geographic and time-based anomaly detection
- Integration with SIEM for correlation and investigation

## Evidence to Collect

- Privileged account inventory and classification
- Privileged authentication logging verification
- Administrative action monitoring assessment
- Privilege escalation detection testing results
- PAM solution deployment and coverage validation
- Critical system change monitoring verification
- Anomaly detection capability testing
- Alert generation and quality assessment
- Privileged access review procedures verification
- Recommendations for monitoring enhancement

## Impact

**Compromised Administrative Credential Impact:**
Unmonitored privileged access enables attackers to use stolen administrative credentials without detection. Pass-the-hash and pass-the-ticket attacks operate unnoticed. Compromised domain administrator accounts provide complete infrastructure control. Organizations discover credential theft only after extensive compromise without privileged activity monitoring.

**Insider Threat Impact:**
Lack of privileged activity monitoring enables insider threats to abuse administrative access. Insiders exfiltrate sensitive data using legitimate privileged access. Sabotage and system destruction proceed undetected. Organizations face data theft and operational disruption from insiders leveraging unmonitored privileged access.

**Privilege Escalation and Lateral Movement Impact:**
Undetected privilege escalation enables attackers to gain administrative rights. Lateral movement through compromised administrative credentials operates without alerts. Organizations fail to detect attack progression when privilege abuse lacks monitoring.

## MITRE ATT&CK Mapping

- **TA0004** - Privilege Escalation (detecting elevation attempts)
- **TA0006** - Credential Access (detecting credential theft)
- **T1078.003** - Valid Accounts: Local Accounts (admin account abuse)
- **T1098** - Account Manipulation (detecting account modification)

## Remediation Guidance

Organizations should implement the following privileged activity monitoring practices:

1. **Comprehensive Privileged Logging:**
   - Enable advanced audit policy on Windows
   - Log all privileged logon events (Event ID 4672)
   - Monitor security group membership changes (Event ID 4728, 4732, 4756)
   - Log privileged group enumeration attempts
   - Enable sudo logging on Linux (/var/log/auth.log)
   - Deploy auditd monitoring privileged commands
   - Log cloud administrative activities (CloudTrail, Activity Log)

2. **Privileged Access Management (PAM):**
   - Deploy enterprise PAM solution
   - Implement credential vaulting for privileged accounts
   - Enable session recording for administrative access
   - Deploy just-in-time (JIT) privilege elevation
   - Implement approval workflows for sensitive operations
   - Use privileged access workstations (PAWs)
   - Eliminate standing administrative privileges

3. **Behavioral Analytics:**
   - Deploy User and Entity Behavior Analytics (UEBA)
   - Implement baseline profiling for privileged accounts
   - Alert on anomalous privileged activities
   - Detect geographic anomalies (impossible travel)
   - Monitor privileged access outside normal hours
   - Detect unusual privileged command execution
   - Alert on rapid sequential privileged actions

4. **Real-Time Monitoring and Alerting:**
   - Integrate privileged logs with SIEM
   - Deploy real-time alerts on privileged activities
   - Alert on privileged account creation
   - Monitor privileged group membership additions
   - Alert on security policy changes
   - Detect privilege escalation attempts
   - Escalate high-severity privileged activity alerts

5. **Privileged Access Reviews:**
   - Conduct regular privileged account access reviews
   - Verify business justification for privileged access
   - Audit privileged group memberships quarterly
   - Review privileged access logs periodically
   - Recertify administrative access annually
   - Remove unnecessary privileged access
   - Document review outcomes and actions

6. **Multi-Factor Authentication (MFA):**
   - Require MFA for all privileged authentication
   - Deploy phishing-resistant MFA (FIDO2, certificates)
   - Implement step-up authentication for sensitive operations
   - Require MFA for privileged remote access
   - Monitor for MFA bypass attempts
   - Alert on privileged authentication without MFA
