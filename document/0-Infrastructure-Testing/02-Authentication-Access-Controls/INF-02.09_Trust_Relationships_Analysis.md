# INF-02.09 — Trust Relationships Between Systems (Delegation, Domains)

## Summary

This test identifies and analyzes trust relationships between systems, domains, and services including authentication delegation, domain trusts, and cross-system authentication that enable privilege flow and lateral movement.

## Risk

Trust relationships create authentication pathways enabling credential delegation and privilege escalation across security boundaries. Unconstrained Kerberos delegation allows compromised servers to impersonate any user to any service creating domain-wide privilege escalation. Overly permissive domain trusts enable attackers to traverse forest boundaries and compromise separate organizational units. Bidirectional trusts unnecessarily increase attack surface by allowing authentication in both directions. Service-to-service trusts without proper restrictions enable lateral movement through delegated credentials. Organizations face complete infrastructure compromise when trust relationships are exploited to escalate privileges or traverse security boundaries.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Access to identity systems and domain infrastructure
- Understanding of organizational structure and administrative boundaries
- Testing includes trust relationship analysis

## Test Objectives

- Enumerate all trust relationships between domains and systems
- Identify Kerberos delegation configurations and constraints
- Assess trust relationship security and necessity
- Test for trust exploitation enabling privilege escalation
- Evaluate trust relationship monitoring and governance

## Test Methodology

1. Enumerate Active Directory domain trusts:
   - Query domain trust relationships using tools (nltest, Get-ADTrust, PowerShell)
   - Identify parent-child trusts within forests
   - Enumerate external trusts to other forests
   - Identify forest trusts and cross-forest authentication
   - Document trust directions (one-way, bidirectional)
   - Assess trust types (transitive, non-transitive)
2. Analyze Kerberos delegation configurations:
   - **Unconstrained Delegation:** Identify accounts and computers with unconstrained delegation enabled
   - **Constrained Delegation:** Enumerate accounts with protocol transition and constrained delegation
   - **Resource-Based Constrained Delegation (RBCD):** Identify systems with RBCD configured
   - Query msDS-AllowedToDelegateTo and msDS-AllowedToActOnBehalfOfOtherIdentity attributes
3. Identify service account delegation:
   - Enumerate service accounts with delegation privileges
   - Assess service principal names (SPNs) associated with delegated accounts
   - Test for service accounts with both delegation and high privileges
   - Identify service accounts trusted for delegation to critical services
4. Test for privilege escalation through delegation:
   - Verify unconstrained delegation enables impersonation attacks
   - Test constrained delegation for unauthorized service access
   - Assess resource-based constrained delegation misconfigurations
   - Test for privilege escalation from compromised delegated systems
5. Enumerate cross-domain and cross-forest authentication:
   - Identify users and services with cross-domain authentication rights
   - Test for selective authentication on trusts
   - Assess SID filtering implementation on external trusts
   - Verify authentication firewall rules between trusted domains
6. Analyze trust authentication flow:
   - Document authentication paths across trust relationships
   - Identify privilege escalation paths through trust chains
   - Map transitive trust relationships and their implications
   - Test for cross-forest privilege escalation
7. Assess trust relationship necessity and least privilege:
   - Verify each trust relationship has documented business justification
   - Test whether trusts can be removed or converted to one-way
   - Assess whether selective authentication limits trust scope
   - Identify over-permissive trust relationships
8. Evaluate trust relationship monitoring:
   - Verify logging of cross-domain authentication events
   - Test for monitoring of delegation usage
   - Assess alerting on suspicious trust relationship usage
   - Verify audit of trust relationship modifications

## Expected Secure Configuration

Organizations managing trust relationships should implement:

- Elimination of unconstrained Kerberos delegation
- Constrained delegation with service-specific restrictions
- Resource-based constrained delegation where appropriate
- One-way domain trusts instead of bidirectional where possible
- Selective authentication enabled on external trusts
- SID filtering enabled on all external trusts
- Regular trust relationship review and justification
- Monitoring and alerting on delegation and cross-domain authentication
- Documentation of all trust relationships with business justification
- Principle of least privilege applied to trust relationships

