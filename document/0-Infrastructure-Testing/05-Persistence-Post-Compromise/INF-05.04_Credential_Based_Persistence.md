# INF-05.04 — Credential-Based Persistence (Cached Credentials, Token Theft)

## Summary

This test identifies credential theft and caching mechanisms that enable persistent authentication without requiring passwords, including cached credentials, stolen tokens, and credential dumping techniques providing long-term access.

## Risk

Credential-based persistence enables attackers to maintain access through stolen authentication materials rather than maintaining executable backdoors. Cached credentials on systems provide persistent authentication after password changes. Stolen Kerberos tickets enable long-term authentication without passwords. NTLM hashes enable pass-the-hash attacks bypassing password requirements. Security tokens and access keys provide persistent cloud and application access. Organizations face ongoing unauthorized access when credential theft enables authentication without detection or password dependencies.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Administrative or privileged access to systems exists
- Ability to analyze credential storage and authentication mechanisms
- Testing includes credential-based persistence assessment

## Test Objectives

- Identify cached credentials enabling persistent authentication
- Test credential dumping and theft capabilities
- Assess token and ticket theft for persistent access
- Evaluate detection capabilities for credential theft
- Validate credential protection mechanisms

## Test Methodology

1. Test Windows cached credential extraction:
   - Dump LSA Secrets containing cached credentials
   - Extract cached domain credentials using Mimikatz or similar tools
   - Retrieve DPAPI master keys and decrypt stored credentials
   - Access Windows Credential Manager stored credentials
   - Test Credential Guard effectiveness preventing credential dumping
2. Assess Kerberos ticket theft and persistence:
   - Extract Kerberos TGTs (Ticket Granting Tickets) from memory
   - Dump Kerberos service tickets for specific services
   - Test Golden Ticket creation using KRBTGT hash
   - Create Silver Tickets for specific service persistence
   - Assess pass-the-ticket attack capabilities
3. Test NTLM hash extraction and reuse:
   - Dump NTLM hashes from SAM database and LSA
   - Extract NTLM hashes from LSASS memory
   - Test pass-the-hash authentication without passwords
   - Assess NTLM relay attack possibilities
   - Verify NTLM hash rotation and lifecycle
4. Identify stored credential exposure:
   - Search for credentials in configuration files
   - Extract browser saved passwords and cookies
   - Locate SSH private keys and certificates
   - Identify API keys and access tokens in files
   - Test credential extraction from application memory
5. Test cloud and API token theft:
   - Extract AWS access keys and session tokens
   - Dump Azure AD tokens and refresh tokens
   - Retrieve Google Cloud service account keys
   - Access stored OAuth tokens and credentials
   - Test SaaS application token persistence
6. Assess credential caching policies:
   - Review Windows cached credential count settings
   - Test Linux credential caching (sssd, cached passwords)
   - Verify credential caching timeout configurations
   - Assess offline authentication capabilities
   - Test cached credential usage after password changes
7. Test authentication token lifetime and rotation:
   - Verify token expiration enforcement
   - Test refresh token abuse for persistent access
   - Assess session token hijacking possibilities
   - Verify token revocation effectiveness
   - Test token reuse across sessions and devices
8. Evaluate credential protection mechanisms:
   - Test Credential Guard effectiveness (Windows)
   - Verify Protected Users security group protections
   - Assess LSA protection configuration
   - Test credential encryption at rest
   - Verify secure credential storage implementations

## Expected Secure Configuration

Organizations protecting credentials should implement:

- Credential Guard enabled on Windows systems
- LSA protection preventing credential dumping
- Minimal credential caching with short timeframes
- Strong encryption for stored credentials
- Token rotation with short expiration times
- Multi-factor authentication preventing credential reuse
- Privileged access workstations (PAWs) protecting admin credentials
- Comprehensive monitoring of credential access
- Regular credential rotation policies
- Incident response procedures for credential compromise

## Evidence to Collect

- Cached credential enumeration and extraction results
- Kerberos ticket theft validation and pass-the-ticket testing
- NTLM hash dumping success and pass-the-hash demonstration
- Stored credential discoveries in files and configurations
- Cloud token and API key extraction results
- Credential caching policy analysis
- Token lifetime and rotation assessment
- Credential protection mechanism testing results
- Detection capability validation for credential theft
- Timeline of credential access and usage

