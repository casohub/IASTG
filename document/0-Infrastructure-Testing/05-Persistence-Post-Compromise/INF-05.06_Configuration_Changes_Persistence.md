# INF-05.06 — Configuration Changes for Persistence (Registry, Startup Scripts)

## Summary

This test identifies malicious configuration modifications enabling persistence through registry keys, startup scripts, shell profiles, and system configuration files that execute attacker code automatically.

## Risk

System configuration changes provide stealthy persistence mechanisms difficult to detect without comprehensive monitoring. Windows registry autorun keys execute malicious code at startup or user logon. Startup scripts and logon scripts run automatically with user or system privileges. Shell profile modifications execute attacker commands on every terminal session. Configuration file tampering enables persistent code execution through legitimate system processes. Organizations face long-term compromise when configuration-based persistence survives reboots and security updates.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Administrative or elevated access to systems exists
- Ability to analyze system configurations and registry
- Testing includes persistence mechanism assessment

## Test Objectives

- Identify malicious registry modifications enabling persistence
- Detect startup and logon script tampering
- Assess shell profile and initialization file modifications
- Evaluate configuration file integrity monitoring
- Validate detection capabilities for configuration changes

## Test Methodology

1. Enumerate Windows registry autorun locations:
   - **Run keys:** `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
   - **RunOnce keys:** Similar path with RunOnce instead of Run
   - **Startup folder:** `%ProgramData%\Microsoft\Windows\Start Menu\Programs\Startup`
   - **Logon scripts:** `HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System`
   - **Image File Execution Options:** `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`
2. Test additional Windows registry persistence:
   - **Shell registry keys:** `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell`
   - **AppInit_DLLs:** `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Windows\AppInit_DLLs`
   - **BootExecute:** `HKLM\System\CurrentControlSet\Control\Session Manager\BootExecute`
   - **Service registry modifications:** `HKLM\System\CurrentControlSet\Services`
   - **COM object hijacking:** `HKLM\Software\Classes\CLSID`
3. Analyze startup and logon scripts:
   - **Group Policy scripts:** `%SystemRoot%\SYSVOL\domain\Policies\{GUID}\Machine\Scripts`
   - **User logon scripts:** Configured in Active Directory user properties
   - **Startup folder scripts:** Windows startup directories
   - **Scheduled task script execution:** Scripts triggered at logon
4. Identify Linux shell profile modifications:
   - **System-wide profiles:** `/etc/profile`, `/etc/profile.d/*`
   - **Bash profiles:** `~/.bashrc`, `~/.bash_profile`, `~/.bash_login`
   - **Shell initialization:** `~/.zshrc`, `~/.profile`, `/etc/bashrc`
   - **Environment files:** `/etc/environment`, `~/.pam_environment`
5. Test Linux system configuration persistence:
   - **Init scripts:** `/etc/rc.local`, `/etc/rc.d/*`
   - **Systemd units:** `/etc/systemd/system/*`, `/lib/systemd/system/*`
   - **PAM configuration:** `/etc/pam.d/*`
   - **LD_PRELOAD:** `/etc/ld.so.preload` for library hijacking
   - **Sudoers modifications:** `/etc/sudoers`, `/etc/sudoers.d/*`
6. Assess configuration file tampering:
   - **Web server configs:** Apache, Nginx, IIS configuration files
   - **Application configs:** Modified to load malicious modules
   - **Database configs:** Startup parameters and initialization scripts
   - **SSH configuration:** `/etc/ssh/sshd_config` backdoor modifications
7. Test persistence detection evasion:
   - Verify file integrity monitoring coverage
   - Test for configuration changes evading detection
   - Assess registry monitoring effectiveness
   - Test behavioral analytics detecting config-based persistence
8. Evaluate configuration backup and restoration:
   - Verify configuration backup procedures
   - Test restoration of clean configurations
   - Assess version control for configuration files
   - Verify change management tracking

## Expected Secure Configuration

Organizations protecting system configurations should implement:

- File integrity monitoring (FIM) on critical configuration files
- Registry monitoring and alerting on autorun key modifications
- Change management for system configuration changes
- Regular configuration audits and baseline validation
- Application whitelisting preventing unauthorized startup execution
- Configuration backup and version control
- Least privilege preventing unauthorized configuration changes
- Comprehensive logging of configuration modifications
- Incident response procedures for configuration tampering
- Regular security baselines and compliance validation

## Evidence to Collect

- Registry autorun enumeration with suspicious entries
- Startup and logon script analysis
- Shell profile and initialization file modifications
- Configuration file tampering identification
- File integrity monitoring alert analysis
- Configuration change audit logs
- Persistence mechanism testing and validation
- Detection capability assessment for configuration changes
- Timeline analysis of configuration modifications
- Baseline comparison showing unauthorized changes

## Impact

**Stealthy Persistent Execution Impact:**
Configuration-based persistence operates through legitimate system mechanisms appearing as normal operations. Registry autorun execution blends with legitimate startup programs. Shell profile persistence triggers on every user session. Configuration tampering executes malicious code through trusted system processes. Organizations struggle to distinguish malicious configuration changes from legitimate administration.

**Privilege Escalation Through Configuration Impact:**
Configuration modifications enable privilege escalation through system-level execution. Registry changes execute code with SYSTEM privileges. Sudoers tampering grants unauthorized root access. PAM configuration modification bypasses authentication. Startup script execution provides high-privilege code execution at boot.

**Detection Evasion and Forensic Impact:**
Configuration-based persistence lacks obvious malicious files requiring sophisticated detection. Malicious registry keys appear as single entries among thousands. Shell profile modifications hide within legitimate configuration. Timestamp manipulation obscures configuration change timing. Organizations face extended dwell time when configuration persistence remains undetected.

## MITRE ATT&CK Mapping

- **T1547.001** - Persistence: Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder
- **T1037** - Persistence: Boot or Logon Initialization Scripts
- **T1037.004** - Persistence: Boot or Logon Initialization Scripts: RC Scripts
- **T1574.002** - Persistence: Hijack Execution Flow: DLL Side-Loading
- **T1546.010** - Persistence: Event Triggered Execution: AppInit DLLs

## Remediation Guidance

Organizations should implement the following configuration protection practices:

1. **File Integrity Monitoring (FIM):**
   - Deploy FIM solutions monitoring critical configuration files
   - Monitor Windows registry autorun locations
   - Alert on unauthorized configuration modifications
   - Implement baseline comparisons detecting changes
   - Regular FIM rule updates for new persistence techniques
   - Integrate FIM alerts with SIEM for correlation

2. **Registry Protection (Windows):**
   - Enable registry auditing on autorun keys
   - Monitor Event ID 4657 (registry value modification)
   - Deploy Sysmon monitoring registry persistence locations
   - Implement registry permissions restricting modifications
   - Use Group Policy to control autorun locations
   - Regular automated scanning for suspicious registry entries

3. **Configuration Change Management:**
   - Implement change management for all configuration modifications
   - Require approval for system configuration changes
   - Document business justification for changes
   - Deploy configuration version control
   - Maintain configuration baselines
   - Regular compliance audits against baselines

4. **Startup and Logon Script Controls:**
   - Implement code signing requirements for scripts
   - Restrict script execution to approved locations
   - Monitor startup folder and GPO script modifications
   - Apply least privilege to script directories
   - Regular audit of logon scripts and GPOs
   - Remove unnecessary startup items

5. **Linux Configuration Hardening:**
   - Set appropriate permissions on profile files (644 or 600)
   - Monitor `/etc/profile.d/` for unauthorized scripts
   - Implement AppArmor or SELinux policies
   - Audit sudoers modifications
   - Monitor LD_PRELOAD usage
   - Disable rc.local if not required

6. **Detection and Response:**
   - Deploy EDR solutions detecting configuration persistence
   - Implement behavioral analytics for persistence indicators
   - Monitor for suspicious process ancestry from startup mechanisms
   - Alert on configuration changes outside maintenance windows
   - Conduct regular threat hunting for persistence
   - Develop runbooks for configuration-based persistence removal
