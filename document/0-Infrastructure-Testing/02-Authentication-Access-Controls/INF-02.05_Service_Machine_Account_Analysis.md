# INF-02.05 — Service and Machine Account Usage Analysis

## Summary

This test analyzes service accounts and machine accounts across infrastructure to identify excessive privileges, weak password management, credential exposure risks, and inappropriate usage patterns that create security vulnerabilities.

## Risk

Service accounts frequently possess excessive privileges enabling significant damage if compromised. Weak password policies applied to service accounts allow brute-force attacks against high-privilege credentials. Service account credentials embedded in scripts or configurations persist without rotation creating permanent backdoors. Machine accounts with administrator privileges enable privilege escalation through compromise of lower-privilege systems. Lack of monitoring and logging for service account activities prevents detection of account misuse. Organizations face complete infrastructure compromise when attackers obtain service account credentials with domain-wide or system-wide privileges.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication mechanisms have been identified (see INF-02.01)
- Access to identity systems for account enumeration
- Testing includes service account analysis

## Test Objectives

- Enumerate all service and machine accounts across infrastructure
- Assess service account privilege levels and permissions
- Identify service account password management practices
- Detect service accounts with administrative or excessive privileges
- Evaluate service account monitoring and logging coverage

## Test Methodology

1. Enumerate service accounts from identity systems:
   - **Windows Active Directory:** Service accounts, managed service accounts (MSA), group managed service accounts (gMSA)
   - **Linux/Unix:** System service accounts (daemon users), application service accounts
   - **Applications:** Database service accounts, application pool identities, API service accounts
   - **Cloud Platforms:** Service principals, managed identities, service accounts
2. Identify machine accounts and computer accounts:
   - Active Directory computer accounts
   - Domain-joined system accounts
   - Service mesh or container platform service accounts
   - Cloud compute instance service accounts
3. Analyze service account naming conventions:
   - Identify accounts by naming patterns (svc-, service-, app-, etc.)
   - Assess whether naming conventions indicate account purpose
   - Detect undocumented or non-standard service accounts
4. Assess service account privilege levels:
   - Identify service accounts with Domain Admin or Enterprise Admin membership
   - Enumerate service accounts with local administrator rights
   - Assess database accounts with sysadmin or elevated privileges
   - Identify cloud service accounts with broad IAM permissions
   - Test for principle of least privilege violations
5. Evaluate service account password management:
   - Test service account password strength and complexity
   - Identify service accounts with non-expiring passwords
   - Assess whether service accounts use managed service accounts (MSA/gMSA)
   - Detect service accounts with passwords same as username
   - Test for weak or default service account passwords
6. Identify service account credential exposure:
   - Search scripts and configuration files for embedded service account passwords
   - Check application configuration files for cleartext credentials
   - Identify service accounts stored in Group Policy Preferences
   - Assess service account credentials in code repositories
   - Test for service account credentials in environment variables
7. Analyze service account authentication methods:
   - Verify service accounts use appropriate authentication (Kerberos vs NTLM)
   - Identify service accounts with interactive logon rights
   - Assess service accounts with network authentication permissions
   - Test for service accounts with RDP or SSH access
8. Evaluate service account monitoring and logging:
   - Test whether service account authentication is logged
   - Assess service account activity monitoring
   - Identify anomalous service account behaviors
   - Verify service account usage auditing
   - Test for service account session monitoring

## Expected Secure Configuration

Organizations managing service and machine accounts should implement:

- Minimal service account usage with preference for managed identities
- Group Managed Service Accounts (gMSA) for Windows environments
- Unique service accounts per application or service
- Principle of least privilege with minimal permissions per account
- No service accounts with domain or enterprise administrator privileges
- Mandatory password complexity and length for service accounts (20+ characters)
- Automated password rotation for service accounts
- No embedded credentials in scripts, configurations, or code
- Secrets management integration for service account credential storage
- Monitoring and alerting on service account authentication and activities
- Regular service account permission and usage reviews

