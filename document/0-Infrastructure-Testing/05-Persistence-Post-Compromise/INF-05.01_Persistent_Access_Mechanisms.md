# INF-05.01 — Persistent Access Mechanisms Identification (Web Shells, Backdoors)

## Summary

This test identifies mechanisms enabling persistent unauthorized access including web shells, backdoors, remote access trojans, and other covert access methods that survive system reboots and credential changes.

## Risk

Persistent access mechanisms enable attackers to maintain long-term unauthorized access to systems and networks despite password changes, system reboots, and incident response efforts. Web shells provide persistent command execution through web applications. Backdoors establish covert remote access channels bypassing authentication. Remote access trojans (RATs) enable complete system control with persistence. Organizations face ongoing compromise when persistent access mechanisms remain undetected enabling continuous data theft, surveillance, and attack staging.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Systems have been compromised or access provided for testing
- File system and process access available
- Testing includes persistence mechanism assessment

## Test Objectives

- Identify web shells and backdoor files on systems
- Detect persistent remote access mechanisms
- Assess covert access channels and backdoors
- Evaluate detection capabilities for persistence
- Validate incident response effectiveness against persistence

## Test Methodology

1. Identify web shell presence on web servers:
   - **PHP Web Shells:** Search for .php files in web directories (c99, r57, b374k, WSO)
   - **ASP/ASPX Web Shells:** Identify .asp/.aspx files with command execution
   - **JSP Web Shells:** Locate .jsp files enabling remote execution
   - Search for obfuscated or encoded web shells
   - Test identified web shells for functionality
2. Detect common web shell indicators:
   - Files with suspicious names (shell.php, cmd.aspx, backdoor.jsp)
   - Recently modified files in web directories
   - Files with unusual permissions or ownership
   - Files containing command execution functions (eval, system, exec, shell_exec)
   - Obfuscated or base64-encoded content in web files
3. Test for backdoor binaries and executables:
   - Search for unauthorized executable files
   - Identify known backdoor signatures (Mimikatz, Cobalt Strike, Meterpreter)
   - Detect packed or obfuscated binaries
   - Verify digital signatures on executables
   - Test network connectivity from suspicious binaries
4. Identify remote access trojan (RAT) installations:
   - Detect known RAT indicators (njRAT, NanoCore, DarkComet, QuasarRAT)
   - Search for RAT configuration files
   - Identify RAT network connections and ports
   - Detect RAT registry keys and persistence mechanisms
   - Test RAT functionality and control channels
5. Assess covert access channels:
   - Identify reverse shells and bind shells
   - Detect SSH backdoors and authorized_keys modifications
   - Search for VNC/RDP backdoor configurations
   - Identify unauthorized remote access software
   - Test covert channel connectivity and functionality
6. Test web application persistence:
   - Identify modified application files for backdoor insertion
   - Detect injected code in legitimate application files
   - Search for unauthorized API endpoints
   - Identify database stored procedures enabling persistence
   - Test application-level backdoors functionality
7. Evaluate detection and removal capabilities:
   - Test antivirus detection of identified persistence
   - Verify EDR alerting on backdoor execution
   - Assess file integrity monitoring effectiveness
   - Test incident response backdoor removal procedures
   - Verify persistence detection in vulnerability scans
8. Assess long-term viability of persistence:
   - Test persistence survival through system reboots
   - Verify persistence survival through credential changes
   - Test persistence detection during incident response
   - Assess stealth and evasion capabilities
   - Evaluate business impact of undetected persistence

## Expected Secure Configuration

Organizations preventing persistent access should implement:

- Web application firewalls (WAF) blocking web shell uploads
- File integrity monitoring (FIM) on web directories and system files
- Regular scanning for web shells and backdoors
- Application whitelisting preventing unauthorized executables
- Endpoint detection and response (EDR) detecting persistence
- Network monitoring identifying command and control traffic
- Regular web directory and system file reviews
- Incident response procedures for backdoor removal
- Secure development practices preventing web shell uploads
- Comprehensive logging and monitoring of file modifications

