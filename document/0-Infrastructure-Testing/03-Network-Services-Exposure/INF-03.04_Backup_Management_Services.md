# INF-03.04 — Backup and Management Services Exposure

## Summary

This test identifies backup systems, management platforms, and orchestration services to assess their security posture, as these systems possess privileged access and control over critical infrastructure and data.

## Risk

Backup systems contain complete copies of organizational data including databases, file servers, email archives, and system configurations. Compromised backup infrastructure enables attackers to access all backed-up data, delete backups preventing recovery, or deploy ransomware destroying both production and backup data. Management platforms possess administrative credentials and control over multiple systems enabling infrastructure-wide compromise from single point. Orchestration services with privileged access to cloud resources provide attackers with ability to modify, delete, or exfiltrate data at scale. Exposed management consoles with weak authentication enable unauthorized administrative access. Organizations face catastrophic data loss and recovery failures when backup and management systems are compromised.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Business-critical assets have been identified (see INF-00.06)
- Testing includes backup and management system evaluation

## Test Objectives

- Identify all backup systems and data protection infrastructure
- Enumerate management platforms and orchestration services
- Assess authentication and access controls protecting critical management systems
- Evaluate backup data security and encryption
- Validate separation of backup infrastructure from production systems

## Test Methodology

1. Enumerate backup and data protection systems:
   - **Backup Software:** Veeam, CommVault, Veritas NetBackup, Rubrik, Cohesity
   - **Cloud Backup:** Azure Backup, AWS Backup, Google Cloud Backup
   - **Backup Appliances:** Dell EMC Data Domain, HPE StoreOnce
   - **Snapshot Systems:** Storage array snapshots, VM snapshots
   - Identify backup servers, repositories, and target storage locations
2. Identify management and orchestration platforms:
   - **Virtualization Management:** VMware vCenter, Hyper-V Manager, Proxmox
   - **Cloud Management:** Azure Portal, AWS Console, GCP Console
   - **Configuration Management:** Ansible, Puppet, Chef, SaltStack
   - **Container Orchestration:** Kubernetes, Docker Swarm, OpenShift
   - **Infrastructure as Code:** Terraform, CloudFormation
3. Enumerate systems management and monitoring platforms:
   - **Systems Management:** SCCM, WSUS, Red Hat Satellite
   - **Monitoring Platforms:** SCOM, Nagios, Zabbix, Prometheus
   - **Remote Management:** BMC, IPMI, iLO, iDRAC, Lights-Out Management
   - **Asset Management:** ServiceNow, CMDB platforms
4. Assess backup system authentication and access controls:
   - Verify multi-factor authentication (MFA) on backup consoles
   - Test for default backup software credentials
   - Assess backup administrator account privileges
   - Verify backup infrastructure access restrictions
   - Test for API access controls on backup systems
5. Test management platform security:
   - Verify MFA requirement on all management consoles
   - Test for weak credentials on management platforms
   - Assess role-based access control (RBAC) implementation
   - Verify audit logging of management actions
   - Test for privilege escalation through management platforms
6. Evaluate backup data security:
   - Verify backup encryption at rest
   - Test backup data encryption in transit
   - Assess backup encryption key management
   - Verify immutable or write-once-read-many (WORM) backup implementation
   - Test for air-gapped or offline backup copies
7. Assess backup and recovery procedures:
   - Test backup restoration processes
   - Verify backup data integrity and recoverability
   - Assess recovery time objectives (RTO) and recovery point objectives (RPO)
   - Test disaster recovery procedures
   - Verify backup testing and validation schedules
8. Evaluate separation and segmentation:
   - Test network segmentation isolating backup infrastructure
   - Verify backup systems are not domain-joined or in separate domains
   - Assess whether backup credentials differ from production credentials
   - Test for lateral movement paths from production to backup systems
   - Verify management platform isolation from general networks

## Expected Secure Configuration

Organizations deploying backup and management systems should implement:

- Multi-factor authentication (MFA) required on all backup and management consoles
- Network segmentation isolating backup and management infrastructure
- Backup encryption at rest and in transit with strong key management
- Immutable backups or WORM storage preventing deletion
- Air-gapped or offline backup copies for ransomware protection
- Separate administrative credentials for backup and management systems
- Principle of least privilege on backup and management accounts
- Comprehensive audit logging of backup and management activities
- Regular backup testing and restoration validation
- Disaster recovery plans with documented recovery procedures

