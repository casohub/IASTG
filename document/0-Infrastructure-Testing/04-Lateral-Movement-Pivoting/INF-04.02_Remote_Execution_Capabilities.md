# INF-04.02 — Remote Execution Capabilities (PsExec, WinRM)

## Summary

This test assesses the availability and security of remote code execution mechanisms including PsExec, Windows Remote Management (WinRM), PowerShell Remoting, and similar tools that enable command execution on remote systems.

## Risk

Remote execution capabilities provide legitimate administrative functions but enable attackers to execute commands, deploy malware, and establish persistence across multiple systems following credential compromise. PsExec and similar tools enable lateral movement without exploit development. Windows Remote Management (WinRM) provides remote PowerShell access for automation and malicious command execution. Unrestricted remote execution services allow attackers to pivot rapidly throughout networks. Weak authentication on remote execution endpoints enables unauthorized access. Organizations face rapid compromise propagation when remote execution capabilities lack proper authentication, authorization, and monitoring.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Credentials for testing have been provided or obtained
- Network access to target systems exists
- Testing includes remote execution assessment

## Test Objectives

- Identify systems with remote execution capabilities enabled
- Test authentication and authorization for remote execution
- Assess PsExec, WinRM, and PowerShell Remoting configuration
- Evaluate remote execution logging and monitoring
- Validate restrictions on remote code execution

## Test Methodology

1. Enumerate Windows remote execution services:
   - **PsExec:** Administrative share access (C$, ADMIN$) and remote service creation
   - **WinRM/PowerShell Remoting:** TCP ports 5985 (HTTP), 5986 (HTTPS)
   - **Remote Registry:** TCP port 445 with registry access
   - **WMI (Windows Management Instrumentation):** TCP port 135 and dynamic RPC
   - **DCOM:** TCP port 135 with DCOM execution
2. Test PsExec execution capabilities:
   - Verify administrative share accessibility for PsExec
   - Test PsExec command execution on remote systems
   - Assess service creation permissions for PsExec
   - Verify SMB signing requirements affecting PsExec
   - Test for antivirus or EDR detection of PsExec usage
3. Evaluate WinRM and PowerShell Remoting:
   - Test WinRM service availability on discovered systems
   - Verify PowerShell Remoting enablement status
   - Test remote PowerShell session establishment
   - Assess WinRM authentication methods (Basic, Kerberos, CredSSP)
   - Verify WinRM encryption and certificate configuration
4. Test WMI-based remote execution:
   - Verify WMI service accessibility remotely
   - Test process creation via WMI (Win32_Process.Create)
   - Assess event subscription-based WMI execution
   - Test for lateral movement through WMI
   - Verify DCOM hardening and restrictions
5. Assess additional remote execution methods:
   - **SCCM (System Center Configuration Manager):** Remote execution through configuration management
   - **Scheduled Tasks:** Remote task creation and execution
   - **Remote Services:** Service creation and manipulation
   - **BITS (Background Intelligent Transfer Service):** Job creation and execution
6. Test remote execution authentication:
   - Verify authentication requirements for remote execution
   - Test with local administrator credentials
   - Assess domain administrator remote execution rights
   - Test for non-administrative user remote execution permissions
   - Verify pass-the-hash susceptibility on remote execution
7. Evaluate remote execution logging and monitoring:
   - Verify command execution logging (PowerShell logging, process creation)
   - Test for monitoring of PsExec and WinRM usage
   - Assess logging of WMI process creation
   - Verify SIEM integration capturing remote execution events
   - Test for detection of lateral movement through remote execution
8. Assess remote execution restrictions:
   - Test Just Enough Administration (JEA) implementation
   - Verify AppLocker or application control preventing malicious execution
   - Assess constrained language mode for PowerShell
   - Test for antivirus or EDR blocking malicious remote execution
   - Verify firewall rules restricting remote execution services

## Expected Secure Configuration

Organizations managing remote execution capabilities should implement:

- WinRM restricted to administrative jump hosts or PAWs
- SMB signing enforced preventing relay attacks
- Administrative share access limited to authorized administrators
- PowerShell logging and transcription enabled
- Just Enough Administration (JEA) limiting PowerShell cmdlet access
- Constrained language mode for non-administrative PowerShell
- Comprehensive logging of remote execution events
- Network segmentation limiting remote execution to administrative networks
- Application control (AppLocker, WDAC) preventing unauthorized execution
- Monitoring and alerting on remote execution tools and techniques

