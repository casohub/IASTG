# INF-02.02 — Analysis of Credential Types (Passwords, Keys, Tokens)

## Summary

This test analyzes credential types used for authentication across infrastructure, including passwords, cryptographic keys, security tokens, and certificates, to identify weak credential storage, transmission, or management practices.

## Risk

Weak credential types enable trivial authentication compromise through brute-force attacks, password cracking, or credential theft. Hardcoded or embedded credentials in scripts, configuration files, or applications provide persistent backdoor access without password rotation. Unprotected private keys or certificates enable impersonation and unauthorized access. Shared credentials across multiple systems create credential reuse risk where single compromise affects multiple assets. Credentials transmitted or stored without encryption expose authentication secrets to interception and theft.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Authentication mechanisms have been identified (see INF-02.01)
- Access to systems for credential analysis exists
- Testing approach and impact constraints defined (see INF-00.04, INF-00.05)

## Test Objectives

- Identify all credential types in use across infrastructure
- Assess credential strength and cryptographic properties
- Detect hardcoded or embedded credentials
- Evaluate credential storage and protection mechanisms
- Identify credential reuse and sharing patterns

## Test Methodology

1. Enumerate password-based credentials:
   - User account passwords on systems and applications
   - Service account and system account passwords
   - Application database passwords
   - Network device enable passwords and privilege level passwords
   - Password strength and complexity enforcement
2. Identify SSH key-based authentication:
   - SSH private and public key pairs on systems
   - Authorized_keys files showing trust relationships
   - Key formats and cryptographic algorithms (RSA, ECDSA, Ed25519)
   - Key bit lengths and cryptographic strength
   - Passphrase protection on private keys
3. Enumerate API keys and application tokens:
   - Application programming interface (API) keys
   - Service-to-service authentication tokens
   - OAuth access tokens and refresh tokens
   - Personal access tokens (PATs) for development tools
   - Cloud service authentication keys (AWS access keys, Azure service principals)
4. Identify certificate-based credentials:
   - X.509 client certificates for authentication
   - Code signing certificates
   - Certificate private keys and storage locations
   - Certificate validity periods and expiration dates
   - Certificate authority trust chains
5. Test for hardcoded and embedded credentials:
   - Search scripts and automation code for embedded passwords
   - Examine configuration files for cleartext credentials
   - Analyze application binaries for hardcoded authentication secrets
   - Review environment variables containing credentials
   - Check version control repositories for committed secrets
6. Assess credential storage security:
   - Password hash storage algorithms (bcrypt, scrypt, Argon2, MD5, SHA1)
   - Encryption of credentials at rest
   - Operating system credential storage (LSA Secrets, SAM database, /etc/shadow)
   - Application credential stores and encryption
   - Hardware security module (HSM) or key management service (KMS) usage
7. Evaluate credential transmission security:
   - Credentials transmitted over encrypted channels (TLS, SSH)
   - Cleartext credential transmission (HTTP, unencrypted protocols)
   - Credential exposure in URL parameters or logs
   - Token transmission in HTTP headers versus body
8. Identify credential reuse and sharing:
   - Shared service account credentials across multiple systems
   - Administrator credential reuse across domains or forests
   - API key reuse across multiple applications
   - SSH key reuse across multiple systems or users
   - Default vendor credentials still in use

## Expected Secure Configuration

Organizations managing credentials should implement:

- Strong password policies with complexity and length requirements (minimum 14 characters)
- Passwordless authentication where feasible (SSH keys, certificates, FIDO2)
- Cryptographically strong algorithms for password hashing (bcrypt, scrypt, Argon2)
- Unique credentials per system, service, and user
- No hardcoded or embedded credentials in code or configurations
- Secrets management solutions (HashiCorp Vault, Azure Key Vault, AWS Secrets Manager)
- Encryption of credentials in transit and at rest
- Private keys stored with appropriate permissions and encryption
- Regular credential rotation for service accounts and API keys
- Monitoring for credential exposure in logs, repositories, or public sources

## Evidence to Collect

- Inventory of credential types in use across infrastructure
- Password policy documentation and enforcement validation
- SSH key enumeration with algorithms and bit lengths
- API key and token inventory with scope and permissions
- Certificate-based credential identification with validity periods
- Hardcoded credential discoveries in scripts, configs, and code
- Credential storage mechanism analysis with algorithms and encryption
- Credential transmission security assessment
- Credential reuse analysis showing shared credentials across systems
- Password hash extraction results showing algorithm strength

## Impact

**Rapid Credential Compromise Impact:**
Weak password policies enable brute-force attacks succeeding in hours or days. Legacy hash algorithms (MD5, SHA1, DES) allow rapid offline cracking. Hardcoded credentials provide immediate access without need for exploitation. Default vendor credentials grant instant administrative access to systems.

**Persistent Backdoor Access Impact:**
Hardcoded credentials in applications or scripts remain accessible after password changes. Embedded API keys provide long-term access without detection. Shared SSH keys enable multiple access paths difficult to revoke. Service accounts with non-rotating passwords create permanent authentication backdoors.

**Credential Theft and Reuse Impact:**
Cleartext credential transmission enables network interception. Unencrypted private keys stolen from systems enable authentication without passwords. Credential reuse across multiple systems amplifies single compromise into widespread access. API key theft grants access to multiple applications and data sources.

## MITRE ATT&CK Mapping

- **T1552.001** - Credential Access: Unsecured Credentials: Credentials In Files
- **T1552.004** - Credential Access: Unsecured Credentials: Private Keys
- **T1552.005** - Credential Access: Unsecured Credentials: Cloud Instance Metadata API
- **T1555** - Credential Access: Credentials from Password Stores
- **T1110** - Credential Access: Brute Force
- **T1606** - Credential Access: Forge Web Credentials

## Remediation Guidance

Organizations should implement the following credential management practices:

1. **Password Policy Strengthening:**
   - Enforce minimum password length of 14 characters minimum, 16+ preferred
   - Implement password complexity requirements
   - Deploy password breach detection checking against compromised credential databases
   - Eliminate password expiration policies; use continuous monitoring instead
   - Implement account lockout after failed authentication attempts

2. **Passwordless Authentication Migration:**
   - Deploy SSH key-based authentication replacing passwords
   - Implement certificate-based authentication for applications
   - Deploy FIDO2 hardware tokens for user authentication
   - Use certificate-based authentication for machine-to-machine communication
   - Implement Windows Hello for Business or similar modern authentication

3. **Secrets Management Implementation:**
   - Deploy secrets management solution (HashiCorp Vault, CyberArk, etc.)
   - Migrate hardcoded credentials to secrets management system
   - Implement dynamic secrets with short-lived credentials
   - Use secrets management APIs for credential retrieval
   - Eliminate cleartext credentials from configurations and code

4. **Credential Storage Hardening:**
   - Upgrade to modern password hashing algorithms (Argon2, bcrypt, scrypt)
   - Implement hardware security modules (HSM) for key storage
   - Encrypt private keys at rest with passphrase protection
   - Apply appropriate file permissions to credential stores
   - Enable operating system credential guard features

5. **Credential Lifecycle Management:**
   - Implement automated credential rotation for service accounts
   - Rotate API keys and tokens on regular schedule
   - Set expiration dates on certificates and credentials
   - Remove credentials for terminated users and decommissioned systems
   - Implement credential discovery to identify orphaned credentials
   - Monitor public repositories and code sharing platforms for credential exposure
