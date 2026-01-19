# INF-06.01 — Logging Coverage Assessment (Network and Host Logs)

## Summary

This test evaluates the comprehensiveness of logging across network infrastructure and host systems, identifying gaps in log collection that prevent detection of security incidents and hinder forensic investigations.

## Risk

Inadequate logging coverage creates blind spots enabling undetected malicious activities. Missing network logs prevent detection of lateral movement and data exfiltration. Absent host logs eliminate visibility into system compromise and privilege escalation. Incomplete authentication logging hides credential theft and unauthorized access. Organizations face extended compromise dwell time when logging gaps prevent incident detection. Forensic investigations fail without adequate logs to reconstruct attack timelines and determine breach scope.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Access to logging infrastructure and configuration
- Network and system inventory available
- Testing includes logging assessment

## Test Objectives

- Assess comprehensiveness of network logging coverage
- Evaluate host-based logging across operating systems
- Identify logging gaps and blind spots
- Test log collection from critical security events
- Validate log retention and storage adequacy

## Test Methodology

1. Enumerate network logging sources:
   - **Firewall logs:** Connection attempts, blocks, allows
   - **Router and switch logs:** Network device events and changes
   - **VPN logs:** Remote access authentication and connections
   - **Proxy logs:** Web traffic and URL access
   - **IDS/IPS logs:** Network intrusion detection alerts
   - **DNS logs:** Query logging and resolution requests
   - **DHCP logs:** IP address assignments
   - **Load balancer logs:** Application traffic and health checks
2. Assess Windows host logging:
   - **Security logs:** Authentication, privilege use, audit events
   - **System logs:** Service status, driver loading, system events
   - **Application logs:** Application errors and events
   - **PowerShell logs:** Script block logging, transcription
   - **Sysmon logs:** Process creation, network connections, file creation
   - **Windows Defender logs:** Malware detection and remediation
3. Evaluate Linux/Unix logging:
   - **Syslog:** System messages and errors
   - **Auth.log:** Authentication attempts and authorization
   - **Audit.log:** Auditd security auditing events
   - **Secure log:** SSH access and sudo usage
   - **Kernel logs:** System kernel messages
   - **Application logs:** Service-specific logging
4. Test application and service logging:
   - **Web server logs:** Access logs, error logs (Apache, Nginx, IIS)
   - **Database logs:** Query logs, error logs, audit logs
   - **Email server logs:** Mail flow, authentication, spam filtering
   - **Authentication service logs:** RADIUS, LDAP, Active Directory
   - **Cloud service logs:** AWS CloudTrail, Azure Activity Log, GCP Audit
5. Assess security tool logging:
   - **Endpoint protection logs:** Antivirus, EDR detections and responses
   - **DLP logs:** Data loss prevention alerts and blocks
   - **WAF logs:** Web application firewall events
   - **Email security logs:** Email filtering and threat detection
   - **CASB logs:** Cloud access security broker events
6. Identify logging gaps and blind spots:
   - Systems without logging enabled
   - Network segments without traffic visibility
   - Critical events not being logged
   - Services running without audit logging
   - Unmonitored application activities
7. Test log collection and centralization:
   - Verify logs forwarded to central SIEM
   - Test real-time log collection reliability
   - Assess log parsing and normalization
   - Verify log collection from all sources
   - Test log collection during network issues
8. Evaluate log retention and storage:
   - Verify log retention periods meet requirements
   - Assess storage capacity and scalability
   - Test log archival processes
   - Verify backup and disaster recovery for logs
   - Assess compliance with retention regulations

## Expected Secure Configuration

Organizations implementing comprehensive logging should maintain:

- Network device logging enabled universally
- Host-based logging on all endpoints and servers
- Application and service logging configured
- Security tool logging and alerting enabled
- Centralized log collection to SIEM
- Minimum 90-day online log retention (1 year preferred)
- Long-term log archival meeting regulatory requirements
- Real-time log forwarding with redundancy
- Comprehensive coverage with minimal blind spots
- Regular logging coverage audits and validation

## Evidence to Collect

- Complete inventory of logging sources
- Network logging coverage assessment
- Host logging configuration analysis per operating system
- Application and service logging evaluation
- Security tool logging validation
- Logging gap identification and documentation
- Log collection and centralization verification
- Log retention and storage capacity assessment
- Compliance mapping for logging requirements
- Remediation recommendations for identified gaps

## Impact

**Undetected Security Incidents Impact:**
Logging gaps prevent detection of malicious activities enabling attackers to operate undetected. Missing authentication logs hide credential theft and unauthorized access. Absent network logs conceal lateral movement and data exfiltration. Organizations discover breaches only after significant damage when logging gaps prevent timely detection.

**Failed Forensic Investigations Impact:**
Inadequate logs prevent incident investigation and attack timeline reconstruction. Investigators cannot determine breach scope without comprehensive logs. Missing forensic evidence prevents root cause analysis and attacker attribution. Organizations struggle to satisfy breach notification requirements without adequate investigation.

**Compliance Violations and Audit Failures Impact:**
Regulatory frameworks mandate specific logging requirements. PCI DSS requires comprehensive audit trails. HIPAA demands access logging for PHI. GDPR requires data processing records. Organizations face compliance violations, penalties, and failed audits when logging coverage is inadequate.

## MITRE ATT&CK Mapping

- **Not Applicable** - Defensive control focused on detection capability

However, adequate logging enables detection of techniques across all MITRE ATT&CK tactics.

## Remediation Guidance

Organizations should implement the following comprehensive logging practices:

1. **Network Logging Implementation:**
   - Enable logging on all network devices (firewalls, routers, switches)
   - Deploy NetFlow or IPFIX for network traffic visibility
   - Enable DNS query logging
   - Implement VPN and remote access logging
   - Deploy network packet capture for investigations
   - Enable proxy and web gateway logging

2. **Host Logging Enhancement:**
   
   **Windows:**
   - Enable Security, System, Application event logs
   - Deploy Sysmon with comprehensive configuration
   - Enable PowerShell script block logging and transcription
   - Configure Windows Defender logging
   - Enable process creation auditing (Event ID 4688)
   - Deploy advanced audit policy configuration
   
   **Linux:**
   - Enable auditd with comprehensive rules
   - Configure centralized syslog forwarding
   - Enable SSH logging with verbose mode
   - Deploy osquery for endpoint visibility
   - Enable process accounting (psacct/acct)
   - Configure sudo logging

3. **Application and Service Logging:**
   - Enable web server access and error logging
   - Configure database audit logging
   - Enable email server comprehensive logging
   - Deploy application performance monitoring (APM)
   - Enable API gateway logging
   - Configure authentication service audit logging

4. **Security Tool Logging:**
   - Enable comprehensive logging on all security tools
   - Configure EDR detailed logging
   - Enable IDS/IPS full alerting
   - Deploy DLP with detailed event logging
   - Enable cloud security tool logging (CASB, CWPP)
   - Configure email security gateway logging

5. **Centralized Log Management:**
   - Deploy enterprise SIEM solution
   - Implement real-time log forwarding
   - Configure log parsing and normalization
   - Enable log correlation and analytics
   - Implement redundant log collectors
   - Deploy log collection monitoring and alerting

6. **Log Retention and Storage:**
   - Implement 90-day minimum online log retention
   - Extend retention to 1 year for security-critical logs
   - Deploy long-term archival for compliance (7 years for some regulations)
   - Implement log compression reducing storage requirements
   - Deploy scalable log storage infrastructure
   - Enable log backup and disaster recovery
   - Implement log storage encryption
