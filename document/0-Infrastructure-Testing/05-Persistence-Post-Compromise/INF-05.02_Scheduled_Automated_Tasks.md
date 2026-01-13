# INF-05.02 — Abuse of Scheduled or Automated Tasks (AT Jobs/Cron)

## Summary

This test identifies unauthorized or malicious scheduled tasks and automated jobs that provide persistence through periodic execution of attacker commands, scripts, or binaries.

## Risk

Scheduled tasks and cron jobs provide legitimate automation but enable persistent command execution when abused by attackers. Malicious scheduled tasks execute attacker payloads at system startup or regular intervals. Cron jobs on Linux systems run attacker scripts with predictable timing. Organizations face persistent compromise when scheduled tasks execute malicious code surviving reboots and evading detection. Task scheduler abuse enables data exfiltration, reconnaissance, lateral movement, and maintaining command and control through automated execution.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Administrative or user-level access to systems exists
- Ability to enumerate scheduled tasks and jobs
- Testing includes persistence mechanism assessment

## Test Objectives

- Enumerate all scheduled tasks and automated jobs
- Identify unauthorized or malicious scheduled executions
- Assess scheduled task security and access controls
- Test detection capabilities for malicious tasks
- Validate monitoring and logging of task execution

## Test Methodology

1. Enumerate Windows scheduled tasks:
   - Query all scheduled tasks using `schtasks /query /fo LIST /v`
   - Review Task Scheduler console for unauthorized tasks
   - Examine task XML files in `C:\Windows\System32\Tasks\`
   - Identify tasks running under SYSTEM or administrator privileges
   - Review task triggers and execution frequency
2. Analyze Linux/Unix cron jobs:
   - Review system-wide cron jobs in `/etc/crontab`
   - Examine cron directories (`/etc/cron.d/`, `/etc/cron.daily/`, `/etc/cron.hourly/`)
   - Check user-specific crontabs (`crontab -l` for each user)
   - Review `/var/spool/cron/` for user cron files
   - Examine anacron jobs in `/etc/anacrontab`
3. Test for malicious scheduled task indicators:
   - Tasks with suspicious names or obfuscated identifiers
   - Tasks executing from temporary or user-writable directories
   - Tasks running PowerShell with encoded commands
   - Tasks executing scripts with suspicious content
   - Recently created tasks without business justification
4. Identify persistence through task scheduling:
   - Tasks triggered at system startup or user logon
   - Tasks running at regular intervals for C2 beaconing
   - Tasks executing reconnaissance or lateral movement tools
   - Tasks performing data exfiltration or staging
   - Tasks maintaining backdoor connectivity
5. Assess scheduled task access controls:
   - Review permissions on task files and directories
   - Test unauthorized task creation capabilities
   - Verify principle of least privilege on task execution
   - Assess task modification permissions
   - Test for task hijacking vulnerabilities
6. Test at and batch job scheduling:
   - Enumerate AT jobs on Windows (`at` command, deprecated but may exist)
   - Review batch job scheduling systems
   - Identify SCCM or enterprise job scheduling misuse
   - Test unauthorized job creation in enterprise schedulers
7. Evaluate systemd timers (Linux):
   - List systemd timer units (`systemctl list-timers`)
   - Review timer unit configurations in `/etc/systemd/system/`
   - Identify unauthorized timer units
   - Test systemd timer permissions and access
8. Assess detection and monitoring:
   - Verify logging of scheduled task creation and modification
   - Test alerting on suspicious task execution
   - Assess SIEM integration of task scheduler logs
   - Verify behavioral analytics detecting malicious tasks
   - Test incident response procedures for task-based persistence

## Expected Secure Configuration

Organizations managing scheduled tasks should implement:

- Comprehensive inventory of authorized scheduled tasks
- Regular review and validation of all scheduled tasks
- Least privilege execution for scheduled tasks
- Monitoring and alerting on task creation and modification
- File integrity monitoring on task scheduler directories
- Access controls preventing unauthorized task creation
- Code signing requirements for scheduled scripts
- Logging of all task execution events
- Regular cleanup of obsolete or unnecessary tasks
- Incident response procedures for malicious tasks

## Evidence to Collect

- Complete enumeration of Windows scheduled tasks
- Comprehensive listing of cron jobs and systemd timers
- Identification of suspicious or unauthorized tasks
- Task configuration files and execution parameters
- Task execution logs and history
- Access control analysis on task scheduling systems
- Detection capability validation for malicious tasks
- Monitoring and logging configuration verification
- Screenshots or exports of suspicious scheduled tasks
- Timeline analysis of task creation and modification

## Impact

**Persistent Command Execution Impact:**
Malicious scheduled tasks execute attacker commands at regular intervals maintaining persistent access. Tasks triggered at startup ensure execution after system reboots. Scheduled PowerShell execution enables sophisticated attack chains. Organizations face ongoing compromise through automated malicious execution despite credential changes or system restarts.

**Privilege Escalation and Lateral Movement Impact:**
Scheduled tasks running under SYSTEM or administrator privileges enable privilege escalation. Attackers create tasks executing with elevated permissions. Task scheduler abuse enables lateral movement through scheduled remote task execution. Service accounts with task scheduling privileges provide cross-system access.

**Data Exfiltration and C2 Impact:**
Scheduled tasks perform periodic data collection and exfiltration. Cron jobs execute scripts staging and transmitting data at predictable intervals. Scheduled beaconing to command and control infrastructure maintains attacker connectivity. Automated reconnaissance tasks gather intelligence for attack progression.

## MITRE ATT&CK Mapping

- **T1053.005** - Persistence: Scheduled Task/Job: Scheduled Task (Windows)
- **T1053.003** - Persistence: Scheduled Task/Job: Cron (Linux)
- **T1053.006** - Persistence: Scheduled Task/Job: Systemd Timers
- **T1053.002** - Persistence: Scheduled Task/Job: At (Linux/Windows)

## Remediation Guidance

Organizations should implement the following scheduled task security practices:

1. **Task Inventory and Governance:**
   - Maintain comprehensive inventory of authorized scheduled tasks
   - Document business justification for each scheduled task
   - Implement change management for task creation and modification
   - Conduct quarterly review of all scheduled tasks
   - Remove obsolete or unnecessary tasks regularly
   - Tag tasks with owner, purpose, and approval information

2. **Access Control and Least Privilege:**
   - Restrict task creation to authorized administrators only
   - Implement least privilege for task execution accounts
   - Use dedicated service accounts for scheduled tasks
   - Remove administrators from task scheduling groups
   - Apply file system permissions preventing task file modification
   - Implement privileged access management (PAM) for task administration

3. **Task Execution Security:**
   - Require code signing for scheduled scripts and executables
   - Execute tasks from protected directories only
   - Implement application whitelisting for task executables
   - Disable script execution from user-writable directories
   - Use absolute paths in task commands preventing hijacking
   - Validate task configurations preventing command injection

4. **Monitoring and Detection:**
   - Enable comprehensive logging of task creation, modification, and execution
   - Deploy real-time monitoring alerting on new task creation
   - Implement behavioral analytics detecting malicious task patterns
   - Monitor for PowerShell execution with encoded commands
   - Alert on tasks executing from suspicious directories
   - Integrate task scheduler logs with SIEM for correlation

5. **Windows-Specific Hardening:**
   - Enable Task Scheduler operational logging
   - Monitor Event ID 4698 (task created), 4699 (task deleted), 4700/4701 (task enabled/disabled)
   - Implement GPO restrictions on scheduled task creation
   - Disable legacy AT command functionality
   - Deploy Sysmon monitoring task scheduler API calls
   - Restrict remote task scheduling permissions

6. **Linux-Specific Hardening:**
   - Restrict crontab access using `/etc/cron.allow` and `/etc/cron.deny`
   - Set appropriate permissions on cron directories (root:root 0700)
   - Enable cron logging in syslog
   - Monitor `/var/log/cron` for suspicious entries
   - Implement SELinux or AppArmor policies for cron
   - Use systemd timers with appropriate permissions and monitoring
