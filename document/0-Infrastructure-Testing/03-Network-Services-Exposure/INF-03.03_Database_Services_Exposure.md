# INF-03.03 — Database Services Exposure (Ports and Credentials)

## Summary

This test identifies database services exposed across networks, assesses their accessibility and authentication controls, and evaluates credential security to prevent unauthorized data access.

## Risk

Exposed database services provide direct access to organizational data including customer information, financial records, intellectual property, and operational data. Internet-facing databases suffer constant scanning and exploitation attempts. Weak database authentication enables unauthorized data access through credential guessing or SQL injection. Default database credentials provide immediate administrative access to data. Unencrypted database connections expose credentials and data to interception. Excessive database privileges on service accounts enable data exfiltration and modification beyond application requirements. Organizations face massive data breaches when database services are exposed without proper authentication, encryption, and access controls.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network and service discovery completed (see INF-01.02, INF-01.03)
- Database services have been identified
- Testing includes database service evaluation

## Test Objectives

- Identify all database services and their network exposure
- Assess database authentication mechanisms and credential security
- Test for default or weak database credentials
- Evaluate database connection encryption and security
- Validate principle of least privilege on database accounts

## Test Methodology

1. Enumerate database services across infrastructure:
   - **Microsoft SQL Server (MSSQL):** TCP ports 1433, 1434
   - **MySQL/MariaDB:** TCP port 3306
   - **PostgreSQL:** TCP port 5432
   - **Oracle Database:** TCP ports 1521, 1522
   - **MongoDB:** TCP port 27017
   - **Redis:** TCP port 6379
   - **Cassandra:** TCP port 9042
   - **Elasticsearch:** TCP port 9200, 9300
2. Assess database network exposure:
   - Test database accessibility from Internet
   - Verify database accessibility from untrusted networks
   - Identify databases accessible from user workstation networks
   - Test network segmentation isolating database servers
   - Document firewall rules controlling database access
3. Test database authentication mechanisms:
   - Verify authentication is required for database connections
   - Test for anonymous or unauthenticated database access
   - Attempt connection with default credentials (sa, root, admin)
   - Test for weak password policies on database accounts
   - Assess authentication methods (SQL authentication vs integrated authentication)
4. Enumerate database users and privileges:
   - List database accounts and their privilege levels
   - Identify accounts with sysadmin or superuser privileges
   - Assess service account permissions and privilege scope
   - Test for overly permissive database roles
   - Verify principle of least privilege implementation
5. Test database connection encryption:
   - Verify TLS/SSL encryption is required for database connections
   - Test whether unencrypted connections are permitted
   - Assess encryption strength and protocol versions
   - Verify certificate validation on encrypted connections
   - Test for man-in-the-middle susceptibility
6. Identify database-specific security issues:
   - **SQL Server:** xp_cmdshell enabled, linked servers, SQL injection
   - **MySQL:** Grant tables, file privileges, user-defined functions
   - **PostgreSQL:** Superuser accounts, COPY commands, extensions
   - **MongoDB:** Unprotected instances, NoSQL injection, role privileges
   - **Oracle:** Default schemas, PL/SQL injection, TNS poisoning
7. Test for credential exposure:
   - Search configuration files for database connection strings
   - Identify applications with embedded database credentials
   - Check for database credentials in environment variables
   - Test for credentials in application logs or error messages
   - Assess database credential storage and encryption
8. Evaluate database auditing and monitoring:
   - Verify database authentication logging is enabled
   - Test for monitoring of privileged database operations
   - Assess alerting on suspicious database activity
   - Verify audit logs capture data access and modifications
   - Test for detection of SQL injection attempts

## Expected Secure Configuration

Organizations deploying database services should implement:

- Network segmentation isolating databases from untrusted networks
- Firewall rules restricting database access to application servers only
- Strong authentication with unique credentials per database and application
- Elimination of default database credentials (sa, root, admin, postgres)
- Mandatory encryption for database connections (TLS 1.2 minimum)
- Principle of least privilege on database accounts and service accounts
- No sysadmin or superuser privileges on application service accounts
- Database auditing capturing authentication and data access events
- Regular vulnerability scanning and patching of database software
- Secrets management for database credential storage and rotation