## Impact

**Persistent Unauthorized Authentication Impact:**
Stolen credentials enable persistent authentication without password knowledge. Cached credentials provide access after password changes fail to revoke access. Kerberos Golden Tickets enable domain-wide authentication for extended periods. Pass-the-hash attacks bypass password authentication entirely. Organizations face ongoing unauthorized access through credential abuse despite password rotation policies.

**Privilege Escalation Through Credential Theft Impact:**
Administrative credential theft enables privilege escalation to highest levels. Domain administrator Kerberos tickets provide complete domain control. SYSTEM account token theft enables local administrator access. Cloud service account keys grant administrative cloud access. Stolen credentials multiply attacker capabilities beyond initial compromise scope.

**Detection Evasion Through Valid Credentials Impact:**
Authentication using stolen legitimate credentials appears normal to security monitoring. Pass-the-ticket and pass-the-hash attacks using valid credentials bypass authentication logs. Token-based authentication lacks password entry detection indicators. Organizations struggle to distinguish stolen credential usage from legitimate authentication.

## MITRE ATT&CK Mapping

- **T1003** - Credential Access: OS Credential Dumping
- **T1003.001** - Credential Access: OS Credential Dumping: LSASS Memory
- **T1003.002** - Credential Access: OS Credential Dumping: Security Account Manager
- **T1550.002** - Use Alternate Authentication Material: Pass the Hash
- **T1550.003** - Use Alternate Authentication Material: Pass the Ticket
- **T1558.001** - Steal or Forge Kerberos Tickets: Golden Ticket
- **T1555** - Credential Access: Credentials from Password Stores

## Remediation Guidance

Organizations should implement the following credential protection practices:

1. **Windows Credential Protection:**
   - Enable Credential Guard on all Windows 10/11 and Server 2016+ systems
   - Implement LSA Protection (RunAsPPL) preventing LSASS access
   - Add privileged users to Protected Users security group
   - Disable WDigest authentication preventing cleartext credentials
   - Enable Restricted Admin mode for RDP connections
   - Deploy Remote Credential Guard for RDP sessions

2. **Credential Caching Minimization:**
   - Reduce cached logon count to minimum (GPO: Interactive logon: Number of previous logons to cache)
   - Disable credential caching where offline authentication unnecessary
   - Implement short credential cache timeouts
   - Use network authentication preventing local credential storage
   - Deploy smart cards or certificate-based authentication
   - Eliminate password caching in applications and scripts

3. **Kerberos Ticket Protection:**
   - Implement short Kerberos ticket lifetimes (8 hours maximum)
   - Rotate KRBTGT password twice annually minimum
   - Enable RC4-HMAC-MD5 encryption upgrade to AES
   - Implement Kerberos Armoring (FAST) where supported
   - Monitor for Golden Ticket and Silver Ticket indicators
   - Alert on Kerberos ticket requests from unusual sources

4. **NTLM Hardening and Elimination:**
   - Migrate from NTLM to Kerberos authentication
   - Enable NTLM auditing to identify NTLM usage
   - Configure LmCompatibilityLevel to reject LM and NTLMv1
   - Implement Extended Protection for Authentication
   - Enable SMB signing preventing NTLM relay
   - Block NTLM where Kerberos available

5. **Token and API Key Management:**
   - Implement short-lived tokens with expiration (1 hour maximum for sensitive operations)
   - Deploy token rotation policies
   - Use refresh token rotation preventing reuse
   - Implement token revocation capabilities
   - Monitor token usage patterns detecting theft
   - Store tokens encrypted at rest
   - Eliminate long-lived API keys favoring temporary credentials

6. **Monitoring and Detection:**
   - Enable credential access auditing (Event IDs 4624, 4625, 4768, 4769)
   - Deploy EDR solutions detecting credential dumping
   - Monitor for Mimikatz and credential theft tool execution
   - Alert on suspicious LSASS process access
   - Implement behavioral analytics detecting pass-the-hash
   - Monitor for unusual Kerberos ticket requests
   - Deploy honeypot credentials detecting theft and usage
