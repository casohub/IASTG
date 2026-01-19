# INF-06.02 — Log Integrity, Protection, and Retention Verification

## Summary

This test validates the integrity, protection, and retention of security logs, ensuring logs cannot be tampered with by attackers and are preserved for incident investigation and regulatory compliance.

## Risk

Unprotected logs enable attackers to destroy evidence of malicious activities through deletion or modification. Inadequate log retention prevents investigation of historical incidents and compliance violations. Logs accessible to compromised accounts allow evidence destruction. Short retention periods eliminate forensic evidence before breaches are discovered. Organizations face investigation failures and regulatory violations when log integrity and retention are inadequate.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Logging infrastructure has been identified (see INF-06.01)
- Access to log storage and management systems
- Testing includes log protection assessment

## Test Objectives

- Verify log integrity protection mechanisms
- Test log tampering and deletion capabilities
- Assess log retention policies and implementation
- Evaluate log backup and recovery procedures
- Validate log access controls and permissions

## Test Methodology

1. Test log modification and deletion capabilities:
   - Attempt to delete Windows Event Logs with administrative privileges
   - Test Linux log file deletion and modification
   - Verify SIEM log deletion or modification prevention
   - Test for selective log entry removal capabilities
   - Assess timestamp manipulation possibilities
2. Assess log integrity protection mechanisms:
   - Verify write-once-read-many (WORM) storage implementation
   - Test append-only log storage enforcement
   - Verify log signing and cryptographic verification
   - Assess immutable log storage deployment
   - Test log integrity monitoring and alerting
3. Evaluate log access controls:
   - Review permissions on log files and directories
   - Test unauthorized log access attempts
   - Verify least privilege on log management
   - Assess privileged access to log systems
   - Test for log access monitoring and alerting
4. Test log retention policies:
   - Verify retention periods for different log types
   - Test automated log rotation and archival
   - Assess compliance with regulatory requirements
   - Verify long-term archival implementation
   - Test log retrieval from archives
5. Assess log backup and recovery:
   - Verify log backup procedures and frequency
   - Test log restoration from backups
   - Assess backup retention and storage
   - Verify offsite or cloud backup implementation
   - Test disaster recovery for log infrastructure
6. Evaluate log storage security:
   - Verify encryption for logs at rest
   - Test encryption for log transmission
   - Assess log storage access controls
   - Verify secure log storage location
   - Test for unauthorized log storage access
7. Test log forwarding reliability:
   - Verify real-time log forwarding functionality
   - Test log forwarding during network disruptions
   - Assess log forwarding buffering and queuing
   - Verify log forwarding redundancy
   - Test for log forwarding monitoring
8. Assess regulatory compliance:
   - Verify retention meets PCI DSS requirements (1 year online, 3 years archived)
   - Assess HIPAA logging and retention compliance (6 years)
   - Test GDPR data processing logging
   - Verify SOX financial system logging (7 years)
   - Assess industry-specific requirements

## Expected Secure Configuration

Organizations protecting log integrity should implement:

- Immutable or write-once log storage
- Append-only log storage preventing modification
- Log signing with cryptographic verification
- Centralized log collection with immediate forwarding
- Restricted log access limited to security operations
- Automated log backup with offsite storage
- Minimum 90-day online retention (1 year preferred)
- Long-term archival meeting regulatory requirements (up to 7 years)
- Encryption for logs at rest and in transit
- Comprehensive log access auditing
- Monitoring and alerting on log tampering attempts

## Evidence to Collect

- Log modification and deletion testing results
- Log integrity protection mechanism validation
- Access control analysis for log systems
- Retention policy documentation and verification
- Backup and recovery testing results
- Storage security assessment
- Log forwarding reliability testing
- Regulatory compliance mapping
- Log tampering detection validation
- Recommendations for log protection enhancement

## Impact

**Evidence Destruction and Investigation Failure Impact:**
Unprotected logs enable attackers to destroy evidence preventing incident investigation. Log deletion eliminates forensic artifacts needed for breach analysis. Selective log modification obscures attacker activities. Organizations cannot reconstruct attack timelines or determine breach scope without protected logs. Legal and regulatory investigations fail without adequate evidence.

**Regulatory Non-Compliance Impact:**
Inadequate log retention violates regulatory requirements. PCI DSS requires one year online retention with three years archived. HIPAA mandates six years of audit log retention. SOX requires seven years of financial system logs. Organizations face penalties, failed audits, and enforcement actions for inadequate log retention.

**Extended Breach Discovery Impact:**
Short log retention prevents detection of historical compromises. Breaches remain undetected when evidence expires before discovery. Organizations lose ability to investigate incidents predating log retention period. Average breach dwell time of 200+ days exceeds many organizations' log retention.

## MITRE ATT&CK Mapping

- **T1070.001** - Defense Evasion: Indicator Removal: Clear Windows Event Logs
- **T1070.002** - Defense Evasion: Indicator Removal: Clear Linux or Mac System Logs
- **T1562.002** - Defense Evasion: Impair Defenses: Disable Windows Event Logging

## Remediation Guidance

Organizations should implement the following log protection practices:

1. **Immutable Log Storage:**
   - Deploy write-once-read-many (WORM) storage for logs
   - Implement append-only log storage systems
   - Use immutable cloud storage (AWS S3 Object Lock, Azure Immutable Blob Storage)
   - Deploy log signing with cryptographic verification
   - Implement blockchain-based log integrity verification
   - Prevent log deletion by any user including administrators

2. **Log Access Controls:**
   - Restrict log access to security operations team only
   - Implement role-based access control (RBAC) on log systems
   - Use separate administrator accounts for log management
   - Enable multi-factor authentication (MFA) for log system access
   - Implement just-in-time (JIT) access for log administration
   - Audit all log access comprehensively

3. **Centralized Log Collection:**
   - Deploy enterprise SIEM with secure log storage
   - Implement real-time log forwarding from all sources
   - Use encrypted log transmission (TLS, VPN)
   - Deploy redundant log collectors for reliability
   - Implement log forwarding monitoring and alerting
   - Buffer logs locally during forwarding failures

4. **Log Retention Policies:**
   
   **Minimum Requirements:**
   - 90 days online retention for all security logs
   - 1 year online retention for authentication and access logs
   - Extended retention for regulatory compliance:
     - PCI DSS: 1 year online, 3 years total
     - HIPAA: 6 years
     - SOX: 7 years for financial systems
     - GDPR: As required by data processing obligations
   
   **Implementation:**
   - Automated log rotation and archival
   - Separate retention policies per log type
   - Document retention justification
   - Regular compliance validation

5. **Log Backup and Recovery:**
   - Implement automated daily log backups
   - Deploy offsite or cloud backup storage
   - Test log restoration procedures quarterly
   - Maintain backup retention matching log retention
   - Encrypt backups at rest and in transit
   - Implement backup integrity verification
   - Document backup and recovery procedures

6. **Log Integrity Monitoring:**
   - Deploy log integrity monitoring solutions
   - Alert on log deletion or modification attempts
   - Monitor log file permissions and ownership
   - Implement file integrity monitoring (FIM) on log files
   - Alert on log forwarding interruptions
   - Monitor log storage capacity and availability
   - Deploy secondary logging capturing log system events
