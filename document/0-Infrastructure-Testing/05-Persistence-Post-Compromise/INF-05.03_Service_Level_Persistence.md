# INF-05.03 — Service-Level Persistence (Windows Services, Daemons)

## Summary

This test identifies unauthorized or malicious system services and daemons that provide persistence through automatic execution at system startup with elevated privileges.

## Risk

System services and daemons run automatically at boot with elevated privileges providing ideal persistence mechanisms for attackers. Malicious Windows services execute with SYSTEM authority enabling complete system control. Linux daemons run as root providing unrestricted access. Service-based persistence survives reboots and user logouts maintaining continuous attacker access. Organizations face persistent compromise when malicious services operate undetected executing attacker code with highest privileges at every system startup.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Administrative access to systems exists for service enumeration
- Ability to analyze service configurations and executables
- Testing includes persistence mechanism assessment

## Test Objectives

- Enumerate all system services and daemons
- Identify unauthorized or malicious service installations
- Assess service security and access controls
- Test detection capabilities for malicious services
- Validate service monitoring and logging

## Test Methodology

1. Enumerate Windows services:
   - List all services using `sc query` or `Get-Service`
   - Review Services console for suspicious services
   - Examine service registry keys in `HKLM\SYSTEM\CurrentControlSet\Services`
   - Identify services running as SYSTEM or administrator
   - Review service startup types (automatic, manual, disabled)
2. Analyze Linux/Unix daemons and services:
   - List systemd services using `systemctl list-units --type=service`
   - Review SysV init scripts in `/etc/init.d/`
   - Examine systemd unit files in `/etc/systemd/system/` and `/lib/systemd/system/`
   - Identify services enabled at boot using `systemctl list-unit-files`
   - Review upstart configurations in `/etc/init/` (if applicable)
3. Test for malicious service indicators:
   - Services with suspicious or generic names
   - Services with no description or publisher information
   - Services executing from temporary or user directories
   - Services with unsigned or unknown executables
   - Recently installed services without business justification
4. Identify persistence through service abuse:
   - Services configured for automatic startup
   - Services running under SYSTEM or root privileges
   - Services executing PowerShell, cmd.exe, or scripting interpreters
   - Services with network connectivity to suspicious destinations
   - Services masquerading as legitimate Windows or system services
5. Assess service binary and configuration security:
   - Verify digital signatures on service executables
   - Check file integrity of service binaries
   - Review service executable locations and permissions
   - Test for DLL hijacking vulnerabilities in service paths
   - Assess service configuration tampering possibilities
6. Test service access controls:
   - Review service permissions and access control lists
   - Test unauthorized service creation capabilities
   - Verify service modification permissions
   - Assess service deletion restrictions
   - Test for privilege escalation through service misconfigurations
7. Evaluate service dependencies and interactions:
   - Identify service dependencies on malicious services
   - Review service recovery actions and failure handling
   - Test for service injection or hijacking opportunities
   - Assess inter-service communication and trust
8. Assess detection and monitoring:
   - Verify logging of service installation and modification
   - Test alerting on new service creation
   - Assess SIEM integration of service control logs
   - Verify EDR detection of malicious service installation
   - Test incident response procedures for service-based persistence

## Expected Secure Configuration

Organizations managing system services should implement:

- Comprehensive inventory of authorized services
- Regular review and validation of all running services
- Least privilege service accounts (not SYSTEM or root)
- Service binary integrity monitoring and verification
- Access controls preventing unauthorized service creation
- Code signing requirements for service executables
- Logging of all service lifecycle events
- Network segmentation for service communications
- Regular cleanup of obsolete services
- Incident response procedures for malicious services

## Evidence to Collect

- Complete enumeration of Windows services and Linux daemons
- Service configuration analysis including startup type and privileges
- Identification of suspicious or unauthorized services
- Service executable hashes and digital signature verification
- Service installation and modification timestamps
- Access control analysis on service management
- Detection capability validation for malicious services
- Network connection analysis from services
- Service execution logs and audit trails
- Screenshots or exports of suspicious services

## Impact

**Privileged Persistent Access Impact:**
Malicious services execute automatically at startup with SYSTEM or root privileges providing highest-level persistent access. Service-based persistence survives reboots maintaining continuous attacker control. Elevated service privileges enable complete system compromise including credential dumping, file access, and system modification. Organizations face ongoing privileged access despite password changes and security measures.

**Stealth and Evasion Impact:**
Services operating as background processes evade casual detection. Malicious services masquerading as legitimate system services blend with normal operations. Service execution lacks user interaction hiding attacker activities. Long-running service processes accumulate extensive system access over time. Organizations remain unaware of compromise when malicious services operate covertly.

**System Stability and Availability Impact:**
Poorly implemented malicious services cause system crashes and instability. Service conflicts and resource consumption degrade performance. Service recovery actions trigger repeated malicious execution. Organizations face operational disruption from unstable persistence mechanisms. Critical system failures may occur from service-based attacks.

## MITRE ATT&CK Mapping

- **T1543.003** - Persistence: Create or Modify System Process: Windows Service
- **T1543.002** - Persistence: Create or Modify System Process: Systemd Service
- **T1574.011** - Persistence: Hijack Execution Flow: Services Registry Permissions Weakness
- **T1569.002** - Execution: System Services: Service Execution

## Remediation Guidance

Organizations should implement the following service security practices:

1. **Service Inventory and Governance:**
   - Maintain comprehensive inventory of authorized services
   - Document business purpose for each service
   - Implement change management for service installation
   - Conduct quarterly service review and validation
   - Remove unnecessary or obsolete services
   - Track service ownership and approval

2. **Service Access Control:**
   - Restrict service creation to authorized administrators
   - Implement least privilege for service accounts
   - Use dedicated managed service accounts (gMSA on Windows)
   - Remove SYSTEM and LocalSystem usage where possible
   - Apply proper permissions on service registry keys
   - Implement privileged access management (PAM) for service administration

3. **Service Binary Security:**
   - Require code signing for all service executables
   - Deploy application whitelisting for service binaries
   - Implement file integrity monitoring on service executables
   - Store service binaries in protected directories only
   - Verify digital signatures before service installation
   - Conduct malware scanning of service executables

4. **Service Configuration Hardening:**
   - Configure services with minimum required privileges
   - Disable interactive desktop access for services
   - Implement service isolation and sandboxing
   - Use restrictive service access tokens
   - Configure service failure actions securely
   - Disable service recovery for untrusted services

5. **Monitoring and Detection:**
   - Enable comprehensive logging of service events
   - Monitor Event ID 7045 (Windows service installation)
   - Alert on new service creation in real-time
   - Deploy behavioral analytics detecting malicious services
   - Monitor service network connections
   - Integrate service logs with SIEM for correlation

6. **Platform-Specific Hardening:**
   
   **Windows:**
   - Enable Service Control Manager (SCM) auditing
   - Monitor registry keys under `HKLM\SYSTEM\CurrentControlSet\Services`
   - Deploy Sysmon monitoring service installation APIs
   - Implement GPO restrictions on service creation
   - Use Windows Defender Application Control (WDAC)
   
   **Linux:**
   - Restrict systemctl and service management permissions
   - Set proper ownership on systemd unit files (root:root 0644)
   - Enable systemd-journald comprehensive logging
   - Implement SELinux or AppArmor policies for services
   - Monitor `/etc/systemd/system/` for unauthorized units
   - Use systemd service hardening options (ProtectSystem, PrivateTmp, etc.)