## Evidence to Collect

- Inventory of systems with remote execution services enabled
- PsExec execution testing results and accessibility
- WinRM and PowerShell Remoting configuration analysis
- WMI remote execution capability testing results
- Authentication and authorization testing for remote execution
- Remote execution logging configuration verification
- Detection and monitoring capability validation
- JEA and PowerShell constraint implementation status
- Network accessibility of remote execution services
- Lateral movement path documentation through remote execution

## Impact

**Rapid Lateral Movement Impact:**
Remote execution capabilities enable attackers to move laterally across infrastructure executing commands on multiple systems. PsExec provides administrative command execution on any system with accessible admin shares. WinRM enables PowerShell execution for complex attack chains. Single credential compromise yields remote execution access to hundreds or thousands of systems.

**Malware Deployment and Persistence Impact:**
Attackers leverage remote execution to deploy ransomware across entire networks. Remote execution enables scheduled task creation for persistence. WMI event subscriptions provide stealthy persistence mechanisms. Malware deployment through legitimate administrative tools evades signature-based detection.

**Data Exfiltration and Destruction Impact:**
Remote execution enables attackers to access files, databases, and sensitive data across systems. PowerShell Remoting provides scripting capabilities for automated data exfiltration. Remote execution facilitates data destruction through command execution. Attackers leverage administrative tools appearing as legitimate activity.

## MITRE ATT&CK Mapping

- **T1021.002** - Lateral Movement: Remote Services: SMB/Windows Admin Shares
- **T1021.006** - Lateral Movement: Remote Services: Windows Remote Management
- **T1047** - Execution: Windows Management Instrumentation
- **T1569.002** - Execution: System Services: Service Execution
- **T1053.005** - Persistence: Scheduled Task/Job: Scheduled Task

## Remediation Guidance

Organizations should implement the following remote execution security practices:

1. **WinRM and PowerShell Remoting Hardening:**
   - Disable WinRM on workstations and non-administrative servers
   - Restrict WinRM access to administrative jump hosts and PAWs
   - Require HTTPS for WinRM connections (port 5986)
   - Implement certificate-based authentication for WinRM
   - Deploy Just Enough Administration (JEA) limiting PowerShell access
   - Enable constrained language mode for non-administrative users

2. **PowerShell Logging Enhancement:**
   - Enable PowerShell script block logging
   - Implement PowerShell transcription logging
   - Enable PowerShell module logging
   - Configure protected event logging for sensitive PowerShell operations
   - Centralize PowerShell logs to SIEM
   - Monitor for obfuscated or encoded PowerShell commands

3. **PsExec and SMB Hardening:**
   - Require SMB signing on clients and servers
   - Disable administrative shares (C$, ADMIN$) where not required
   - Implement network access control restricting SMB to authorized systems
   - Monitor for PsExec usage and PsExeSvc service creation
   - Deploy EDR solutions detecting and blocking PsExec abuse
   - Restrict local administrator rights limiting PsExec capabilities

4. **WMI Security Hardening:**
   - Implement WMI namespace security and access controls
   - Deploy WMI filtering through Group Policy
   - Monitor WMI process creation (Event ID 5857)
   - Restrict DCOM access through hardening configurations
   - Implement network-level restrictions on WMI traffic
   - Deploy detection rules for WMI-based lateral movement

5. **Network Segmentation and Access Control:**
   - Segment administrative networks from user networks
   - Implement firewall rules restricting WinRM to management VLANs
   - Block WMI and RPC from workstation-to-workstation
   - Deploy jump hosts for administrative remote execution
   - Implement microsegmentation limiting lateral movement
   - Restrict remote execution services to authorized source IPs

6. **Detection and Response:**
   - Deploy EDR solutions detecting remote execution techniques
   - Implement behavioral analytics identifying lateral movement
   - Monitor for service creation and process execution from remote systems
   - Alert on PowerShell execution from WinRM sessions
   - Deploy honeypot systems detecting unauthorized remote execution
   - Integrate remote execution logs with SIEM for correlation
   - Implement automated response blocking suspicious remote execution