## Evidence to Collect

- Web shell identification with file paths and contents
- Backdoor binary detection with hashes and signatures
- RAT installation evidence including configuration files
- Network connection logs from persistence mechanisms
- File integrity monitoring alerts on suspicious files
- Detection capability validation for identified persistence
- Persistence survival testing results through reboots
- Incident response effectiveness against backdoors
- Screenshots or samples of identified persistence mechanisms
- Timeline analysis of persistence installation

## Impact

**Long-Term Unauthorized Access Impact:**
Persistent access mechanisms enable attackers to maintain presence despite security measures. Web shells provide persistent command execution on compromised web servers. Backdoors survive password resets and system reboots. Organizations face ongoing data theft, reconnaissance, and attack staging. Incident response efforts fail when persistence mechanisms remain undetected.

**Reinfection and Recovery Impact:**
Undetected persistence enables rapid reinfection after remediation efforts. Attackers regain access through web shells or backdoors immediately after cleanup. Recovery efforts fail when persistence mechanisms survive remediation. Organizations repeat incident response cycles unable to achieve full remediation. Business operations remain impacted by recurring compromise.

**Data Breach and Exfiltration Impact:**
Persistent access enables continuous data exfiltration over extended periods. Attackers leverage backdoors for long-term surveillance and data collection. Web shells facilitate database access and sensitive file retrieval. Organizations suffer massive data breaches spanning months or years through undetected persistence. Regulatory notification requirements and penalties follow extended data exposure.

## MITRE ATT&CK Mapping

- **T1505.003** - Persistence: Server Software Component: Web Shell
- **T1543** - Persistence: Create or Modify System Process
- **T1574** - Persistence: Hijack Execution Flow
- **T1133** - Persistence: External Remote Services
- **T1219** - Command and Control: Remote Access Software

## Remediation Guidance

Organizations should implement the following persistent access prevention practices:

1. **Web Shell Prevention:**
   - Deploy web application firewall (WAF) blocking file uploads
   - Implement input validation preventing malicious file uploads
   - Restrict file upload functionality to authenticated users only
   - Disable dangerous PHP functions (eval, system, exec, shell_exec)
   - Deploy file type validation and content inspection
   - Regular scanning for web shells in web directories

2. **File Integrity Monitoring:**
   - Deploy FIM solutions monitoring critical directories
   - Alert on unauthorized file modifications in web directories
   - Monitor system directories for new executables
   - Implement baseline comparisons detecting unauthorized changes
   - Regular integrity verification of application files
   - Automated alerting on file integrity violations

3. **Application Whitelisting:**
   - Implement application control preventing unauthorized executables
   - Deploy whitelisting solutions (AppLocker, WDAC, Carbon Black)
   - Block execution from user-writable directories
   - Require digital signatures on executable files
   - Implement path-based and hash-based whitelisting
   - Regular review and update of whitelist policies

4. **Endpoint Detection and Response:**
   - Deploy EDR solutions detecting backdoors and RATs
   - Implement behavioral analysis identifying persistence
   - Monitor process execution for suspicious activities
   - Deploy network behavior analysis for C2 detection
   - Automated response blocking persistence mechanisms
   - Regular EDR signature and rule updates

5. **Web Application Security:**
   - Implement secure coding practices preventing vulnerabilities
   - Deploy code review processes identifying backdoor risks
   - Use parameterized queries preventing SQL injection
   - Implement output encoding preventing XSS
   - Regular security testing of web applications
   - Penetration testing including web shell placement attempts

6. **Incident Response and Remediation:**
   - Develop backdoor removal procedures and runbooks
   - Implement forensic analysis identifying all persistence
   - Conduct comprehensive system rebuilds after compromise
   - Verify complete removal through multiple scanning methods
   - Deploy monitoring detecting reinfection attempts
   - Document lessons learned improving future response
