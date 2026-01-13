# INF-01.04 — Identification of Legacy or Unsupported Systems

## Summary

This test identifies systems running end-of-life (EOL) operating systems, unsupported software versions, or legacy platforms that no longer receive security updates from vendors.

## Risk

Legacy and unsupported systems lack security patches for newly discovered vulnerabilities, creating persistent exposure to exploitation. Organizations face breaches through unpatched vulnerabilities in EOL systems that vendors no longer support. Compliance frameworks prohibit use of unsupported systems processing regulated data. Legacy systems often cannot implement modern security controls, authentication mechanisms, or encryption standards. Business continuity risks emerge when critical applications depend on obsolete platforms approaching failure without vendor support.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Hosts have been discovered (see INF-01.02)
- Services and banners have been enumerated (see INF-01.03)
- Software version information has been collected

## Test Objectives

- Identify operating systems that have reached vendor end-of-life
- Detect applications and services running unsupported versions
- Enumerate legacy protocols and technologies without security update support
- Assess criticality and exposure of identified legacy systems
- Validate compensating controls for legacy systems that cannot be upgraded

## Test Methodology

1. Analyze operating system versions from service banners and discovery data:
   - Windows: Windows 2000, XP, 2003, Vista, 7, 2008/2008 R2, 8/8.1, 2012/2012 R2 (EOL dates vary)
   - Linux: Red Hat/CentOS 6 and earlier, Ubuntu versions past EOL
   - Unix: Solaris, HP-UX, AIX versions no longer supported
   - Network devices: Cisco IOS, Juniper JunOS versions past support lifecycle
2. Cross-reference identified versions against vendor end-of-life databases:
   - Microsoft Product Lifecycle Database
   - Red Hat Product Life Cycle
   - Cisco End-of-Life Notices
   - Ubuntu Release Cycle
   - Application vendor support policies
3. Identify unsupported applications and services:
   - Web servers: Apache, IIS, Nginx versions without security support
   - Databases: MySQL, PostgreSQL, MSSQL, Oracle versions past EOL
   - Application frameworks: Java, .NET, PHP, Python versions unsupported
   - Middleware and enterprise applications past support lifecycle
4. Enumerate legacy protocols and technologies without security support:
   - SMBv1, TLS 1.0/1.1, SSL 2.0/3.0
   - Telnet, FTP, TFTP without encryption
   - Legacy authentication protocols (NTLMv1, LANMAN)
   - Deprecated cryptographic algorithms (DES, MD5, RC4)
5. Assess legacy system characteristics:
   - Business function and criticality of legacy systems
   - Data sensitivity and compliance requirements
   - Network exposure (Internet-facing, internal, isolated)
   - Dependencies and applications requiring legacy platforms
   - Available upgrade or migration paths
6. Evaluate compensating controls for unavoidable legacy systems:
   - Network isolation or segmentation protecting legacy systems
   - Firewall rules restricting access to legacy platforms
   - Additional monitoring or IDS coverage
   - Privilege access restrictions
   - Regular vulnerability scanning despite lack of patches
7. Document upgrade or remediation timelines:
   - Systems with planned upgrade or replacement projects
   - Systems accepted as risk with compensating controls
   - Systems requiring immediate remediation due to exposure or compliance
8. Identify business or technical barriers to legacy system remediation:
   - Application compatibility requirements
   - Budget or resource constraints
   - Specialized hardware or software dependencies
   - Vendor or third-party support limitations

## Expected Secure Configuration

Organizations managing system lifecycle should implement:

- Asset inventory tracking operating system and software versions with EOL dates
- Automated alerting for systems approaching end-of-life
- Formal policy prohibiting unsupported systems without documented risk acceptance
- System upgrade and migration planning integrated with EOL timelines
- Documented risk acceptance for legacy systems with compensating controls
- Network segmentation isolating legacy systems from general networks
- Enhanced monitoring and intrusion detection for unsupported platforms
- Regular vulnerability assessments of legacy systems
- Business case development for legacy system replacement
- Compliance validation ensuring EOL systems do not process regulated data

## Evidence to Collect

- Complete list of identified legacy and EOL systems with versions
- Operating system EOL status verification from vendor databases
- Application and service EOL status documentation
- Legacy protocol and cryptographic algorithm usage inventory
- Network exposure assessment for each legacy system
- Documentation of compensating controls for EOL systems
- Upgrade or migration timeline documentation
- Risk acceptance documentation for legacy systems
- Compliance assessment results for EOL systems processing regulated data
- Business justification for continued operation of legacy platforms

## Impact

**Security Exposure Impact:**
Unpatched vulnerabilities in EOL systems provide persistent attack vectors that cannot be remediated through patching. Known exploits target legacy operating systems and applications. Attackers prioritize EOL systems as high-probability targets. Successful exploitation leads to system compromise, data breaches, and lateral movement throughout networks.

**Compliance and Regulatory Impact:**
Regulatory frameworks prohibit EOL systems processing payment card data (PCI DSS), healthcare information (HIPAA), or personal data (GDPR). Organizations face audit failures, penalties, and loss of certifications. Continued use of unsupported systems despite compliance requirements demonstrates negligent security practices.

**Operational Risk Impact:**
Legacy systems approaching end-of-support create business continuity risks. Hardware failures occur without spare parts availability. Software issues lack vendor support or patches. Business-critical applications depending on EOL platforms face disruption risk. Organizations remain locked to obsolete technologies preventing modernization.

## MITRE ATT&CK Mapping

- **T1190** - Initial Access: Exploit Public-Facing Application
- **T1210** - Lateral Movement: Exploitation of Remote Services
- **T1212** - Exploitation for Credential Access

Note: EOL systems are particularly vulnerable to exploitation tactics across Initial Access, Privilege Escalation, and Lateral Movement.

## Remediation Guidance

Organizations should implement the following legacy system management practices:

1. **Lifecycle Management Program:**
   - Track all system and application EOL dates in asset inventory
   - Implement automated alerting 18-24 months before EOL dates
   - Establish policy requiring upgrade or replacement before EOL
   - Integrate EOL tracking with change management and budgeting
   - Conduct quarterly reviews of systems approaching EOL

2. **Migration and Upgrade Planning:**
   - Develop upgrade or migration project plans before EOL dates
   - Allocate budget for system replacements and upgrades
   - Test application compatibility with newer platforms
   - Plan phased migration strategies for complex environments
   - Engage vendors for upgrade assistance and migration tools

3. **Risk-Based Prioritization:**
   - Prioritize EOL system remediation based on exposure and criticality
   - Immediately address Internet-facing EOL systems
   - Prioritize systems processing regulated or sensitive data
   - Evaluate business impact of system unavailability during migration
   - Consider risk tolerance and compensating controls for each system

4. **Compensating Controls for Unavoidable Legacy:**
   - Implement network isolation for systems that cannot be upgraded
   - Deploy additional network segmentation and access controls
   - Increase monitoring and intrusion detection coverage
   - Implement strict change control preventing modification
   - Conduct frequent vulnerability assessments and penetration testing
   - Document risk acceptance with executive approval

5. **Strategic Modernization:**
   - Develop long-term IT modernization roadmap
   - Replace legacy applications enabling EOL platform retirement
   - Virtualize or containerize legacy workloads where possible
   - Evaluate cloud migration opportunities for legacy applications
   - Prioritize security and maintainability in technology selection decisions
