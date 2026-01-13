# INF-03.05 — Monitoring and Logging Services Exposure

## Summary

This test identifies monitoring, logging, and security information event management (SIEM) systems to assess their security posture, as these systems contain sensitive operational data and security intelligence that attackers seek to compromise or disable.

## Risk

Monitoring and logging systems contain comprehensive records of network activity, system events, security alerts, and incident data making them high-value targets for attackers seeking to understand defenses and cover tracks. Compromised SIEM platforms enable attackers to delete logs preventing forensic investigation and detection. Exposed monitoring systems reveal security architecture, detection capabilities, and defensive blind spots informing attacker tactics. Weak authentication on logging infrastructure allows log tampering destroying evidence. Unencrypted log transmission exposes sensitive data including credentials, user activities, and security events. Organizations lose ability to detect, respond to, or investigate security incidents when monitoring and logging infrastructure is compromised.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Defensive controls have been identified (see INF-01.09)
- Testing includes logging and monitoring system evaluation

## Test Objectives

- Identify all monitoring, logging, and SIEM infrastructure
- Assess authentication and access controls protecting monitoring systems
- Evaluate log data security including encryption and integrity
- Validate separation of logging infrastructure from production systems
- Test for log tampering or deletion capabilities

## Test Methodology

1. Enumerate SIEM and security monitoring platforms:
   - **Commercial SIEM:** Splunk, IBM QRadar, LogRhythm, ArcSight, Sentinel
   - **Open Source SIEM:** ELK Stack (Elasticsearch, Logstash, Kibana), Graylog, Wazuh
   - **Cloud SIEM:** Azure Sentinel, AWS Security Hub, Google Chronicle
   - Identify SIEM servers, search heads, indexers, and data collectors
2. Identify logging and log aggregation systems:
   - **Syslog Servers:** rsyslog, syslog-ng, centralized syslog collectors
   - **Windows Event Collectors:** Windows Event Forwarding (WEF), event subscription
   - **Log Shippers:** Fluentd, Filebeat, Logstash, NXLog
   - **Cloud Logging:** CloudWatch, Azure Monitor, Stackdriver
3. Enumerate monitoring and observability platforms:
   - **Infrastructure Monitoring:** Nagios, Zabbix, PRTG, SolarWinds
   - **Application Performance Monitoring (APM):** New Relic, Datadog, Dynatrace
   - **Network Monitoring:** SNMP managers, NetFlow collectors, packet capture systems
   - **Security Monitoring:** IDS/IPS management consoles, EDR platforms
4. Assess monitoring system authentication and access controls:
   - Verify multi-factor authentication (MFA) on monitoring consoles
   - Test for default monitoring software credentials
   - Assess role-based access control (RBAC) implementation
   - Test for privilege escalation through monitoring platforms
   - Verify API access controls and authentication
5. Test log data security:
   - Verify log encryption in transit (TLS for syslog, HTTPS for APIs)
   - Assess log storage encryption at rest
   - Test log integrity protection (digital signatures, hashing)
   - Verify immutable or write-once log storage
   - Test for log tampering or deletion capabilities
6. Evaluate log retention and protection:
   - Verify log retention policies meet regulatory requirements
   - Assess log backup and disaster recovery procedures
   - Test for log data loss prevention mechanisms
   - Verify log storage capacity and management
   - Test log archival and long-term storage security
7. Assess log transmission security:
   - Verify encrypted log transmission (TLS, VPN)
   - Test for cleartext log transmission
   - Assess log collector authentication
   - Verify mutual TLS or certificate authentication for log sources
   - Test for unauthorized log injection or manipulation
8. Evaluate monitoring system segmentation:
   - Test network segmentation isolating monitoring infrastructure
   - Verify monitoring systems operate in separate security zones
   - Assess firewall rules protecting monitoring services
   - Test for lateral movement paths from production to monitoring systems
   - Verify monitoring data flows and network architecture

## Expected Secure Configuration

Organizations deploying monitoring and logging systems should implement:

- Multi-factor authentication (MFA) required on all monitoring consoles
- Network segmentation isolating logging and monitoring infrastructure
- Encrypted log transmission using TLS 1.2 minimum
- Log storage encryption at rest
- Immutable or write-once log storage preventing tampering
- Comprehensive log retention meeting regulatory requirements (minimum 90 days, 1 year preferred)
- Regular log backup and disaster recovery testing
- Role-based access control (RBAC) with least privilege on monitoring platforms
- Audit logging of monitoring system access and configuration changes
- Separate administrative credentials for monitoring infrastructure

