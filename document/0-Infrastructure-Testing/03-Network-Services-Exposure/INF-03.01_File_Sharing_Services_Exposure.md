# INF-03.01 — File Sharing Services Exposure (SMB/NFS)

## Summary

This test identifies file sharing services (SMB/CIFS and NFS) exposed across networks, assesses their accessibility, and evaluates the sensitivity of shared data to determine appropriate access controls and segmentation.

## Risk

Exposed file sharing services provide direct access to organizational data including confidential documents, credentials, source code, and intellectual property. Misconfigured SMB shares accessible from untrusted networks enable data exfiltration without exploitation. Weak or missing authentication on file shares allows unauthorized data access. World-readable NFS exports expose sensitive data to any network-connected system. File sharing services vulnerable to exploitation (EternalBlue, SMBv1 vulnerabilities) provide remote code execution enabling system compromise. Organizations face data breaches, ransomware deployment, and intellectual property theft through unsecured file sharing exposure.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Network connectivity to file sharing services
- Testing includes file share enumeration

## Test Objectives

- Identify all SMB/CIFS and NFS file sharing services
- Enumerate accessible shares and their permissions
- Assess sensitive data exposure through file shares
- Evaluate file sharing service versions and vulnerabilities
- Validate network segmentation isolating file services

## Test Methodology

1. Identify SMB/CIFS services across infrastructure:
   - Scan for TCP ports 139, 445 indicating SMB service
   - Enumerate SMB version (SMBv1, SMBv2, SMBv3)
   - Identify Windows file servers, NAS devices, and SAMBA servers
   - Document SMB service locations and network exposure
2. Enumerate accessible SMB shares:
   - List shares on discovered SMB services
   - Attempt anonymous access to shares
   - Test authenticated access with discovered or provided credentials
   - Identify hidden shares (shares ending with $)
   - Document share permissions (read, write, full control)
3. Identify NFS services and exports:
   - Scan for TCP/UDP port 2049 indicating NFS service
   - Query RPC port mapper (port 111) for NFS exports
   - Enumerate NFS exports using showmount or rpcinfo
   - Identify export permissions and access controls
   - Document NFS version (NFSv2, NFSv3, NFSv4)
4. Assess file share accessibility:
   - Test share access from different network segments
   - Verify whether shares are accessible from Internet
   - Identify shares accessible from guest networks
   - Test access from untrusted VLANs
   - Document network-level restrictions on file sharing traffic
5. Analyze shared data sensitivity:
   - Browse accessible shares and enumerate files and directories
   - Identify sensitive data types (credentials, PII, financial data, source code)
   - Search for specific high-value files (passwords.txt, private keys, database backups)
   - Document business-critical or regulated data exposure
   - Assess appropriateness of data location and access controls
6. Test file sharing service vulnerabilities:
   - Identify SMBv1 usage vulnerable to EternalBlue and other attacks
   - Test for SMB signing requirement
   - Assess encryption in transit (SMB encryption)
   - Identify NFS security features (Kerberos authentication, krb5p encryption)
   - Test for known file sharing vulnerabilities based on version
7. Evaluate file sharing authentication and authorization:
   - Verify authentication requirements for share access
   - Test for shares with Everyone group permissions
   - Assess granularity of access controls (share vs NTFS permissions)
   - Identify service accounts with excessive file share access
   - Test for credential requirements and strength
8. Assess file sharing monitoring and auditing:
   - Verify file access logging enabled
   - Test whether file modifications are audited
   - Assess monitoring for unusual file access patterns
   - Verify alerting on bulk file access or data exfiltration indicators

## Expected Secure Configuration

Organizations deploying file sharing services should implement:

- Network segmentation isolating file servers from untrusted networks
- Firewall rules blocking SMB/NFS from Internet and guest networks
- SMBv3 with encryption and signing required
- NFS with Kerberos authentication (sec=krb5p)
- Principle of least privilege on share permissions
- Elimination of Everyone group from share ACLs
- Regular share permission reviews and access recertification
- File access auditing and monitoring for anomalous activity
- Sensitive data classification and appropriate storage controls
- Removal or securing of legacy file sharing protocols

