# INF-05.08 — Detection Evasion Techniques (Log Tampering, Rootkits)

## Summary

This test assesses attacker capabilities to evade detection through log manipulation, rootkit installation, anti-forensics techniques, and other methods designed to conceal malicious activities from security monitoring and investigation.

## Risk

Detection evasion techniques enable attackers to operate without triggering security alerts or leaving forensic evidence. Log tampering destroys evidence of malicious activities preventing incident detection and investigation. Rootkits provide kernel-level stealth hiding processes, files, and network connections. Anti-forensics techniques eliminate artifacts hindering post-incident analysis. Organizations face extended compromise when evasion techniques prevent detection enabling attackers to achieve objectives before discovery.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Administrative or root access to systems exists for testing
- Understanding of logging and monitoring infrastructure
- Testing includes detection evasion assessment

## Test Objectives

- Test log tampering and manipulation capabilities
- Assess rootkit installation and effectiveness
- Evaluate anti-forensics technique viability
- Test detection system evasion and bypass
- Validate security monitoring resilience against evasion

## Test Methodology

1. Test log tampering and manipulation:
   - **Windows Event Log clearing:** Clear Security, System, Application logs
   - **Linux log deletion:** Remove or truncate syslog, auth.log, audit logs
   - **Selective log modification:** Edit specific entries removing evidence
   - **Timestamp manipulation:** Modify log timestamps obscuring timelines
   - Test log forwarding interruption preventing centralized logging
2. Assess rootkit deployment and stealth:
   - **User-mode rootkits:** Test process and DLL injection hiding
   - **Kernel-mode rootkits:** Deploy kernel drivers hiding system objects
   - **Bootkit installation:** Test boot sector rootkits for maximum persistence
   - Verify process hiding from task managers and monitoring
   - Test file and directory cloaking capabilities
3. Test anti-forensics techniques:
   - **Timestomp:** Modify file timestamps (MACE attributes)
   - **Secure deletion:** Overwrite files preventing recovery
   - **Memory-only operation:** Avoid disk artifacts through in-memory execution
   - **Artifact wiping:** Delete temporary files, prefetch, recent documents
   - Test browser history and artifact clearing
4. Evaluate living-off-the-land (LOLBins) evasion:
   - Use legitimate Windows binaries for malicious purposes
   - Execute attacks through PowerShell, WMI, built-in tools
   - Avoid custom malware requiring signature-based detection
   - Test behavioral detection against native tool abuse
   - Verify EDR evasion through legitimate process use
5. Test security tool detection and bypass:
   - Identify antivirus and EDR products deployed
   - Test signature-based detection bypass through obfuscation
   - Assess behavioral detection evasion
   - Test EDR disabling or unhooking techniques
   - Verify sandbox and detonation evasion
6. Assess network detection evasion:
   - Test encryption and protocol tunneling hiding traffic
   - Verify DNS tunneling bypassing network monitoring
   - Test slow-and-low communication avoiding rate limits
   - Assess domain fronting and CDN abuse
   - Test traffic blending with legitimate patterns
7. Test command and control evasion:
   - Verify beaconing interval randomization
   - Test jitter in communication timing
   - Assess traffic shaping mimicking legitimate applications
   - Test protocol impersonation (HTTPS, DNS)
   - Verify geo-blocking bypass techniques
8. Evaluate detection resilience and validation:
   - Test whether log tampering triggers alerts
   - Verify rootkit detection by security tools
   - Assess forensic capability against anti-forensics
   - Test behavior analytics detecting evasion attempts
   - Verify incident response effectiveness against evasion

## Expected Secure Configuration

Organizations preventing detection evasion should implement:

- Immutable or write-once logging preventing tampering
- Centralized log forwarding with real-time transmission
- Log integrity monitoring detecting modifications
- EDR solutions detecting rootkits and anti-forensics
- Memory analysis capabilities for rootkit detection
- Behavioral analytics detecting evasion attempts
- Network monitoring resilient to encryption and tunneling
- Regular security tool effectiveness validation
- Incident response capabilities for sophisticated evasion
- Assume breach mentality not relying solely on detection

