# INF-06.06 — Detection of Authentication Abuse or Brute Force

## Summary

This test evaluates the detection capabilities for authentication attacks including brute force, password spraying, credential stuffing, and other authentication abuse techniques targeting user accounts.

## Risk

Undetected authentication attacks enable unauthorized access through credential guessing and password cracking. Brute force attacks against exposed services eventually succeed without detection and blocking. Password spraying avoids account lockouts while testing common passwords across many accounts. Credential stuffing leverages breached passwords from other services. Organizations face account compromise when authentication abuse operates undetected enabling unauthorized access to systems and data.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication systems and services identified
- Access to authentication logs and monitoring
- Testing includes authentication security assessment

## Test Objectives

- Test detection of brute force authentication attacks
- Assess password spraying detection capabilities
- Evaluate credential stuffing detection
- Test account lockout and blocking mechanisms
- Validate authentication abuse alerting

## Test Methodology

1. Test brute force attack detection:
   - Conduct rapid failed authentication attempts against single account
   - Verify detection of failed login patterns
   - Test account lockout triggering and timing
   - Assess alerting on brute force attempts
   - Verify blocking or throttling implementation
2. Assess password spraying detection:
   - Test single password across multiple accounts
   - Verify detection of distributed authentication attempts
   - Test detection when staying below lockout thresholds
   - Assess behavioral analytics detecting spraying patterns
   - Verify alerting on password spraying indicators
3. Test credential stuffing detection:
   - Use breached credentials from public dumps
   - Verify detection of credential reuse attempts
   - Test detection of automated authentication tools
   - Assess password breach monitoring integration
   - Verify blocking of compromised credentials
4. Evaluate authentication rate limiting:
   - Test authentication attempt throttling
   - Verify per-account rate limiting
   - Assess per-source-IP rate limiting
   - Test CAPTCHA or challenge-response triggering
   - Verify temporary blocking after threshold
5. Test account lockout mechanisms:
   - Verify lockout threshold configuration
   - Test lockout duration and reset
   - Assess lockout on service accounts
   - Verify administrator notification on lockouts
   - Test lockout bypass attempts
6. Assess geographic and contextual detection:
   - Test detection of authentication from unusual locations
   - Verify impossible travel detection
   - Assess device fingerprinting and tracking
   - Test detection of authentication from suspicious IPs
   - Verify threat intelligence integration
7. Test multi-factor authentication (MFA) abuse:
   - Assess detection of MFA fatigue attacks
   - Verify detection of MFA bypass attempts
   - Test monitoring of MFA failures
   - Assess detection of push notification bombing
   - Verify alerting on repeated MFA denials
8. Evaluate monitoring and alerting:
   - Verify logging of authentication attempts
   - Test real-time alerting on attack patterns
   - Assess alert quality and false positive rates
   - Verify integration with SIEM
   - Test automated response and blocking

## Expected Secure Configuration

Organizations detecting authentication abuse should implement:

- Comprehensive logging of all authentication attempts
- Account lockout after failed attempts (5-10 maximum)
- Rate limiting on authentication services
- Detection of password spraying patterns
- Credential breach monitoring and blocking
- Geographic and impossible travel detection
- Multi-factor authentication (MFA) universally
- Real-time alerting on authentication abuse
- Integration with threat intelligence
- Automated blocking of malicious sources

## Evidence to Collect

- Brute force attack detection testing results
- Password spraying detection validation
- Credential stuffing detection assessment
- Rate limiting and throttling verification
- Account lockout mechanism testing
- Geographic detection capability validation
- MFA abuse detection testing
- Authentication logging comprehensiveness
- Alert generation and quality assessment
- Recommendations for detection enhancement

## Impact

**Account Compromise Through Brute Force Impact:**
Undetected brute force attacks eventually succeed compromising accounts. Weak passwords fall to rapid authentication attempts. Internet-facing services suffer continuous brute force attacks. Organizations face unauthorized access when brute force proceeds undetected and unblocked.

**Password Spraying and Credential Stuffing Impact:**
Password spraying tests common passwords across accounts staying below lockout thresholds. Credential stuffing leverages passwords breached from other services. Users reusing passwords across services face compromise. Organizations lose accounts to password-based attacks when detection is inadequate.

**Lateral Movement Through Compromised Accounts Impact:**
Compromised accounts enable authenticated access to systems and data. Attackers use valid credentials appearing as legitimate users. Lateral movement succeeds through compromised accounts. Organizations face expanded compromise when authentication attacks enable initial access.

## MITRE ATT&CK Mapping

- **T1110** - Credential Access: Brute Force
- **T1110.001** - Credential Access: Brute Force: Password Guessing
- **T1110.003** - Credential Access: Brute Force: Password Spraying
- **T1110.004** - Credential Access: Brute Force: Credential Stuffing

## Remediation Guidance

Organizations should implement the following authentication abuse detection practices:

1. **Account Lockout Policies:**
   - Implement account lockout after 5-10 failed attempts
   - Configure 30-minute lockout duration minimum
   - Apply lockout to all accounts including administrators
   - Exempt service accounts from lockout (use separate controls)
   - Monitor and alert on account lockouts
   - Implement administrator notification on lockouts
   - Document lockout policy in security standards

2. **Authentication Rate Limiting:**
   - Implement per-account authentication rate limiting
   - Deploy per-source-IP rate limiting
   - Throttle authentication attempts after threshold
   - Implement progressive delays (increasing wait times)
   - Deploy CAPTCHA challenges after failed attempts
   - Temporarily block sources exceeding thresholds
   - Apply rate limiting to APIs and services

3. **Password Spraying Detection:**
   - Deploy behavioral analytics detecting spraying patterns
   - Monitor failed authentication across multiple accounts
   - Alert on authentication from single source to many accounts
   - Detect time-distributed authentication attempts
   - Implement baseline deviation detection
   - Block sources exhibiting spraying behavior
   - Integrate detection with SIEM for correlation

4. **Credential Breach Monitoring:**
   - Deploy password breach detection services
   - Monitor for compromised credentials in breaches
   - Block known breached passwords at authentication
   - Force password resets for breached credentials
   - Alert users of credential exposure
   - Integrate Have I Been Pwned API or similar
   - Scan password databases against breach lists

5. **Geographic and Contextual Detection:**
   - Implement geographic anomaly detection
   - Deploy impossible travel detection
   - Monitor authentication from unusual locations
   - Integrate threat intelligence for malicious IPs
   - Implement device fingerprinting and tracking
   - Alert on authentication from suspicious sources
   - Deploy adaptive authentication based on risk

6. **Multi-Factor Authentication (MFA):**
   - Require MFA for all user authentication
   - Deploy phishing-resistant MFA (FIDO2, certificates)
   - Monitor for MFA fatigue attacks (repeated prompts)
   - Detect MFA bypass attempts
   - Alert on excessive MFA failures
   - Implement rate limiting on MFA prompts
   - Educate users on MFA security
