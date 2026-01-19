# INF-06.07 — Lateral Movement Detection Capabilities

## Summary

This test evaluates the organization's ability to detect lateral movement techniques used by attackers to traverse network and access additional systems after initial compromise.

## Risk

Undetected lateral movement enables attackers to expand access across infrastructure reaching critical systems and data. Compromised credentials facilitate authenticated lateral movement appearing as legitimate activity. Pass-the-hash and pass-the-ticket attacks bypass password authentication. Remote execution tools enable system-to-system access. Organizations face extensive compromise when lateral movement proceeds undetected enabling attackers to access domain controllers, databases, and sensitive systems.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Initial access or compromise position established for testing
- Network and authentication monitoring in place
- Testing includes lateral movement assessment

## Test Objectives

- Test detection of common lateral movement techniques
- Assess credential-based lateral movement detection
- Evaluate remote execution detection capabilities
- Test network-based lateral movement visibility
- Validate behavioral analytics for movement patterns

## Test Methodology

1. Test pass-the-hash detection:
   - Execute pass-the-hash attacks using NTLM hashes
   - Verify detection of NTLM authentication from unusual sources
   - Test monitoring of NTLM authentication patterns
   - Assess detection of credential reuse across systems
   - Verify alerting on suspicious NTLM usage
2. Assess pass-the-ticket detection:
   - Execute pass-the-ticket attacks using Kerberos tickets
   - Verify detection of unusual Kerberos ticket usage
   - Test monitoring of ticket request patterns
   - Assess detection of ticket manipulation
   - Verify alerting on suspicious Kerberos activity
3. Test remote execution detection:
   - Execute PsExec lateral movement
   - Test detection of WinRM and PowerShell Remoting
   - Verify monitoring of WMI-based remote execution
   - Assess detection of RDP-based lateral movement
   - Test monitoring of SMB administrative shares
4. Evaluate remote desktop protocol (RDP) monitoring:
   - Test detection of unusual RDP connections
   - Verify monitoring of RDP from workstation-to-workstation
   - Assess detection of RDP from unusual sources
   - Test alerting on administrative RDP sessions
   - Verify RDP session logging and recording
5. Test lateral tool transfer detection:
   - Transfer tools between systems using various methods
   - Verify detection of suspicious file transfers
   - Test monitoring of SMB file transfers
   - Assess detection of HTTP/HTTPS downloads
   - Verify alerting on tool staging activities
6. Assess service-based lateral movement:
   - Test lateral movement via MSSQL services
   - Verify detection of database link abuse
   - Test monitoring of scheduled task creation remotely
   - Assess detection of service creation on remote systems
   - Verify alerting on service-based lateral movement
7. Test behavioral analytics:
   - Verify detection of unusual authentication patterns
   - Test detection of rapid sequential system access
   - Assess detection of authentication from multiple systems
   - Verify impossible travel detection for system access
   - Test detection of abnormal privileged account usage
8. Evaluate network-based detection:
   - Verify detection of lateral movement network traffic
   - Test monitoring of east-west traffic patterns
   - Assess detection of uncommon ports and protocols
   - Verify network segmentation violation detection
   - Test alerting on suspicious internal connections

## Expected Secure Configuration

Organizations detecting lateral movement should implement:

- Comprehensive authentication logging across systems
- Behavioral analytics detecting movement patterns
- Credential usage monitoring and anomaly detection
- Network traffic analysis for internal movements
- EDR deployment detecting remote execution
- Pass-the-hash and pass-the-ticket detection
- RDP and remote access monitoring
- File transfer and tool staging detection
- Integration with SIEM correlating activities
- Real-time alerting on lateral movement indicators

## Evidence to Collect

- Pass-the-hash detection testing results
- Pass-the-ticket detection validation
- Remote execution detection assessment
- RDP monitoring capability verification
- Tool transfer detection testing
- Service-based lateral movement detection
- Behavioral analytics effectiveness
- Network-based detection validation
- Alert generation and quality assessment
- Recommendations for detection improvement

## Impact

**Undetected Infrastructure Compromise Impact:**
Undetected lateral movement enables attackers to access domain controllers, file servers, databases, and critical infrastructure. Single compromised workstation escalates to complete network access. Organizations discover breaches only after extensive compromise when lateral movement operates undetected.

**Credential Theft Escalation Impact:**
Pass-the-hash and pass-the-ticket attacks leverage stolen credentials for authenticated lateral movement. Attackers access systems using legitimate credentials appearing as normal user activity. Organizations struggle to distinguish malicious lateral movement from legitimate administrative access.

**Extended Dwell Time Impact:**
Inability to detect lateral movement extends attacker dwell time enabling prolonged data theft and reconnaissance. Attackers spend weeks or months traversing networks undetected. Organizations face massive data breaches resulting from extended undetected lateral movement.

## MITRE ATT&CK Mapping

- **TA0008** - Lateral Movement (entire tactic)
- **T1021.002** - Lateral Movement: SMB/Windows Admin Shares
- **T1021.006** - Lateral Movement: Windows Remote Management
- **T1021.001** - Lateral Movement: Remote Desktop Protocol
- **T1550.002** - Use Alternate Authentication Material: Pass the Hash
- **T1550.003** - Use Alternate Authentication Material: Pass the Ticket

## Remediation Guidance

Organizations should implement the following lateral movement detection practices:

1. **Pass-the-Hash/Ticket Detection:**
   - Deploy Credential Guard on Windows 10/11 and Server 2016+
   - Implement LSA Protection preventing credential dumping
   - Monitor NTLM authentication from unusual sources
   - Alert on Kerberos ticket anomalies
   - Deploy honeypot credentials detecting credential theft
   - Implement Protected Users security group
   - Monitor Event ID 4624 (logon) with logon type analysis

2. **Remote Execution Monitoring:**
   - Deploy EDR monitoring remote execution techniques
   - Monitor Windows Admin Shares (C$, ADMIN$, IPC$) access
   - Alert on PsExec service creation (PSEXESVC)
   - Monitor WinRM and PowerShell Remoting usage
   - Detect WMI process creation remotely
   - Alert on scheduled task creation from remote systems
   - Monitor service creation and modifications

3. **RDP and Remote Access Monitoring:**
   - Log all RDP connection attempts (Event ID 4778, 4779)
   - Monitor RDP connections between workstations
   - Alert on RDP from unusual sources
   - Deploy RDP session recording
   - Implement jump host enforcement for RDP
   - Block workstation-to-workstation RDP
   - Monitor for RDP tunneling and port forwarding

4. **Behavioral Analytics:**
   - Deploy User and Entity Behavior Analytics (UEBA)
   - Implement credential usage baselines
   - Alert on unusual authentication patterns
   - Detect rapid sequential system access
   - Implement impossible travel detection for system access
   - Monitor for abnormal privileged account usage
   - Alert on authentication to many systems rapidly

5. **Network-Based Detection:**
   - Deploy network detection and response (NDR) solutions
   - Monitor east-west internal traffic
   - Detect unusual protocols and ports
   - Alert on segmentation boundary violations
   - Implement NetFlow or IPFIX analysis
   - Detect SMB traffic patterns indicating lateral movement
   - Monitor for tunneling and protocol manipulation

6. **Endpoint Detection and Response (EDR):**
   - Deploy EDR universally across endpoints and servers
   - Enable comprehensive process monitoring
   - Monitor network connections from processes
   - Alert on remote execution tool usage
   - Detect credential theft attempts
   - Implement automated response isolating systems
   - Integrate EDR with SIEM for correlation