## Evidence to Collect

- Complete inventory of SIEM, logging, and monitoring systems
- Authentication testing results for monitoring consoles
- Log transmission encryption validation
- Log storage encryption and integrity verification
- Log retention policy documentation and validation
- Network segmentation validation for monitoring infrastructure
- Log tampering testing results
- Monitoring system access control analysis
- Log collector authentication and authorization testing
- Monitoring platform audit log configuration verification

## Impact

**Anti-Forensics and Evidence Destruction Impact:**
Compromised logging infrastructure enables attackers to delete logs destroying evidence of malicious activity. Log tampering modifies or removes entries concealing attack indicators. Security operations lose visibility into historical events preventing incident investigation. Compliance violations occur when required logs are unavailable for audit. Organizations cannot reconstruct attack timelines or understand breach scope without log data.

**Detection and Response Evasion Impact:**
Knowledge of monitoring capabilities enables attackers to evade detection by avoiding monitored activities. Disabled monitoring alerts prevent security operations from detecting ongoing attacks. Modified monitoring rules reduce detection accuracy creating blind spots. Compromised SIEM platforms provide attackers with visibility into detection logic enabling precise evasion.

**Sensitive Information Disclosure Impact:**
Logs contain sensitive data including usernames, IP addresses, authentication attempts, URLs, database queries, and application data. Unencrypted log transmission exposes credentials and user activities to interception. Compromised log repositories enable bulk exfiltration of operational intelligence. Logs reveal security architecture, detection capabilities, and organizational structure.

## MITRE ATT&CK Mapping

- **T1070.001** - Defense Evasion: Indicator Removal on Host: Clear Windows Event Logs
- **T1070.002** - Defense Evasion: Indicator Removal on Host: Clear Linux or Mac System Logs
- **T1562.001** - Impair Defenses: Disable or Modify Tools
- **T1562.008** - Impair Defenses: Disable Cloud Logs
- **T1036** - Defense Evasion: Masquerading (log spoofing)

## Remediation Guidance

Organizations should implement the following monitoring and logging security practices:

1. **Monitoring Infrastructure Hardening:**
   - Implement multi-factor authentication (MFA) on all monitoring consoles
   - Deploy network segmentation isolating monitoring infrastructure
   - Use unique credentials for monitoring administrators
   - Implement just-in-time (JIT) access for monitoring administration
   - Remove monitoring systems from production domains or use separate domains
   - Regular security assessments of monitoring infrastructure

2. **Log Transmission Security:**
   - Encrypt all log transmission using TLS 1.2 minimum
   - Implement mutual TLS authentication between log sources and collectors
   - Deploy syslog over TLS (RFC 5425) instead of cleartext syslog
   - Use VPN tunnels for log transmission over untrusted networks
   - Implement certificate-based authentication for log collectors
   - Block cleartext syslog protocols (UDP 514)

3. **Log Storage Protection:**
   - Implement log storage encryption at rest
   - Deploy write-once-read-many (WORM) or immutable storage for logs
   - Use append-only log storage preventing modification
   - Implement log integrity verification (digital signatures, cryptographic hashing)
   - Store critical logs on separate storage from production data
   - Implement access controls preventing log deletion by administrators

4. **Log Retention and Backup:**
   - Implement log retention policies meeting regulatory requirements (minimum 90 days)
   - Extend retention to 1 year for security-critical logs
   - Deploy automated log archival to long-term storage
   - Implement log backup with offsite or cloud storage
   - Regular testing of log restoration procedures
   - Document log retention rationale and compliance mapping

5. **Access Control and Segregation:**
   - Implement role-based access control (RBAC) on monitoring platforms
   - Apply principle of least privilege to monitoring system access
   - Separate read-only analysts from administrative access
   - Audit all monitoring system access and configuration changes
   - Deploy separate accounts for monitoring administration
   - Implement time-based access restrictions for sensitive operations

6. **Monitoring the Monitors:**
   - Deploy secondary logging capturing monitoring system activities
   - Implement monitoring system health checks and availability alerts
   - Monitor for log volume anomalies indicating log source failures
   - Alert on monitoring system authentication failures
   - Deploy integrity monitoring for log files and monitoring configurations
   - Implement out-of-band alerting for monitoring system compromises
   - Regular validation that monitoring systems are functioning correctly