## Evidence to Collect

- Complete inventory of Active Directory domain trusts with directions and types
- Enumeration of accounts and computers with Kerberos delegation
- Service account delegation configuration and SPN associations
- Cross-domain authentication testing results
- Trust relationship privilege escalation path analysis
- SID filtering and selective authentication configuration
- Trust relationship business justification documentation
- Delegation and cross-domain authentication logs
- Trust relationship modification audit history
- Trust exploitation testing results

## Impact

**Unconstrained Delegation Privilege Escalation Impact:**
Compromised systems with unconstrained delegation enable attackers to impersonate any user to any service. Domain Administrator tickets cached on delegated systems provide complete domain compromise. Single compromised web server or SQL server with unconstrained delegation yields domain-wide administrative access through ticket theft.

**Cross-Domain Trust Exploitation Impact:**
Bidirectional trusts enable attackers to traverse domain and forest boundaries. Compromised accounts in one domain provide access to trusted domains. Transitive trust chains create privilege escalation paths spanning multiple forests. SID history abuse through trusts enables privilege inheritance across domain boundaries.

**Lateral Movement via Delegated Credentials Impact:**
Service accounts with delegation enable lateral movement to additional systems. Constrained delegation misconfigurations allow unauthorized service impersonation. Attackers leverage delegated credentials to access databases, file servers, and administrative systems. Credential delegation creates credential flow enabling privilege escalation.

## MITRE ATT&CK Mapping

- **T1558.003** - Steal or Forge Kerberos Tickets: Kerberoasting (exploiting delegation)
- **T1134.001** - Access Token Manipulation: Token Impersonation/Theft
- **T1550.003** - Use Alternate Authentication Material: Pass the Ticket
- **T1482** - Discovery: Domain Trust Discovery
- **T1484.002** - Domain Policy Modification: Domain Trust Modification

## Remediation Guidance

Organizations should implement the following trust relationship security practices:

1. **Kerberos Delegation Hardening:**
   - Eliminate unconstrained delegation completely from all accounts and computers
   - Migrate to constrained delegation with service-specific restrictions
   - Implement resource-based constrained delegation (RBCD) for modern applications
   - Mark sensitive accounts "Account is sensitive and cannot be delegated"
   - Add privileged accounts to Protected Users security group preventing delegation
   - Regular auditing identifying new delegation configurations

2. **Domain Trust Optimization:**
   - Convert bidirectional trusts to one-way where business requirements allow
   - Remove unnecessary trust relationships without active usage
   - Document business justification for each remaining trust
   - Implement selective authentication on all external trusts
   - Configure trust authentication firewall restricting cross-domain access
   - Quarterly trust relationship review and justification

3. **SID Filtering Implementation:**
   - Enable SID filtering on all external and forest trusts
   - Verify SID filtering prevents SID history abuse
   - Test SID filtering effectiveness regularly
   - Monitor for SID filtering bypass attempts
   - Document any SID filtering exceptions with security approval

4. **Service Account Delegation Management:**
   - Minimize service accounts with delegation privileges
   - Use group Managed Service Accounts (gMSA) restricting delegation
   - Implement least privilege for delegated service accounts
   - Remove delegation from service accounts with high privileges
   - Regular review of service account delegation configurations
   - Monitor for service account compromise enabling delegation abuse

5. **Trust Relationship Monitoring:**
   - Enable comprehensive logging of cross-domain authentication
   - Alert on delegation usage by sensitive accounts
   - Monitor for privilege escalation through trust relationships
   - Deploy anomaly detection for unusual cross-domain access patterns
   - Log and alert on trust relationship modifications
   - Regular analysis of delegation and cross-domain authentication logs

6. **Administrative Tier Isolation:**
   - Prevent Tier 0 (domain) accounts from authenticating to lower tiers
   - Implement authentication silos preventing cross-tier delegation
   - Use separate administrative accounts per domain or forest
   - Deploy authentication policies enforcing tier isolation
   - Restrict credential flow between administrative tiers
   - Regular validation of tier isolation effectiveness
