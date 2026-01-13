# INF-01.07 — DNS Zone Transfer Testing

## Summary

This test attempts DNS zone transfers (AXFR and IXFR) to determine whether DNS servers permit unauthorized bulk retrieval of all DNS records within a zone.

## Risk

Successful unauthorized DNS zone transfers disclose complete zone inventory in single operation, providing attackers with comprehensive network maps, hostnames, IP addresses, and service locations. Organizations face immediate reconnaissance advantage for attackers who obtain full infrastructure topology without incremental enumeration. Zone transfers bypass individual query rate limiting and monitoring, enabling rapid information gathering. Misconfigured zone transfer permissions represent fundamental DNS security failure exposing entire domain infrastructure.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Internal DNS servers have been identified (see INF-01.06)
- Network connectivity to DNS servers exists
- Authorization includes testing of zone transfer security

## Test Objectives

- Verify zone transfer restrictions are properly configured
- Test both full (AXFR) and incremental (IXFR) zone transfer attempts
- Identify DNS servers permitting unauthorized zone transfers
- Assess zone transfer authentication mechanisms
- Evaluate monitoring and detection of zone transfer attempts

## Test Methodology

1. Identify authoritative DNS servers for internal zones:
   - Query NS records to identify authoritative name servers
   - Determine primary and secondary DNS server roles
   - Identify all DNS servers hosting each zone
2. Attempt full zone transfer (AXFR) from unauthenticated position:
   - Execute AXFR query against each identified DNS server
   - Test zone transfers for all discovered internal domains
   - Attempt zone transfer from different source IPs if multiple positions available
   - Document whether zone transfer succeeds or is refused
3. Attempt incremental zone transfer (IXFR):
   - Execute IXFR query with serial number parameter
   - Test whether IXFR is permitted when AXFR is blocked
   - Document response differences between AXFR and IXFR attempts
4. Test zone transfer from multiple network positions:
   - Attempt transfers from internal network position
   - Test from DMZ or segmented networks if accessible
   - Verify transfers are blocked from unauthorized network segments
5. Analyze successful zone transfer contents:
   - Extract complete list of A, AAAA, CNAME, MX, SRV, TXT records
   - Identify all hosts, services, and infrastructure components
   - Compare zone transfer data with individual enumeration results
   - Document sensitive information disclosed through zone data
6. Test zone transfer authentication:
   - Attempt zone transfers with TSIG authentication if credentials known
   - Test for weak or default TSIG keys
   - Verify authentication requirements are enforced
7. Evaluate zone transfer error messages:
   - Analyze zone transfer refusal responses for information disclosure
   - Document whether error messages reveal zone existence or configuration details
   - Test for timing differences indicating zone existence
8. Verify monitoring and logging:
   - Review DNS server logs for zone transfer attempt recording
   - Test whether zone transfer attempts trigger alerts
   - Evaluate detection capabilities for unauthorized transfer attempts

## Expected Secure Configuration

Organizations securing DNS zone transfers should implement:

- Zone transfers disabled by default on all authoritative DNS servers
- Zone transfer permissions explicitly restricted to authorized secondary servers by IP
- TSIG (Transaction Signature) authentication required for zone transfers
- Unique TSIG keys per secondary server relationship
- Cryptographically strong TSIG keys with regular rotation
- Zone transfer logging capturing all attempts (successful and failed)
- Real-time alerting on unexpected zone transfer requests
- Network-level access controls limiting zone transfer traffic to authorized paths
- Regular audit of zone transfer configuration and permissions
- Documented change management for zone transfer authorization modifications

## Evidence to Collect

- Zone transfer attempt commands and syntax used
- DNS server responses to zone transfer requests (success or refusal)
- Complete zone data if transfer succeeded, including all record types
- List of DNS servers tested with their zone transfer status
- Error messages or responses from failed transfer attempts
- TSIG authentication testing results
- Network positions from which transfers were attempted
- DNS server log excerpts showing transfer attempts if accessible
- Comparative analysis of zone transfer versus individual query enumeration results
- Alert or monitoring system responses to transfer attempts

## Impact

**Complete Information Disclosure Impact:**
Successful zone transfers provide complete DNS zone inventory in single request. Attackers obtain comprehensive list of all hostnames, IP addresses, service locations, mail servers, and infrastructure components. Enumeration that might require thousands of individual queries completes instantly. Full network topology becomes visible without incremental discovery efforts.

**Reconnaissance Efficiency Impact:**
Zone transfers enable rapid reconnaissance bypassing query rate limiting, monitoring, and detection mechanisms designed to identify enumeration. Single zone transfer provides equivalent information to extensive scanning and query campaigns. Attackers gain complete understanding of infrastructure in minutes rather than hours or days.

**Attack Surface Exposure Impact:**
Zone data reveals all potential attack targets including development environments, administrative systems, backup infrastructure, and testing platforms. Obsolete or forgotten hosts disclosed in zone data provide weak entry points. Service location records expose Active Directory, database, and application server locations enabling targeted attacks.

## MITRE ATT&CK Mapping

- **T1590.002** - Reconnaissance: Gather Victim Network Information: DNS
- **T1596** - Reconnaissance: Search Open Technical Databases

## Remediation Guidance

Organizations should implement the following zone transfer security controls:

1. **Zone Transfer Restriction:**
   - Configure all authoritative DNS servers with zone transfer disabled by default
   - Create explicit allow-transfer or allow-query-transfer directives only for authorized secondaries
   - Specify authorized secondary servers by IP address, not by network range
   - Verify zone transfer configuration on all DNS servers regularly
   - Test zone transfer restrictions as part of configuration validation

2. **TSIG Authentication Implementation:**
   - Deploy TSIG authentication for all zone transfer relationships
   - Generate cryptographically strong TSIG keys (minimum 256-bit HMAC-SHA256)
   - Use unique TSIG keys for each primary-secondary server relationship
   - Store TSIG keys securely with access controls
   - Implement TSIG key rotation procedures (annual or upon personnel changes)

3. **Network-Level Protection:**
   - Implement firewall rules restricting DNS zone transfer traffic (TCP port 53)
   - Limit zone transfer connections to explicit IP pairs (primary to secondary)
   - Deploy network segmentation isolating DNS infrastructure
   - Block zone transfer attempts from untrusted networks at perimeter
   - Use private network connections between geographically distributed DNS servers

4. **Monitoring and Alerting:**
   - Enable comprehensive logging of zone transfer requests on all DNS servers
   - Log both successful and failed transfer attempts with source IP
   - Implement real-time alerts for unexpected zone transfer requests
   - Alert on zone transfers from unauthorized source IPs
   - Integrate DNS zone transfer logs with SIEM for correlation

5. **Zone Content Minimization:**
   - Remove unnecessary DNS records reducing information disclosure
   - Use split-horizon DNS limiting external zone exposure
   - Implement separate internal and external DNS zones
   - Avoid descriptive hostnames in external DNS zones
   - Regular audit and cleanup of DNS records
   - Evaluate whether zone transfer capability is required; consider alternatives like DNS synchronization APIs
