# INF-01.01 — Network Range Identification

## Summary

This test verifies that all in-scope network ranges are accurately identified and that network boundaries, routing, and reachability are understood prior to conducting detailed discovery activities.

## Risk

Incomplete or inaccurate network range identification results in security testing that misses critical network segments, leaving vulnerabilities undetected in untested areas. Organizations face exposure from unassessed network segments that contain high-value assets or weak security controls. Testing effort is misdirected toward incorrect or non-existent network ranges while actual infrastructure remains unvalidated. Network range errors may cause testing activities to inadvertently target out-of-scope systems or miss entire business units.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Network access to at least one in-scope network range or system
- Testing approach has been determined (see INF-00.04)

## Test Objectives

- Verify accuracy of documented in-scope network ranges
- Identify all active network segments within authorized scope
- Confirm network routing and reachability between segments
- Validate network range documentation matches actual infrastructure
- Identify undocumented network segments that may be in scope

## Test Methodology

1. Review documented in-scope network ranges provided in authorization:
   - CIDR notation ranges (e.g., 192.168.1.0/24, 10.0.0.0/16)
   - Individual IP addresses
   - Named network segments or VLANs
2. Perform initial network range validation using passive and active techniques:
   - Query routing tables on accessible systems to identify directly connected networks
   - Review ARP cache entries to identify local network ranges
   - Examine network interface configurations showing IP addressing and subnet masks
   - Analyze routing protocol advertisements if accessible (OSPF, EIGRP, BGP)
3. Validate reachability of documented network ranges:
   - Perform ICMP echo requests (ping) to sample addresses across each documented range
   - Test connectivity to network broadcast addresses or all-ones addresses
   - Verify routing exists to reach documented ranges from testing position
   - Identify network ranges that are documented but unreachable
4. Identify additional network ranges not explicitly documented:
   - Examine routing tables for routes to undocumented networks
   - Review network configurations for additional interface IP addresses
   - Analyze traceroute or network path information revealing intermediate networks
   - Check for NAT or proxy configurations indicating hidden networks
5. Document network topology and segmentation:
   - Identify routers and layer 3 boundaries between segments
   - Map network segments to business functions or organizational units
   - Identify DMZ, internal, management, and other security zones
   - Document VLAN structure if discoverable
6. Verify IPv6 network ranges if dual-stack infrastructure exists:
   - Query IPv6 routing tables and interface configurations
   - Identify IPv6 prefixes and address allocations
   - Test IPv6 reachability separately from IPv4
7. Confirm accuracy of network range documentation with client:
   - Report any discrepancies between documented and discovered ranges
   - Request clarification for undocumented but discovered networks
   - Confirm whether discovered additional ranges are in scope
8. Document final validated network ranges for subsequent testing phases

## Expected Secure Configuration

Organizations providing network range information should maintain:

- Accurate and current IP address management (IPAM) system
- Network topology documentation showing all network segments
- CIDR notation for all network ranges with subnet masks
- IPv4 and IPv6 address allocation documentation
- Network segmentation and security zone definitions
- Routing topology documentation showing inter-segment connectivity
- VLAN assignments and network naming conventions
- Documentation of cloud and hybrid network extensions
- Regular network discovery audits to validate documentation accuracy
- Version-controlled network documentation with change tracking

## Evidence to Collect

- Original documented in-scope network ranges from authorization
- Routing table outputs from accessible systems
- Network interface configuration details
- Ping sweep or connectivity test results for documented ranges
- Traceroute outputs showing network paths and intermediate networks
- Documentation of discovered undocumented network ranges
- Clarification communications regarding network range discrepancies
- Network topology diagram showing validated ranges and connectivity
- Final validated and confirmed network range list for testing

## Impact

**Security Coverage Impact:**
Incomplete network range identification leaves network segments untested, creating blind spots where vulnerabilities remain undetected. Entire business units, remote sites, or DMZ networks may be excluded from security validation. Attackers exploit untested network segments as entry points or pivot locations. Organizations face breaches originating from unassessed network areas.

**Testing Accuracy Impact:**
Inaccurate network ranges cause testing effort to focus on non-existent or out-of-scope systems. Resources are wasted on unreachable networks while critical infrastructure receives inadequate attention. Testing reports contain incomplete coverage assessments and misleading security posture conclusions.

**Operational Risk Impact:**
Undiscovered network segments may contain legacy systems, shadow IT, or unmanaged infrastructure with weak security controls. Critical assets in undocumented networks lack security oversight. Compliance violations occur when regulated systems exist in unidentified network ranges. Network architecture understanding gaps hinder effective security control implementation.

## MITRE ATT&CK Mapping

- **T1590.005** - Reconnaissance: Gather Victim Network Information: IP Addresses
- **T1590.004** - Reconnaissance: Gather Victim Network Information: Network Topology

## Remediation Guidance

Organizations should implement the following network range management practices:

1. **IP Address Management (IPAM):**
   - Deploy IPAM system tracking all network ranges and IP allocations
   - Maintain current network documentation with automatic updates
   - Document CIDR notation for all network ranges
   - Track IPv4 and IPv6 address assignments separately
   - Integrate IPAM with network device configurations

2. **Network Discovery and Validation:**
   - Conduct regular automated network discovery to identify active ranges
   - Perform periodic manual validation of network documentation accuracy
   - Reconcile discovered networks with IPAM records
   - Investigate and document any discrepancies
   - Update network range documentation following infrastructure changes

3. **Network Segmentation Documentation:**
   - Document all network security zones and their purposes
   - Maintain network topology diagrams showing segment relationships
   - Define and document VLAN structure and assignments
   - Map network segments to business functions and asset groups
   - Document routing between network segments

4. **Cloud and Hybrid Network Integration:**
   - Extend network range documentation to include cloud network ranges
   - Document VPC/VNet address spaces in cloud environments
   - Track hybrid connectivity (VPN, Direct Connect, ExpressRoute) network ranges
   - Maintain unified view of on-premises and cloud networks
   - Document cloud network segmentation and security group rules

5. **Change Management Integration:**
   - Require network range documentation updates for infrastructure changes
   - Review and approve new network segments through change control
   - Audit network changes against documentation periodically
   - Implement automated alerts for undocumented network segments
   - Conduct quarterly network range documentation reviews
