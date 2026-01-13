# INF-02.08 — Anonymous or Guest Access Testing

## Summary

This test identifies systems and services allowing unauthenticated access through anonymous accounts, guest accounts, null sessions, or similar mechanisms that bypass authentication requirements.

## Risk

Anonymous and guest access enables unauthorized access to resources without credential requirements, bypassing authentication controls entirely. Unauthenticated access to file shares exposes sensitive documents and data to any network-connected entity. Anonymous LDAP queries reveal organizational structure, user accounts, and system information. Null session connections to Windows systems enable complete enumeration of users, groups, shares, and policies. Guest accounts with excessive permissions provide backdoor access routes. Organizations face data breaches, reconnaissance, and unauthorized access when anonymous authentication remains enabled.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.01 through INF-01.03)
- Unauthenticated network access to in-scope systems
- Testing includes anonymous access evaluation

## Test Objectives

- Identify systems and services permitting anonymous access
- Enumerate resources accessible without authentication
- Test for null session connections to Windows systems
- Assess anonymous LDAP bind capabilities
- Evaluate guest account status and permissions

## Test Methodology

1. Test for Windows null session access:
   - Attempt null session connection to SMB services (`net use \\target\IPC$ "" /user:""`)
   - Enumerate users through null session using tools (enum4linux, rpcclient)
   - List shares accessible via null session
   - Query security policies through null session
   - Enumerate groups and group memberships anonymously
2. Test anonymous SMB/CIFS share access:
   - Enumerate network shares on discovered hosts
   - Attempt anonymous connection to identified shares
   - List files and directories on anonymously accessible shares
   - Test read and write permissions on anonymous shares
   - Identify sensitive data exposed through anonymous shares
3. Test anonymous LDAP access:
   - Attempt anonymous LDAP bind to directory servers
   - Enumerate directory structure without authentication
   - Query user objects, organizational units, and attributes
   - Test whether sensitive LDAP attributes are readable anonymously
   - Assess anonymous access to LDAP schema information
4. Test anonymous FTP access:
   - Attempt anonymous FTP login (username: anonymous, password: any email)
   - List directories and files via anonymous FTP
   - Test download and upload capabilities
   - Identify sensitive data accessible via anonymous FTP
5. Test anonymous web server access:
   - Identify directory browsing enabled without authentication
   - Test for anonymous access to administrative interfaces
   - Enumerate files and directories through web server
   - Test anonymous API endpoints without authentication
6. Enumerate guest account status:
   - Verify Windows guest account enabled/disabled status
   - Test guest account authentication capabilities
   - Assess guest account permissions and group memberships
   - Identify applications using guest accounts
7. Test anonymous database access:
   - Attempt connection to databases without credentials
   - Test for default or blank database passwords
   - Enumerate database schemas and tables anonymously
   - Assess data accessibility without authentication
8. Test anonymous access to network services:
   - SNMP with default community strings (public, private)
   - Anonymous SMTP relay testing
   - DNS zone transfers without authentication
   - Anonymous NFS share mounting
   - Unauthenticated VNC or RDP connections

## Expected Secure Configuration

Organizations securing authentication should implement:

- Windows null sessions disabled via RestrictAnonymous registry settings
- SMB/CIFS shares requiring authenticated access
- Guest accounts disabled on all Windows systems
- Anonymous LDAP binds disabled or heavily restricted
- FTP anonymous access disabled unless specifically required for public file distribution
- Web servers configured without directory browsing
- Databases requiring authentication for all connections
- SNMP v3 with authentication replacing SNMPv1/v2c
- Default-deny access policies requiring explicit authentication
- Regular auditing of anonymous access paths

## Evidence to Collect

- Null session enumeration results showing users, groups, and shares
- List of SMB/CIFS shares accessible anonymously with contents
- Anonymous LDAP bind test results and enumerated directory information
- Anonymous FTP access results showing accessible files
- Guest account status on Windows systems
- Web server directory browsing findings
- Anonymous database connection testing results
- SNMP community string enumeration results
- Documentation of services allowing unauthenticated access
- Sensitive data identified through anonymous access

## Impact

**Unauthenticated Information Disclosure Impact:**
Null session access reveals complete user and group inventory without authentication. Anonymous LDAP queries expose organizational structure, email addresses, and user attributes. Anonymous share access discloses sensitive documents, credentials, and intellectual property. Attackers conduct comprehensive reconnaissance without valid accounts.

**Unauthorized Data Access Impact:**
Anonymous file share access enables data exfiltration without authentication. Guest accounts with read permissions provide unrestricted data access. Anonymous database connections expose business data and customer information. Unauthenticated API access reveals sensitive application data.

**Attack Surface Expansion Impact:**
Anonymous access provides entry point bypassing authentication entirely. Every service allowing unauthenticated access represents potential initial access vector. Anonymous access enables attacks from Internet or untrusted networks. Attackers leverage anonymous access for internal reconnaissance and lateral movement.

## MITRE ATT&CK Mapping

- **T1087** - Discovery: Account Discovery (via anonymous enumeration)
- **T1135** - Discovery: Network Share Discovery (anonymous share enumeration)
- **T1069** - Discovery: Permission Groups Discovery (null session group enumeration)
- **T1082** - Discovery: System Information Discovery (anonymous system queries)
- **T1021.002** - Lateral Movement: Remote Services: SMB/Windows Admin Shares

## Remediation Guidance

Organizations should implement the following anonymous access restriction practices:

1. **Windows Null Session Elimination:**
   - Configure RestrictAnonymous registry key to level 2 (highest restriction)
   - Disable anonymous SID enumeration via Group Policy
   - Configure "Network access: Do not allow anonymous enumeration of SAM accounts and shares"
   - Enable "Network access: Let Everyone permissions apply to anonymous users" = Disabled
   - Restrict anonymous access to named pipes and shares
   - Regular testing validating null session restrictions

2. **SMB/CIFS Share Hardening:**
   - Remove Everyone group from share permissions
   - Grant share access to authenticated users only
   - Eliminate default administrative shares if not required ($C, $ADMIN, $IPC)
   - Deploy share access auditing and monitoring
   - Regular share permission reviews identifying anonymous access
   - Document legitimate anonymous shares with business justification

3. **Guest Account Management:**
   - Disable guest accounts on all Windows systems via Group Policy
   - Rename guest accounts to non-standard names if retention required
   - Remove guest accounts from all groups except Guests
   - Monitor for guest account re-enablement
   - Audit applications or services requiring guest accounts

4. **LDAP Anonymous Bind Restriction:**
   - Disable anonymous LDAP binds on directory servers
   - Configure "dsHeuristics" attribute restricting anonymous access
   - Implement LDAP signing and channel binding
   - Require authentication for all LDAP queries
   - Monitor for anonymous LDAP bind attempts

5. **Service-Specific Anonymous Access Control:**
   - Disable anonymous FTP unless required for public file distribution
   - Configure web servers preventing directory browsing
   - Require authentication for all database connections
   - Migrate from SNMPv1/v2c to SNMPv3 with authentication
   - Disable SMTP open relay and require authentication
   - Implement authentication on all network services

6. **Default-Deny Access Architecture:**
   - Implement principle of least privilege requiring explicit authentication
   - Remove default anonymous access permissions
   - Deploy network access control (NAC) requiring authentication
   - Configure firewalls and network devices with authenticated management only
   - Regular scanning for anonymous access paths and immediate remediation
