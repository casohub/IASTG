# INF-04.07 — Cross-Environment Access Paths (On-Premises to Cloud)

## Summary

This test identifies and assesses connectivity paths between on-premises infrastructure and cloud environments, evaluating security controls at environment boundaries and hybrid infrastructure integration points.

## Risk

Hybrid cloud architectures create attack surface spanning on-premises and cloud environments with inconsistent security controls. Compromised on-premises systems provide pivot points into cloud infrastructure. Cloud identity synchronization exposes credentials across environments. VPN and Direct Connect links enable lateral movement between environments. Misconfigured cloud hybrid connectivity bypasses segmentation. Organizations face expanded compromise when attackers leverage hybrid connections traversing environment boundaries without equivalent security oversight at each crossing point.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Hybrid or multi-cloud architecture exists in scope
- Cloud and on-premises connectivity has been identified
- Testing includes cross-environment assessment

## Test Objectives

- Identify connectivity paths between on-premises and cloud environments
- Assess security controls at environment boundaries
- Test lateral movement across hybrid infrastructure
- Evaluate identity synchronization and credential exposure
- Validate segmentation between environments

## Test Methodology

1. Enumerate hybrid connectivity mechanisms:
   - **VPN Connections:** Site-to-site VPN between on-premises and cloud
   - **Direct Connect / ExpressRoute:** Dedicated private connections
   - **Virtual Network Peering:** Cloud-to-cloud or hybrid network peering
   - **Public Internet Exposure:** Services accessible via Internet
   - **Hybrid Identity:** Azure AD Connect, AD Federation Services (ADFS)
2. Test VPN and private connectivity paths:
   - Enumerate VPN tunnels and their endpoints
   - Test access from on-premises to cloud resources via VPN
   - Assess Direct Connect or ExpressRoute connectivity
   - Verify encryption on hybrid connectivity links
   - Test for segmentation within hybrid connections
3. Assess identity synchronization security:
   - Test Azure AD Connect or similar identity sync solutions
   - Identify synchronized accounts and credential exposure
   - Assess password hash synchronization security
   - Test for credential theft enabling cross-environment access
   - Verify MFA requirements for synchronized identities
4. Test lateral movement from on-premises to cloud:
   - Attempt cloud resource access from compromised on-premises system
   - Test credential reuse across environments
   - Assess API access from on-premises to cloud services
   - Verify cloud resource isolation from on-premises
   - Test for privilege escalation across environments
5. Evaluate cloud management access from on-premises:
   - Test on-premises access to cloud management consoles
   - Assess jumpbox or bastion host access to cloud
   - Verify segmentation of cloud management traffic
   - Test for privileged access paths crossing environments
   - Assess monitoring of cross-environment management activities
6. Test federated authentication paths:
   - Assess ADFS or federation infrastructure security
   - Test for ADFS server compromise enabling cloud access
   - Verify token signing certificate protection
   - Test for federation manipulation or trust abuse
   - Assess monitoring of federated authentication events
7. Assess cloud-to-on-premises access paths:
   - Test cloud resource access to on-premises systems
   - Verify reverse connectivity security controls
   - Assess hybrid application architectures
   - Test for cloud-initiated lateral movement to on-premises
   - Verify bidirectional security control parity
8. Evaluate hybrid infrastructure monitoring:
   - Verify logging captures cross-environment traffic
   - Test for detection of lateral movement across environments
   - Assess SIEM integration of hybrid infrastructure logs
   - Verify alerting on suspicious cross-environment activities
   - Test for visibility gaps at environment boundaries

## Expected Secure Configuration

Organizations implementing hybrid infrastructure should maintain:

- Zero trust architecture for all cross-environment access
- Multi-factor authentication (MFA) required for cloud access
- Network segmentation at environment boundaries
- Encrypted connectivity (VPN, TLS) for hybrid links
- Monitoring and logging of all cross-environment traffic
- Privilege access management spanning environments
- Identity synchronization with credential protection
- Regular security assessments of hybrid connectivity
- Incident response procedures for cross-environment attacks
- Consistent security controls across all environments