## Evidence to Collect

- Complete inventory of backup systems and data protection infrastructure
- Management platform identification and configuration assessment
- Authentication testing results for backup and management consoles
- Backup encryption validation (at rest and in transit)
- Immutable backup implementation verification
- Network segmentation validation for backup infrastructure
- Backup administrator privilege assessment
- Backup restoration testing results
- Management platform access control analysis
- Audit logging configuration verification for backup and management systems

## Impact

**Ransomware and Data Destruction Impact:**
Compromised backup systems enable attackers to delete backups before deploying ransomware eliminating recovery options. Organizations face permanent data loss when both production and backup data are destroyed. Ransomware targeting backup infrastructure (Veeam, backup shares) prevents business continuity. Recovery failures occur when backups are corrupted or inaccessible.

**Complete Infrastructure Compromise Impact:**
Management platforms with administrative access to all systems enable infrastructure-wide compromise from single credential theft. Attackers leverage orchestration platforms to deploy malware across entire environments. Cloud management console access provides ability to delete resources, modify configurations, and exfiltrate data at scale. Virtualization management compromise enables access to all virtual machines and hypervisors.

**Data Exfiltration from Backups Impact:**
Backup repositories contain complete copies of sensitive data spanning years. Attackers access backup archives retrieving historical data including deleted files and old credentials. Database backups expose structured data for bulk exfiltration. Email archive backups contain confidential communications and intellectual property. Unencrypted backups expose all organizational data in centralized location.

## MITRE ATT&CK Mapping

- **T1490** - Impact: Inhibit System Recovery (backup deletion)
- **T1485** - Impact: Data Destruction (backup corruption)
- **T1213** - Collection: Data from Information Repositories (backup data access)
- **T1530** - Collection: Data from Cloud Storage Object
- **T1078.004** - Valid Accounts: Cloud Accounts (management console access)

## Remediation Guidance

Organizations should implement the following backup and management security practices:

1. **Backup Infrastructure Hardening:**
   - Implement multi-factor authentication (MFA) on all backup consoles
   - Deploy network segmentation isolating backup infrastructure
   - Remove backup systems from Active Directory domain or use separate domain
   - Use unique credentials for backup administrators
   - Implement just-in-time (JIT) access for backup administration
   - Regular security assessments of backup infrastructure

2. **Backup Data Protection:**
   - Implement backup encryption at rest with strong encryption (AES-256)
   - Encrypt backup data in transit using TLS 1.2 minimum
   - Deploy immutable backups or WORM storage preventing deletion
   - Implement air-gapped backup copies disconnected from network
   - Create offline backup copies stored physically separate
   - Secure backup encryption keys in hardware security modules (HSM)

3. **Management Platform Security:**
   - Require MFA for all management console access
   - Implement privileged access workstations (PAWs) for management tasks
   - Deploy role-based access control (RBAC) with least privilege
   - Use separate administrator accounts per platform
   - Implement session recording for management activities
   - Deploy just-in-time (JIT) privilege elevation for management access

4. **3-2-1 Backup Rule Implementation:**
   - Maintain 3 copies of data (production plus 2 backups)
   - Store backups on 2 different media types
   - Keep 1 copy offsite or air-gapped
   - Implement immutable backups for at least one copy
   - Regular testing of offsite and air-gapped backup restoration

5. **Backup Testing and Validation:**
   - Conduct regular backup restoration testing (monthly minimum)
   - Verify backup data integrity through automated validation
   - Test disaster recovery procedures annually
   - Document and update recovery procedures
   - Validate recovery time objectives (RTO) and recovery point objectives (RPO)
   - Maintain backup restoration playbooks

6. **Monitoring and Auditing:**
   - Enable comprehensive audit logging on backup systems
   - Monitor for backup deletion or modification attempts
   - Alert on failed backup jobs or integrity issues
   - Implement anomaly detection for unusual backup access patterns
   - Log all management platform authentication and actions
   - Integrate backup and management logs with SIEM
   - Alert on backup administrator account usage outside maintenance windows