## Evidence to Collect

- Log tampering testing results showing manipulation success
- Rootkit deployment and detection evasion validation
- Anti-forensics technique effectiveness assessment
- Living-off-the-land evasion testing results
- Security tool bypass validation
- Network detection evasion testing outcomes
- Detection system resilience assessment
- Forensic analysis capability validation
- Evasion technique detection verification
- Recommendations for detection improvement

## Impact

**Compromised Incident Detection Impact:**
Effective detection evasion prevents security operations from identifying ongoing attacks. Log tampering eliminates evidence of malicious activities. Rootkits hide attacker presence from monitoring tools. Organizations remain unaware of compromise enabling attackers to achieve objectives undetected. Extended dwell time results from evasion preventing detection.

**Forensic Investigation Failure Impact:**
Anti-forensics techniques destroy evidence hindering incident investigation. Investigators cannot reconstruct attack timelines without logs. Rootkits conceal artifacts preventing forensic analysis. Organizations struggle to determine compromise scope without forensic evidence. Legal and regulatory requirements for breach notification fail without adequate investigation.

**Security Tool Ineffectiveness Impact:**
Detection system bypass renders security investments ineffective. Signature-based detection fails against obfuscated malware. Behavioral analytics bypassed through LOLBins and legitimate tool abuse. Network monitoring evaded through encryption and tunneling. Organizations face false sense of security when tools fail against sophisticated evasion.

## MITRE ATT&CK Mapping

- **T1070.001** - Defense Evasion: Indicator Removal: Clear Windows Event Logs
- **T1070.002** - Defense Evasion: Indicator Removal: Clear Linux or Mac System Logs
- **T1070.004** - Defense Evasion: Indicator Removal: File Deletion
- **T1070.006** - Defense Evasion: Indicator Removal: Timestomp
- **T1562.001** - Defense Evasion: Impair Defenses: Disable or Modify Tools
- **T1014** - Defense Evasion: Rootkit
- **T1027** - Defense Evasion: Obfuscated Files or Information

## Remediation Guidance

Organizations should implement the following detection evasion prevention practices:

1. **Log Protection and Integrity:**
   - Deploy immutable or write-once log storage (WORM)
   - Implement real-time log forwarding to centralized SIEM
   - Enable log integrity monitoring detecting modifications
   - Use log signing and cryptographic verification
   - Deploy secondary logging capturing security log modifications
   - Implement append-only log storage preventing deletion

2. **Rootkit Detection and Prevention:**
   - Deploy memory analysis and rootkit detection tools
   - Enable kernel-mode driver signing enforcement
   - Implement Secure Boot preventing bootkit installation
   - Deploy Endpoint Detection and Response (EDR) with memory scanning
   - Regular rootkit scanning with multiple tools
   - Enable Windows Defender System Guard and HVCI

3. **Comprehensive Monitoring:**
   - Deploy behavioral analytics detecting evasion techniques
   - Implement file integrity monitoring (FIM) on critical files
   - Monitor for timestamp manipulation and anomalies
   - Enable Windows Audit Policy comprehensively
   - Deploy Sysmon with extensive configuration
   - Implement network behavior analysis

4. **Anti-Anti-Forensics:**
   - Deploy forensic readiness programs
   - Implement continuous forensic data collection
   - Enable Volume Shadow Copy for file recovery
   - Deploy forensic imaging capabilities
   - Maintain forensic investigation expertise
   - Regular forensic tool validation

5. **Security Tool Protection:**
   - Protect EDR and antivirus from tampering
   - Deploy tamper protection on security agents
   - Monitor for security tool disabling attempts
   - Implement redundant security layers
   - Regular security tool effectiveness validation
   - Deploy deception technology detecting evasion attempts

6. **Detection Resilience:**
   - Conduct regular red team exercises testing evasion
   - Validate detection against MITRE ATT&CK techniques
   - Implement assume breach mentality
   - Deploy honeypots and honey credentials
   - Enable threat hunting detecting subtle evasion
   - Continuous improvement based on evasion attempts