## Evidence to Collect

- Hybrid connectivity enumeration results
- VPN and Direct Connect configuration analysis
- Identity synchronization security assessment
- Lateral movement testing across environments
- Cloud management access path documentation
- Federated authentication security evaluation
- Bidirectional access testing results
- Monitoring and logging coverage assessment
- Security control consistency analysis across environments
- Cross-environment attack path documentation

## Impact

**Expanded Attack Surface Impact:**
Hybrid connectivity expands attack surface across on-premises and cloud environments. Single compromised on-premises system provides access to cloud resources. Cloud misconfigurations expose on-premises infrastructure. Attackers leverage connectivity paths as lateral movement routes. Organizations face multi-environment compromise from single breach point.

**Identity and Credential Exposure Impact:**
Password hash synchronization exposes hashed credentials across environments. Compromised Azure AD Connect server provides access to cloud identities. ADFS compromise enables token forgery and federation abuse. Credential reuse across environments multiplies compromise impact. Single credential theft affects both on-premises and cloud access.

**Privilege Escalation Across Environments Impact:**
Cloud administrator access from on-premises systems enables environment-spanning privilege escalation. Compromised hybrid identity connectors provide privileged access paths. Federation trust abuse escalates privileges across environments. Attackers leverage cross-environment access for maximum impact and persistence.

## MITRE ATT&CK Mapping

- **T1199** - Initial Access: Trusted Relationship
- **T1078.004** - Valid Accounts: Cloud Accounts
- **T1550.001** - Use Alternate Authentication Material: Application Access Token
- **T1484.002** - Domain Policy Modification: Domain Trust Modification
- **T1580** - Discovery: Cloud Infrastructure Discovery

## Remediation Guidance

Organizations should implement the following hybrid infrastructure security practices:

1. **Zero Trust for Cross-Environment Access:**
   - Implement zero trust architecture for all hybrid connectivity
   - Require continuous authentication and authorization
   - Deploy identity-based access controls spanning environments
   - Verify every access request regardless of source
   - Implement least privilege across all environments
   - Regular validation of access privileges and entitlements

2. **Secure Hybrid Connectivity:**
   - Deploy encrypted VPN with strong authentication
   - Use private connectivity (ExpressRoute, Direct Connect) where possible
   - Implement network segmentation at environment boundaries
   - Deploy firewalls controlling cross-environment traffic
   - Limit hybrid connectivity to required services only
   - Regular review of hybrid connectivity configurations

3. **Identity Synchronization Hardening:**
   - Deploy Azure AD Connect on dedicated hardened servers
   - Implement credential protection (credential guard, protected users)
   - Use password hash synchronization with strong protections
   - Deploy Pass-Through Authentication instead of password sync where feasible
   - Implement MFA for all synchronized accounts
   - Monitor Azure AD Connect servers comprehensively

4. **Federation Security:**
   - Harden ADFS and federation infrastructure
   - Protect token signing certificates with HSM
   - Implement MFA at ADFS authentication
   - Deploy ADFS on dedicated hardened servers
   - Monitor federation authentication events
   - Regular security assessments of federation infrastructure

5. **Cross-Environment Monitoring:**
   - Deploy unified SIEM spanning all environments
   - Implement cloud-native and hybrid security monitoring
   - Log all cross-environment authentication and access
   - Alert on unusual cross-environment activities
   - Deploy behavioral analytics for hybrid infrastructure
   - Integrate cloud and on-premises logs for correlation

6. **Privileged Access Management Across Environments:**
   - Implement PAM solutions spanning hybrid infrastructure
   - Use separate administrative accounts per environment
   - Deploy just-in-time (JIT) access for cloud and on-premises
   - Implement privileged access workstations (PAWs) for both environments
   - Monitor all privileged activities across environments
   - Regular recertification of cross-environment privileges
