# INF-01.06 — Internal DNS Reconnaissance

## Summary

This test enumerates internal Domain Name System (DNS) records to identify hosts, services, and network infrastructure through name resolution queries and DNS server interrogation.

## Risk

DNS records disclose internal network architecture, naming conventions, and system inventory without requiring authentication. Attackers leverage DNS enumeration to identify critical systems, map organizational structure, discover service locations, and plan targeted attacks. Verbose DNS records reveal development environments, administrative systems, backup infrastructure, and application servers. Lack of DNS query restrictions enables comprehensive infrastructure reconnaissance from initial network access. Organizations face exposure of internal topology and high-value targets through unprotected DNS enumeration.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network connectivity to internal DNS servers exists
- DNS server IP addresses have been identified
- Testing is conducted from authenticated or internal network position

## Test Objectives

- Identify internal DNS servers and their configurations
- Enumerate DNS records revealing hosts and services
- Map organizational structure through DNS naming conventions
- Identify critical systems and applications via DNS records
- Assess DNS security controls and query restrictions

## Test Methodology

1. Identify internal DNS servers:
   - Query DHCP responses for DNS server assignments
   - Extract DNS server information from network configurations
   - Test connectivity to discovered DNS servers on port 53/UDP and 53/TCP
   - Identify authoritative DNS servers for internal domains
2. Enumerate internal DNS domains and zones:
   - Query DNS servers for domain names and zone information
   - Identify parent and child domains in DNS hierarchy
   - Discover internal Active Directory domain names
   - Enumerate subdomains and delegation zones
3. Perform forward DNS enumeration:
   - Query common hostname patterns (www, mail, vpn, ftp, ssh, admin, backup)
   - Enumerate service-specific records (domain controllers, file servers, databases)
   - Test sequential hostname enumeration (server1, server2, workstation1)
   - Query application-specific naming conventions
   - Perform DNS brute-force enumeration with wordlists
4. Conduct reverse DNS lookups:
   - Perform reverse DNS queries for discovered IP addresses
   - Identify PTR records mapping IPs to hostnames
   - Cross-reference forward and reverse DNS consistency
   - Discover internal naming conventions through reverse lookups
5. Query specialized DNS record types:
   - **A records:** IPv4 address mappings
   - **AAAA records:** IPv6 address mappings
   - **CNAME records:** Alias records revealing service relationships
   - **MX records:** Mail server identification
   - **SRV records:** Service location records (Active Directory services, SIP, XMPP)
   - **TXT records:** Descriptive information, SPF records, metadata
   - **NS records:** Name server delegations
6. Enumerate Active Directory DNS records:
   - Query _ldap._tcp.dc._msdcs.[domain] for domain controllers
   - Enumerate _kerberos._tcp.[domain] for Kerberos KDC locations
   - Query _gc._tcp.[domain] for Global Catalog servers
   - Identify forest and domain structure through DNS SRV records
7. Analyze DNS enumeration results for intelligence:
   - Identify naming conventions and organizational structure
   - Map hosts to business functions (HR, Finance, IT, Development)
   - Discover administrative and privileged systems
   - Identify development, staging, and production environments
   - Locate backup systems, monitoring infrastructure, and management platforms
8. Test DNS security controls:
   - Attempt DNS zone transfers (AXFR) if not blocked
   - Test query rate limiting and defensive measures
   - Evaluate DNS response authenticity (DNSSEC implementation)
   - Assess whether DNS queries are logged and monitored

## Expected Secure Configuration

Organizations securing internal DNS should implement:

- DNS query restrictions limiting enumeration from unauthorized hosts
- DNS zone transfer (AXFR) disabled or restricted to authorized secondary servers
- Internal DNS servers not accessible from Internet or untrusted networks
- Split-horizon DNS separating internal and external name resolution
- Descriptive naming conventions that do not expose system functions
- DNSSEC implementation for response authenticity
- DNS query logging and monitoring for enumeration attempts
- Rate limiting on DNS queries to prevent brute-force enumeration
- Removal of unnecessary or obsolete DNS records
- Regular DNS record hygiene and cleanup procedures

## Evidence to Collect

- List of identified internal DNS servers with IP addresses
- Complete enumeration of discovered DNS domains and zones
- Inventory of DNS A, AAAA, CNAME, MX, SRV, and TXT records
- Active Directory service location records (SRV)
- Reverse DNS lookup results
- DNS zone transfer attempt results
- Analysis of naming conventions and organizational intelligence
- Identification of critical systems discovered through DNS
- Documentation of DNS security control testing
- DNS query logs if accessible showing enumeration activity

## Impact

**Information Disclosure Impact:**
DNS enumeration reveals comprehensive internal network inventory without authentication. Attackers map organizational structure, identify critical systems, locate administrative infrastructure, and discover development environments. Naming conventions expose system purposes, business functions, and technology platforms. Enumeration provides target list for subsequent attacks.

**Targeted Attack Facilitation Impact:**
DNS records identify high-value targets including domain controllers, database servers, backup systems, and administrative workstations. Service location records (SRV) reveal Active Directory infrastructure enabling targeted attacks. Mail server records enable phishing and email-based attacks. Development and staging environment discovery exposes potentially weaker security controls.

**Operational Security Impact:**
Verbose DNS records violate principle of least disclosure. Internal naming reveals more information than necessary for name resolution. Obsolete DNS records for decommissioned systems provide misleading information. Lack of DNS query monitoring prevents detection of reconnaissance activities.

## MITRE ATT&CK Mapping

- **T1590.002** - Reconnaissance: Gather Victim Network Information: DNS
- **T1087.002** - Discovery: Account Discovery: Domain Account
- **T1018** - Discovery: Remote System Discovery

## Remediation Guidance

Organizations should implement the following DNS security practices:

1. **DNS Access Control:**
   - Restrict DNS query access to authenticated internal hosts
   - Block DNS server access from Internet and untrusted networks
   - Implement split-horizon DNS for internal versus external resolution
   - Deploy firewall rules limiting DNS traffic to authorized sources
   - Disable recursive queries on authoritative DNS servers

2. **Zone Transfer Protection:**
   - Disable DNS zone transfers (AXFR) unless required for replication
   - Restrict zone transfers to specific authorized secondary servers by IP
   - Implement transaction signatures (TSIG) for authenticated zone transfers
   - Monitor and alert on zone transfer attempts
   - Regularly audit zone transfer configuration

3. **DNS Record Hygiene:**
   - Implement naming conventions that do not reveal system functions
   - Use generic or non-descriptive hostnames for sensitive systems
   - Remove DNS records for decommissioned or offline systems
   - Conduct quarterly DNS record cleanup and validation
   - Document and justify retention of descriptive DNS names

4. **Query Monitoring and Rate Limiting:**
   - Enable comprehensive DNS query logging
   - Implement rate limiting on queries per source to prevent enumeration
   - Deploy behavioral analysis detecting enumeration patterns
   - Alert on excessive queries from single sources
   - Integrate DNS logs with SIEM for correlation

5. **DNSSEC and Security Enhancements:**
   - Implement DNSSEC for response authenticity and integrity
   - Deploy DNS response policy zones (RPZ) for threat blocking
   - Enable DNS over HTTPS (DoH) or DNS over TLS (DoT) for encrypted queries
   - Implement DNS firewall capabilities for malicious domain blocking
   - Regular security reviews of DNS infrastructure configuration
