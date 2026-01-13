# INF-02.04 — Password Policy and Enforcement Review

## Summary

This test reviews organizational password policies and validates technical enforcement of password requirements to ensure passwords meet minimum security standards for complexity, length, rotation, and reuse.

## Risk

Weak password policies enable credential guessing attacks, brute-force authentication attempts, and use of compromised credentials. Lack of technical enforcement allows users to bypass password requirements creating security exceptions. Passwords below minimum complexity or length standards fall rapidly to password cracking tools. Excessive password expiration requirements without breach monitoring encourage predictable password patterns and reuse. Inconsistent password policies across systems create variable security posture where attackers target weakest requirements. Organizations face account compromise through weak passwords despite documented policy expectations.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication mechanisms have been identified (see INF-02.01)
- Access to systems for policy review exists
- Testing approach includes password policy analysis

## Test Objectives

- Review documented password policies for security adequacy
- Validate technical enforcement of password requirements
- Test for password policy bypasses or exceptions
- Assess password policy consistency across systems
- Identify gaps between policy documentation and implementation

## Test Methodology

1. Review documented organizational password policies:
   - Minimum password length requirements
   - Password complexity requirements (character classes)
   - Password history and reuse prevention settings
   - Password expiration and maximum age policies
   - Account lockout thresholds and durations
   - Password breach detection integration
2. Extract technical password policies from systems:
   - **Windows Active Directory:** Group Policy password settings, Fine-Grained Password Policies (FGPP)
   - **Linux/Unix:** PAM (Pluggable Authentication Modules) password requirements
   - **Network Devices:** Password configuration on routers, switches, firewalls
   - **Applications:** Application-specific password requirements and enforcement
   - **Cloud Services:** Azure AD, AWS IAM, Google Workspace password policies
3. Analyze password length requirements:
   - Verify minimum password length meets or exceeds 14 characters
   - Identify systems with weak length requirements (<12 characters)
   - Test whether shorter passwords can be set despite policy
   - Assess whether length requirements increase for privileged accounts
4. Evaluate password complexity requirements:
   - Review character class requirements (uppercase, lowercase, numbers, symbols)
   - Assess dictionary word and common pattern prevention
   - Test for excessive complexity causing predictable patterns
   - Verify complexity balances security with usability
5. Test password history and reuse prevention:
   - Verify password history prevents reuse of recent passwords
   - Test whether password history can be bypassed
   - Assess password history depth (recommended 24 passwords)
   - Verify history enforcement across password changes
6. Assess password expiration policies:
   - Review maximum password age settings
   - Identify systems with short expiration (<90 days) encouraging weak practices
   - Evaluate whether expiration policies exist without breach monitoring
   - Test for password expiration exemptions or disabled enforcement
7. Verify account lockout configurations:
   - Test account lockout after failed authentication attempts
   - Verify lockout threshold (recommended 5-10 attempts)
   - Assess lockout duration and reset mechanisms
   - Test for lockout bypass techniques
   - Verify lockout applies to all authentication methods
8. Test for policy enforcement gaps:
   - Attempt to set weak passwords violating stated policy
   - Test password changes via different interfaces (web, CLI, API)
   - Verify policy applies to service accounts and privileged accounts
   - Identify systems or accounts exempt from password policy
   - Test password policy during initial account creation versus changes
9. Assess password breach detection integration:
   - Verify integration with compromised password databases (Have I Been Pwned, etc.)
   - Test whether known compromised passwords are rejected
   - Assess real-time versus periodic breach checking
   - Verify breach detection coverage across all systems

## Expected Secure Configuration

Organizations implementing secure password policies should maintain:

- Minimum password length of 14 characters, 16+ preferred for privileged accounts
- Password complexity preventing dictionary words and common patterns
- Password history preventing reuse of 24 previous passwords
- Elimination of password expiration unless breach monitoring unavailable
- Account lockout after 5-10 failed authentication attempts
- Lockout duration of 15-30 minutes or administrator unlock
- Password breach detection rejecting known compromised credentials
- Consistent policy enforcement across all systems and account types
- No password policy exemptions without documented risk acceptance
- Regular password policy reviews and updates based on threat landscape

## Evidence to Collect

- Documented organizational password policies
- Extracted technical password policy configurations from systems
- Group Policy Object (GPO) exports for Windows environments
- PAM configuration files from Linux systems
- Network device password configuration settings
- Application-specific password policy documentation
- Test results showing password policy enforcement or bypass
- Weak password acceptance testing results
- Account lockout testing and configuration analysis
- Password breach detection implementation status
- Comparison matrix of policy documentation versus technical implementation

## Impact

**Credential Guessing and Brute Force Impact:**
Short passwords (<12 characters) fall to brute-force attacks in hours or days. Lack of complexity enables dictionary attacks using common passwords. Weak account lockout or missing lockout allows unlimited authentication attempts. Attackers gain access through systematic password guessing against weak policies.

**Compromised Credential Reuse Impact:**
Absence of password breach detection allows users to select passwords from data breaches. Attackers test known compromised credentials against authentication systems. Password reuse across sites enables credential stuffing attacks. Stolen credentials from third-party breaches provide immediate access.

**Predictable Password Patterns Impact:**
Excessive password expiration forces predictable patterns (Summer2024!, Fall2024!). Users circumvent history requirements with minimal changes. Password complexity without adequate length creates false security. Predictable patterns enable targeted guessing based on time periods and common substitutions.

## MITRE ATT&CK Mapping

- **T1110.001** - Credential Access: Brute Force: Password Guessing
- **T1110.002** - Credential Access: Brute Force: Password Cracking
- **T1110.003** - Credential Access: Brute Force: Password Spraying
- **T1110.004** - Credential Access: Brute Force: Credential Stuffing

## Remediation Guidance

Organizations should implement the following password policy practices:

1. **Modern Password Length Standards:**
   - Implement minimum 14-character password length for standard users
   - Require minimum 16-character passwords for privileged accounts
   - Increase minimum length requirements over time as computing power grows
   - Consider passphrases (3-4 random words) as alternative to complex passwords
   - Document rationale for password length requirements

2. **Balanced Complexity Requirements:**
   - Require passwords contain multiple character classes
   - Implement dictionary word and common pattern rejection
   - Avoid excessive complexity rules that encourage predictable patterns
   - Focus on length over complexity where practical
   - Ban known weak passwords and common substitution patterns

3. **Eliminate Harmful Expiration Policies:**
   - Remove periodic password expiration requirements
   - Implement continuous password breach monitoring instead
   - Force password change only upon confirmed compromise
   - Implement anomalous authentication detection triggering reviews
   - Educate users that password changes should occur when compromise suspected

4. **Robust Password Breach Detection:**
   - Integrate with password breach databases (Have I Been Pwned API, etc.)
   - Check passwords against known compromised credential lists
   - Implement real-time checking during password creation and changes
   - Deploy periodic scanning of existing passwords against breach databases
   - Force password change for users with breached credentials

5. **Account Lockout and Rate Limiting:**
   - Configure account lockout after 5-10 failed authentication attempts
   - Implement lockout duration of 15-30 minutes or administrator unlock
   - Deploy rate limiting on authentication attempts
   - Implement CAPTCHA or progressive delays after failures
   - Monitor and alert on account lockout patterns indicating attacks
   - Consider risk-based authentication reducing lockout impact for legitimate users

6. **Consistent Policy Enforcement:**
   - Apply identical password policies across all systems
   - Eliminate password policy exemptions without documented justification
   - Extend password policies to service accounts and privileged accounts
   - Validate policy enforcement through regular testing
   - Implement automated policy compliance checking
