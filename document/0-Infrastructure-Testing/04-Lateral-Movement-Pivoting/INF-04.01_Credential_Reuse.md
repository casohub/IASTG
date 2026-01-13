# INF-04.01 — Credential Reuse Across Systems

## Summary

This test identifies credential reuse patterns across multiple systems where identical or similar passwords enable lateral movement and privilege escalation following initial compromise.

## Risk

Credential reuse multiplies the impact of single password compromise across multiple systems. Local administrator passwords identical across workstations enable mass lateral movement. Service accounts with same credentials on multiple servers provide widespread access. Users reusing passwords across systems create credential stuffing opportunities. Once attackers obtain single set of credentials, reuse enables authentication to numerous additional systems without further exploitation. Organizations face rapid compromise propagation when credential reuse creates authentication shortcuts across infrastructure.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Initial access to at least one system or credentials has been established
- Password hashes or credentials have been obtained
- Testing includes credential reuse analysis

## Test Objectives

- Identify identical credentials used across multiple systems
- Assess local administrator password consistency patterns
- Test credential reuse enabling lateral movement
- Evaluate service account password reuse
- Validate credential uniqueness enforcement

## Test Methodology

1. Assess local administrator password reuse:
   - Extract local administrator password hashes from compromised systems
   - Test local administrator credentials on additional systems
   - Identify systems sharing identical local administrator passwords
   - Verify whether local administrator passwords follow patterns
   - Test LAPS (Local Administrator Password Solution) implementation
2. Analyze service account credential reuse:
   - Enumerate service accounts discovered on systems
   - Test service account credentials across multiple systems
   - Identify service accounts with identical passwords
   - Verify service account credential uniqueness per system
   - Assess managed service account (MSA/gMSA) implementation
3. Test user credential reuse patterns:
   - Identify user accounts present on multiple systems
   - Test obtained user credentials across additional systems
   - Assess whether users reuse passwords across applications
   - Test for credential stuffing opportunities
   - Verify password policy enforcement consistency
4. Evaluate domain credential reuse:
   - Test domain user credentials on non-domain systems
   - Identify local accounts matching domain account passwords
   - Assess whether privileged domain accounts reuse passwords locally
   - Test for password synchronization between domains
5. Assess credential reuse detection:
   - Test whether credential reuse attempts generate alerts
   - Verify monitoring of authentication from multiple systems
   - Assess detection of pass-the-hash attacks
   - Test for anomalous authentication pattern detection
6. Test password pattern analysis:
   - Identify predictable password patterns (Company123, Summer2024)
   - Test for sequential or related passwords across systems
   - Assess whether passwords follow seasons, dates, or increments
   - Verify password complexity prevents pattern prediction
7. Evaluate credential management solutions:
   - Test LAPS implementation randomizing local admin passwords
   - Verify password vault or PAM solution deployment
   - Assess automated password rotation implementation
   - Test for centralized credential management
8. Assess lateral movement through credential reuse:
   - Document lateral movement paths enabled by reused credentials
   - Test for privilege escalation through credential reuse
   - Verify whether credential reuse enables domain-wide access
   - Assess business impact of credential reuse compromise

## Expected Secure Configuration

Organizations managing credentials should implement:

- LAPS (Local Administrator Password Solution) enforcing unique local admin passwords
- Unique service account credentials per system and application
- Managed Service Accounts (MSA/gMSA) with automated password management
- Password uniqueness enforcement preventing reuse across systems
- Centralized credential management with automated rotation
- Monitoring and alerting on credential reuse patterns
- Regular password rotation for administrative and service accounts
- Detection of lateral movement through credential testing
- Password breach monitoring preventing compromised credential reuse
- User education on password uniqueness importance

## Evidence to Collect

- Local administrator password hash analysis showing reuse patterns
- Service account credential reuse identification across systems
- User credential testing results across multiple systems
- LAPS implementation status and coverage
- Lateral movement path documentation enabled by credential reuse
- Credential management solution assessment
- Password pattern analysis showing predictable formats
- Detection and monitoring capability validation for credential reuse
- Business impact assessment of identified credential reuse
- Remediation timeline for credential reuse elimination

## Impact

**Rapid Lateral Movement Impact:**
Credential reuse transforms single system compromise into multi-system access. Identical local administrator passwords enable mass lateral movement across workstations. Attackers authenticate to hundreds or thousands of systems using single credential set. Credential reuse eliminates barriers to lateral movement enabling rapid infrastructure compromise.

**Privilege Escalation and Domain Compromise Impact:**
Service accounts with administrator privileges and reused passwords provide immediate privilege escalation. Reused administrative credentials grant domain-wide access. Single compromised administrative password affects all systems sharing credentials. Organizations face complete domain compromise from single credential theft.

**Persistent Access and Recovery Impact:**
Credential reuse prevents effective incident response and recovery. Changing passwords on compromised systems insufficient when credentials are reused elsewhere. Attackers maintain access through alternate systems sharing credentials. Password resets ineffective without addressing all systems sharing credentials.

## MITRE ATT&CK Mapping

- **T1078** - Valid Accounts (credential reuse for authentication)
- **T1021.002** - Lateral Movement: Remote Services: SMB/Windows Admin Shares
- **T1550.002** - Use Alternate Authentication Material: Pass the Hash
- **T1110.003** - Credential Access: Brute Force: Password Spraying

## Remediation Guidance

Organizations should implement the following credential reuse elimination practices:

1. **LAPS Deployment:**
   - Deploy Microsoft LAPS across all workstations and servers
   - Configure automated local administrator password rotation (30 days maximum)
   - Implement LAPS with unique passwords per system
   - Store LAPS passwords in Active Directory with appropriate permissions
   - Monitor LAPS password retrieval and usage
   - Regularly audit LAPS implementation and coverage

2. **Service Account Password Management:**
   - Deploy Group Managed Service Accounts (gMSA) eliminating password management
   - Use unique service account credentials per system and application
   - Implement automated service account password rotation
   - Deploy Privileged Access Management (PAM) for service credential vaulting
   - Eliminate hardcoded service account credentials
   - Regular audit of service account password uniqueness

3. **Centralized Credential Management:**
   - Deploy enterprise password vault or PAM solution
   - Implement automated credential rotation and management
   - Use secrets management for application credentials
   - Deploy password synchronization preventing manual reuse
   - Implement just-in-time (JIT) credential provisioning
   - Monitor credential checkout and usage from vault

4. **Password Uniqueness Enforcement:**
   - Implement technical controls preventing password reuse across systems
   - Deploy password breach detection rejecting compromised passwords
   - Enforce password history preventing immediate reuse (24 passwords minimum)
   - Implement cross-system password uniqueness validation
   - Deploy password managers encouraging unique passwords
   - User education on password uniqueness importance

5. **Detection and Monitoring:**
   - Monitor for authentication attempts across multiple systems with same credentials
   - Deploy behavioral analytics detecting lateral movement patterns
   - Alert on pass-the-hash attack indicators
   - Implement credential honeypots detecting unauthorized credential testing
   - Monitor for privilege escalation through credential reuse
   - Integrate authentication logs across systems for correlation

6. **Incident Response Integration:**
   - Include credential reuse identification in incident response procedures
   - Develop runbooks for credential compromise response
   - Implement automated credential rotation following compromise
   - Coordinate password resets across all affected systems
   - Validate credential uniqueness post-incident
   - Conduct post-incident review of credential reuse enabling compromise