## Evidence to Collect

- Complete inventory of database services with network locations
- Network exposure assessment showing accessibility from various segments
- Database authentication testing results including default credential attempts
- Database user enumeration with privilege levels
- Connection encryption validation results
- Database-specific security configuration analysis
- Credential exposure discoveries in configurations and applications
- Database auditing and logging configuration verification
- Service account privilege assessment
- Vulnerability scanning results for database software

## Impact

**Data Breach and Exfiltration Impact:**
Compromised database credentials provide direct access to sensitive data including customer information, financial records, healthcare data, and intellectual property. Attackers exfiltrate entire databases containing millions of records. SQL injection vulnerabilities enable unauthorized data access bypassing application controls. Unencrypted database connections expose data in transit to interception.

**Unauthorized Data Modification Impact:**
Excessive database privileges enable attackers to modify data affecting business operations and data integrity. Financial transaction manipulation, customer record changes, and product catalog modifications cause business harm. Database backup deletion or encryption (ransomware) destroys business-critical data.

**Lateral Movement and Privilege Escalation Impact:**
SQL Server xp_cmdshell and similar features enable operating system command execution from database context. Linked servers provide paths to additional databases. Database credentials frequently reused across multiple systems. Compromised database service accounts with elevated OS privileges enable system-level compromise.

## MITRE ATT&CK Mapping

- **T1190** - Initial Access: Exploit Public-Facing Application (SQL injection)
- **T1552.001** - Credential Access: Unsecured Credentials: Credentials In Files (connection strings)
- **T1213** - Collection: Data from Information Repositories
- **T1530** - Collection: Data from Cloud Storage Object
- **T1567** - Exfiltration: Exfiltration Over Web Service

## Remediation Guidance

Organizations should implement the following database security practices:

1. **Network Segmentation and Access Control:**
   - Deploy databases in dedicated network segments
   - Implement firewall rules restricting database ports to application servers only
   - Block database access from Internet and untrusted networks
   - Use jump hosts or bastion hosts for administrative database access
   - Deploy database firewalls filtering SQL queries
   - Implement microsegmentation between application and database tiers

2. **Authentication Hardening:**
   - Eliminate default database credentials immediately
   - Implement strong password policies for database accounts (20+ characters)
   - Use unique credentials per database and application
   - Deploy integrated authentication (Kerberos, Windows auth) where possible
   - Implement certificate-based database authentication
   - Remove or disable unused database accounts

3. **Connection Encryption:**
   - Require TLS/SSL encryption for all database connections
   - Configure TLS 1.2 minimum, prefer TLS 1.3
   - Implement certificate validation preventing man-in-the-middle attacks
   - Disable unencrypted database listeners
   - Encrypt data at rest using database transparent data encryption (TDE)
   - Implement column-level encryption for sensitive fields

4. **Privilege Management:**
   - Implement principle of least privilege on all database accounts
   - Remove sysadmin/superuser privileges from application service accounts
   - Grant only required permissions (SELECT, INSERT, UPDATE, DELETE)
   - Restrict access to specific schemas, tables, and stored procedures
   - Disable dangerous features (xp_cmdshell, OPENROWSET, file operations)
   - Conduct quarterly privilege reviews and recertification

5. **Credential Management:**
   - Deploy secrets management solutions for database credentials (Azure Key Vault, AWS Secrets Manager)
   - Remove embedded credentials from application code and configuration files
   - Implement automated credential rotation
   - Use managed identities or Azure AD authentication for cloud databases
   - Encrypt database connection strings
   - Monitor for credential exposure in logs, repositories, and public sources

6. **Database Security Monitoring:**
   - Enable comprehensive database auditing
   - Log all authentication attempts (success and failure)
   - Monitor privileged operations and data access patterns
   - Implement real-time alerting on suspicious queries
   - Deploy database activity monitoring (DAM) solutions
   - Integrate database logs with SIEM for correlation
   - Monitor for SQL injection attempts and anomalous queries
