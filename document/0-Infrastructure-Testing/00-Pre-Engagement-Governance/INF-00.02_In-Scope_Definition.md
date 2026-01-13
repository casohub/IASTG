# INF-00.02 — Definition of In-Scope Networks, Hosts, and Services

## Summary

This test verifies that specific in-scope assets including networks, IP ranges, hostnames, and services have been explicitly defined and documented prior to testing activities.

## Risk

Testing undefined or ambiguously scoped systems may result in unauthorized access to systems outside the intended scope, causing business disruption, legal liability, and damage to systems not prepared for security testing. Unclear scope definitions lead to incomplete testing coverage, failed detection of vulnerabilities in assumed-excluded systems, and disputes regarding testing deliverables.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Point of contact is available to clarify scope questions
- No technical access is required

## Test Objectives

- Verify explicit definition of all in-scope network ranges in CIDR notation or equivalent
- Confirm in-scope hostnames and domain names are documented
- Validate in-scope services and applications are specified
- Ensure in-scope assets are unambiguously identifiable
- Confirm scope boundaries are technically enforceable

## Test Methodology

1. Review authorization document and supplementary scope documentation for the following elements:
   - IP address ranges in CIDR notation (e.g., 192.168.1.0/24)
   - Individual IP addresses if specific hosts are in-scope
   - Hostnames or fully qualified domain names (FQDNs)
   - Domain names for DNS-based scope definition
2. Verify that network scope definitions do not contain overlapping or contradictory ranges
3. Confirm service-level scope if testing is limited to specific protocols or applications:
   - Web applications and URLs
   - Specific network services (e.g., SSH, RDP, databases)
   - Application-layer targets
4. Request clarification for any ambiguous scope definitions such as:
   - Use of terms like "internal network" without IP specification
   - Partial IP ranges without subnet masks
   - References to logical groupings without technical identifiers
5. Obtain asset inventory or network diagram if available to cross-reference scope
6. Validate scope includes both production and non-production environments if applicable
7. Document any scope elements that are defined by business function rather than technical identifiers
8. Confirm scope definition method supports testable boundary enforcement

## Expected Secure Configuration

Organizations defining testing scope should provide:

- Complete list of in-scope IP ranges in CIDR notation with no ambiguous entries
- Hostname or FQDN lists for systems identified by name rather than IP
- Service or port specifications if testing is constrained to specific protocols
- Network topology diagram showing in-scope network segments
- Asset inventory mapping IP addresses to asset names and functions
- Clear distinction between production, staging, development, and test environments
- Documented rationale for scope boundaries where exclusions exist
- Version-controlled scope documentation with change tracking

## Evidence to Collect

- Copy of documented in-scope IP ranges in CIDR or equivalent format
- List of in-scope hostnames, FQDNs, or domain names
- Service or application-level scope restrictions documentation
- Network diagrams or topology maps showing in-scope segments
- Asset inventory cross-reference if provided
- Email or written correspondence clarifying ambiguous scope elements
- Scope boundary definition explaining why specific assets are included
- Version and date of scope documentation

## Impact

**Security Impact:**
Ambiguous scope definitions result in incomplete testing where critical assets are inadvertently excluded. Vulnerabilities in undefined areas remain undetected, creating a false sense of security and leaving attack vectors unaddressed.

**Legal and Contractual Impact:**
Unclear scope creates disputes regarding deliverables and may result in testing activities that exceed authorization. This exposes both tester and client to legal liability and contractual breach.

**Operational Impact:**
Poor scope definition leads to testing inefficiency, wasted effort on out-of-scope systems, and the need for costly scope renegotiation during the engagement. Testing timelines are extended and resources are misallocated.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

## Remediation Guidance

Organizations should implement the following scope definition practices:

1. **Technical Scope Documentation:**
   - Use CIDR notation for all IP-based scope definitions
   - Provide both IP and hostname references for dual identification
   - Include subnet masks and network boundaries explicitly
   - Document DNS domains and zones if name-based scope is used

2. **Asset Inventory Integration:**
   - Maintain current asset inventory with IP-to-hostname mappings
   - Tag assets with environment classifications (production, staging, etc.)
   - Cross-reference scope documentation with asset management systems
   - Update asset inventory prior to each testing engagement

3. **Scope Review Process:**
   - Conduct technical scope review with network and security teams
   - Validate scope against current network architecture
   - Review scope for completeness and coverage of critical assets
   - Document scope exclusion rationale for audit purposes

4. **Scope Communication:**
   - Provide scope documentation in structured machine-readable format where possible
   - Include visual network diagrams to supplement IP listings
   - Establish scope clarification procedures for tester questions
   - Define scope change request process for mid-engagement adjustments

5. **Scope Validation:**
   - Perform pre-testing scope verification sweeps
   - Confirm in-scope assets are reachable and active
   - Identify and document any scope conflicts or overlaps
   - Update scope documentation based on validation findings