## Evidence to Collect

- Complete inventory of service and machine accounts
- Service account privilege assessment showing group memberships and permissions
- Service account password policy validation results
- Embedded credential discoveries in scripts and configurations
- Service account authentication method analysis
- Service account with excessive privilege identification
- Managed service account (MSA/gMSA) implementation status
- Service account monitoring and logging configuration
- Service account activity analysis showing usage patterns
- Documentation of service account owners and purposes

## Impact

**High-Privilege Credential Compromise Impact:**
Service accounts with Domain Admin or administrator privileges provide immediate privilege escalation paths. Single compromised service account credential grants domain-wide or system-wide access. Attackers leverage privileged service accounts for lateral movement, data access, and persistence. Excessive permissions on service accounts enable ransomware deployment, data exfiltration, and infrastructure destruction.

**Persistent Access and Backdoors Impact:**
Non-rotating service account passwords provide permanent authentication once stolen. Embedded credentials in scripts or configurations survive password changes and credential resets. Service accounts without interactive logon restrictions enable attackers to use credentials for unauthorized access. Weak service account passwords enable rapid cracking and unauthorized use.

**Undetected Malicious Activity Impact:**
Lack of service account monitoring prevents detection of account misuse. Attackers blend malicious activities with legitimate service account operations. Service account compromise remains undetected during incident response. Insufficient logging hinders forensic investigation of service account abuse.

## MITRE ATT&CK Mapping

- **T1078.003** - Valid Accounts: Local Accounts (service account abuse)
- **T1078.002** - Valid Accounts: Domain Accounts (domain service account compromise)
- **T1550.002** - Use Alternate Authentication Material: Pass the Hash (service account NTLM)
- **T1134** - Access Token Manipulation (service account token abuse)
- **T1098** - Account Manipulation (service account privilege escalation)

## Remediation Guidance

Organizations should implement the following service account management practices:

1. **Minimize Service Account Usage:**
   - Replace service accounts with managed identities where possible (Azure, AWS, GCP)
   - Use Windows Group Managed Service Accounts (gMSA) eliminating password management
   - Deploy containerized applications with service mesh identities
   - Eliminate service accounts for applications supporting Windows authentication
   - Document justification for each service account

2. **Principle of Least Privilege:**
   - Grant service accounts minimum permissions required for function
   - Remove Domain Admin and Enterprise Admin from all service accounts
   - Limit local administrator rights to specific systems only where required
   - Implement just-in-time (JIT) privilege elevation for service maintenance
   - Regular permission reviews removing unused or excessive privileges

3. **Service Account Password Security:**
   - Implement minimum 20-character passwords for service accounts
   - Deploy automated password rotation (90 days maximum for manual accounts)
   - Use cryptographically random password generation
   - Eliminate password expiration for gMSA accounts
   - Implement account lockout for service accounts detecting brute-force

4. **Credential Protection and Storage:**
   - Eliminate embedded credentials in scripts, configs, and code
   - Deploy secrets management solutions (Azure Key Vault, AWS Secrets Manager, HashiCorp Vault)
   - Implement secrets rotation automation
   - Encrypt service account credentials at rest
   - Apply strict access controls to secrets management systems

5. **Service Account Governance:**
   - Maintain centralized service account inventory with owners and purposes
   - Implement service account creation approval workflow
   - Conduct quarterly service account permission reviews
   - Disable or remove unused service accounts
   - Tag service accounts with metadata for tracking and governance
   - Implement service account lifecycle management

6. **Monitoring and Detection:**
   - Enable comprehensive logging for service account authentication
   - Implement alerting on service account interactive logon attempts
   - Monitor service account authentication from unusual locations or systems
   - Deploy behavioral analytics detecting anomalous service account activities
   - Alert on service account privilege escalation or permission changes
