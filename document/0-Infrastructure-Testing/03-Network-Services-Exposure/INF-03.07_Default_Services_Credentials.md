# INF-03.07 — Misconfigured Default Services or Credentials

## Summary

This test identifies systems and services operating with vendor default configurations, default credentials, or well-known passwords that enable unauthorized access without exploitation or password cracking.

## Risk

Default credentials provide immediate administrative access to systems and applications without requiring technical exploitation. Vendor-supplied default passwords are publicly documented enabling trivial unauthorized access. Systems deployed with default configurations often expose unnecessary services, weak security settings, and administrative interfaces. Default credentials persist across organizations enabling mass compromises when attackers discover single default-configured system type. Unmodified default services run with insecure settings, excessive privileges, and known vulnerabilities. Organizations face complete system compromise when default credentials remain unchanged and default configurations persist.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Service discovery has been completed (see INF-01.03)
- Default credential databases and documentation available
- Testing includes default configuration assessment

## Test Objectives

- Identify systems and services using default credentials
- Detect unmodified vendor default configurations
- Test for common default password patterns
- Assess default service exposure and security posture
- Validate credential change enforcement policies

## Test Methodology

1. Enumerate common default credentials on discovered services:
   - **Administrative Web Interfaces:** admin/admin, admin/password, administrator/administrator
   - **Databases:** sa/[blank], root/[blank], postgres/postgres, admin/admin
   - **Network Devices:** admin/admin, cisco/cisco, admin/[blank], root/[blank]
   - **Application Defaults:** administrator/password, admin/123456, admin/changeme
   - **Cloud Platforms:** administrator/Password123, admin/Admin123
2. Test vendor-specific default credentials:
   - **Cisco:** admin/cisco, cisco/cisco, admin/[blank]
   - **Juniper:** root/[blank], admin/admin
   - **Dell:** root/calvin, admin/admin
   - **HP:** admin/admin, Administrator/[blank]
   - **VMware:** root/vmware, administrator/Password123
   - **Microsoft:** Administrator/[blank], sa/[blank]
3. Identify default service accounts:
   - Database service accounts (sa, postgres, mysql)
   - Application service accounts with default passwords
   - Windows service accounts (ASPNET, IUSR)
   - SNMP community strings (public, private)
   - Backup software default accounts
4. Test for default configuration weaknesses:
   - Unnecessary default services enabled
   - Default administrative shares (C$, ADMIN$, IPC$)
   - Default web server pages and sample applications
   - Default SSL/TLS certificates with vendor names
   - Default encryption keys or shared secrets
5. Enumerate devices with factory default settings:
   - Network devices (routers, switches, firewalls)
   - IoT devices (IP cameras, access control systems, building management)
   - Printers and multifunction devices
   - Embedded systems and appliances
   - Out-of-band management (IPMI, BMC, iLO, iDRAC)
6. Test common password patterns:
   - Variations of "admin", "password", "default", "changeme"
   - Product name as password (e.g., product named "Widget", password "widget")
   - Company name as password
   - Sequential patterns (123456, password123)
   - Empty/blank passwords
7. Assess default configuration hardening:
   - Verify manufacturer default accounts are disabled or removed
   - Test whether default passwords can be set during deployment
   - Assess whether systems force password change on first login
   - Verify default configurations are modified during deployment
   - Test for default configuration detection and alerting
8. Cross-reference against public default credential databases:
   - CIRT.net Default Password Database
   - RouterPasswords.com
   - DefaultPassword.us
   - Vendor documentation and installation guides
   - Public exploit databases containing default credentials

## Expected Secure Configuration

Organizations deploying systems should implement:

- Mandatory unique password requirement during system deployment
- Automated detection of default credentials during provisioning
- Force password change on first login for all systems
- Disabling or removal of vendor default accounts
- Security baseline configurations applied during deployment
- Regular scanning for default credentials across infrastructure
- Network segmentation limiting exposure of newly deployed systems
- Change management process validating security hardening
- Automated configuration management enforcing secure baselines
- Monitoring and alerting on default credential usage attempts

## Evidence to Collect

- Complete inventory of systems tested for default credentials
- Successful default credential authentication results
- List of systems using vendor default configurations
- Identification of default service accounts and passwords
- Default configuration weakness documentation
- Screenshots or authentication logs showing default credential access
- Vendor-specific default credential testing matrix
- Configuration assessment showing unmodified default settings
- IoT and embedded device default credential status
- Remediation timeline for identified default credentials

## Impact

**Immediate Administrative Access Impact:**
Default credentials provide instant administrative access to systems without exploitation or technical skill. Attackers enumerate systems and attempt known default credentials achieving rapid unauthorized access. Single default-configured system type enables mass compromise across organizations. Internet-facing systems with default credentials are discovered and compromised within hours of deployment.

**Automated Attack Success Impact:**
Botnets and automated scanners test default credentials across IP ranges. Worms and malware target systems with known default passwords. Attackers chain default credential access across multiple systems for lateral movement. Default credentials on Internet-facing devices enable entry into internal networks.

**Reputation and Compliance Impact:**
Default credential compromises demonstrate negligent security practices. Regulatory frameworks require changing default credentials and hardening configurations. Audit failures occur when default settings persist. Customer trust erodes when breaches result from default password failures. Organizations face penalties for inadequate security due diligence.

## MITRE ATT&CK Mapping

- **T1078** - Valid Accounts (default credential abuse)
- **T1110.001** - Credential Access: Brute Force: Password Guessing
- **T1552.001** - Credential Access: Unsecured Credentials: Credentials In Files

## Remediation Guidance

Organizations should implement the following default credential elimination practices:

1. **Deployment Security Requirements:**
   - Require unique password entry during all system deployments
   - Implement automated tools blocking default credential deployment
   - Force mandatory password change on first login
   - Deploy security baselines automatically during provisioning
   - Include security hardening in deployment checklists
   - Validate configuration hardening before production transition

2. **Default Account Management:**
   - Disable or remove all vendor default accounts
   - Rename default administrative accounts where removal not possible
   - Create unique administrative accounts per system
   - Document justification for any retained default accounts
   - Implement least privilege on retained default accounts
   - Regular audit of default account status

3. **Automated Detection and Remediation:**
   - Deploy automated scanning for default credentials weekly
   - Implement configuration management tools enforcing secure baselines
   - Use vulnerability scanners with default credential checks
   - Deploy network access control (NAC) blocking default-configured devices
   - Implement automated password rotation for service accounts
   - Alert security teams on default credential detection

4. **IoT and Embedded Device Security:**
   - Maintain inventory of IoT devices with default credential status
   - Segment IoT devices into restricted VLANs
   - Change default credentials on all IoT devices during deployment
   - Replace IoT devices unable to change default credentials
   - Implement device certificates instead of default passwords where possible
   - Regular scanning of IoT device networks for default credentials

5. **Configuration Hardening Standards:**
   - Develop and maintain security baseline configurations
   - Disable unnecessary default services during deployment
   - Remove default administrative shares where not required
   - Delete default sample applications and web pages
   - Replace default SSL/TLS certificates with organizational certificates
   - Apply vendor security hardening guides during deployment

6. **Change Management Integration:**
   - Include security hardening validation in change approval
   - Require security baseline application before production
   - Document configuration changes from vendor defaults
   - Conduct security review of new system deployments
   - Implement automated compliance checking for security baselines
   - Quarterly review of systems against security baseline configurations