## Evidence to Collect

- Complete inventory of SMB/CIFS and NFS services with locations
- SMB share enumeration results showing accessible shares
- NFS export enumeration showing mount points and permissions
- Share permission analysis identifying overly permissive access
- Sensitive data discoveries on accessible shares
- SMB version identification and vulnerability assessment
- Network accessibility testing from various segments
- Authentication and authorization testing results
- File access logging configuration verification
- Screenshots or listings of sensitive files accessible through shares

## Impact

**Data Breach and Exfiltration Impact:**
Exposed file shares provide direct access to confidential documents, customer data, financial records, and intellectual property. Attackers browse shares and exfiltrate gigabytes or terabytes of data without detection. Ransomware encrypts accessible file shares destroying business-critical data. Sensitive data exposure through misconfigured shares leads to regulatory violations and data breach notifications.

**Credential Theft Impact:**
File shares frequently contain scripts, configuration files, and documentation with embedded credentials. Password files, SSH private keys, database connection strings, and API keys discovered on shares enable further compromise. Service account credentials found in shared documents provide elevated access across infrastructure.

**Lateral Movement and Privilege Escalation Impact:**
Compromised file shares enable credential harvesting facilitating lateral movement. Source code and internal documentation on shares reveal architecture and vulnerability information. File shares provide staging locations for malware deployment and persistence mechanisms. Access to file shares enables attackers to modify legitimate files injecting malicious code.

## MITRE ATT&CK Mapping

- **T1021.002** - Lateral Movement: Remote Services: SMB/Windows Admin Shares
- **T1039** - Collection: Data from Network Shared Drive
- **T1135** - Discovery: Network Share Discovery
- **T1005** - Collection: Data from Local System
- **T1486** - Impact: Data Encrypted for Impact (ransomware via file shares)

## Remediation Guidance

Organizations should implement the following file sharing security practices:

1. **Network Segmentation and Access Control:**
   - Deploy dedicated file server network segments
   - Implement firewall rules blocking SMB ports (139, 445) from Internet
   - Block SMB/NFS from guest and untrusted networks
   - Restrict file sharing traffic to internal corporate networks only
   - Deploy network access control (NAC) for file server connectivity
   - Use VPN for remote file share access instead of direct exposure

2. **SMB/CIFS Protocol Hardening:**
   - Disable SMBv1 completely across all systems and servers
   - Enable SMBv3 with encryption in transit
   - Require SMB signing on clients and servers
   - Configure "Restrict Anonymous" preventing null session enumeration
   - Implement share-level and NTFS-level access controls
   - Remove administrative shares ($C, $ADMIN) if not required

3. **NFS Security Enhancement:**
   - Implement Kerberos authentication for NFS (sec=krb5p with encryption)
   - Restrict NFS exports to specific client IP addresses or networks
   - Use NFSv4 with security features instead of legacy NFSv2/v3
   - Disable portmapper access from untrusted networks
   - Implement NFS export root squashing preventing root access

4. **Share Permission Management:**
   - Remove Everyone group from all share ACLs
   - Implement role-based access control (RBAC) for file shares
   - Grant permissions to security groups, not individual users
   - Apply principle of least privilege to share permissions
   - Conduct quarterly share permission reviews and recertification
   - Implement automated alerts on permission changes

5. **Sensitive Data Protection:**
   - Implement data classification and labeling
   - Restrict sensitive data to dedicated shares with enhanced controls
   - Deploy data loss prevention (DLP) monitoring file shares
   - Encrypt sensitive data at rest on file servers
   - Implement file screening blocking credential files and sensitive patterns
   - Regular scanning for sensitive data in inappropriate locations

6. **Monitoring and Auditing:**
   - Enable comprehensive file access auditing
   - Monitor for bulk file access or mass downloads
   - Alert on file access from unusual locations or accounts
   - Deploy User and Entity Behavior Analytics (UEBA) for file shares
   - Implement honeypot shares detecting unauthorized access
   - Regular analysis of file access logs identifying anomalous patterns
