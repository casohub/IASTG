# INF-00.03 — Identification of Out-of-Scope Systems

## Summary

This test verifies that systems, networks, and services explicitly excluded from testing have been identified, documented, and communicated to prevent unauthorized testing of restricted assets.

## Risk

Testing out-of-scope systems results in unauthorized access, potential disruption of critical business operations, legal liability, and contractual breach. Failure to clearly identify exclusions may cause inadvertent testing of third-party systems, shared infrastructure, or business-critical assets that cannot tolerate security testing impacts. Organizations face service disruption, data loss, and regulatory violations when testing boundaries are not enforced.

## Preconditions

- Written authorization document exists (see INF-00.01)
- In-scope assets have been defined (see INF-00.02)
- No technical access is required

## Test Objectives

- Verify explicit documentation of out-of-scope systems and networks
- Confirm out-of-scope exclusions are unambiguous and technically identifiable
- Validate rationale for out-of-scope designations is documented
- Ensure out-of-scope boundaries do not create untestable security gaps
- Confirm procedures exist for handling inadvertent out-of-scope access

## Test Methodology

1. Review authorization and scope documentation for explicit out-of-scope definitions including:
   - IP ranges, subnets, or individual addresses excluded from testing
   - Hostnames, FQDNs, or domain names that must not be tested
   - Specific services, ports, or applications restricted from testing
   - Third-party hosted systems or shared infrastructure
   - Business-critical systems with zero-tolerance for impact
2. Verify out-of-scope exclusions are specified using concrete technical identifiers rather than ambiguous descriptions
3. Analyze out-of-scope definitions for potential conflicts with in-scope definitions:
   - Overlapping IP ranges where subset is excluded
   - Host exclusions within in-scope network ranges
   - Service-level exclusions on otherwise in-scope hosts
4. Request documented rationale for each out-of-scope designation to understand:
   - Business criticality or zero-downtime requirements
   - Third-party ownership or shared responsibility
   - Legal or regulatory constraints
   - Technical limitations preventing testing
5. Identify any implicit exclusions not explicitly documented such as:
   - Management networks or infrastructure control planes
   - Hypervisor or virtualization management systems
   - Backup and disaster recovery systems
   - Third-party SaaS or cloud services
6. Confirm procedures are documented for handling scenarios where:
   - Out-of-scope systems are inadvertently accessed during testing
   - Lateral movement paths traverse out-of-scope systems
   - Vulnerabilities are discovered that impact out-of-scope assets
7. Document any security coverage gaps created by out-of-scope exclusions
8. Verify notification requirements if out-of-scope systems are encountered during testing

## Expected Secure Configuration

Organizations defining out-of-scope exclusions should provide:

- Explicit list of excluded IP ranges, hostnames, and services using technical identifiers
- Documented business or technical rationale for each exclusion
- Clear distinction between absolute exclusions and conditional restrictions
- Procedures for handling inadvertent out-of-scope access
- Risk acceptance documentation for untested assets
- Alternative security validation methods for excluded critical systems
- Network-level controls preventing access to out-of-scope systems where technically feasible
- Out-of-scope boundary definitions aligned with network segmentation architecture

## Evidence to Collect

- Complete list of out-of-scope IP ranges, hostnames, or services
- Documentation of exclusion rationale for each out-of-scope asset
- Network diagrams showing out-of-scope boundaries
- Procedures for inadvertent out-of-scope access scenarios
- Risk acceptance or mitigation documentation for untested critical assets
- Communication records clarifying out-of-scope questions
- Confirmation of implicit exclusions (third-party systems, management networks)
- Version and date of out-of-scope documentation

## Impact

**Operational Impact:**
Testing out-of-scope systems may cause service disruptions to business-critical applications, financial systems, or production environments that cannot tolerate security testing impacts. Service level agreements (SLAs) may be violated and customer-facing services disrupted.

**Legal and Compliance Impact:**
Accessing third-party systems or shared infrastructure without proper authorization violates contractual agreements and potentially applicable laws. Regulatory frameworks such as PCI DSS, HIPAA, or GDPR may impose restrictions on testing of systems processing regulated data.

**Security Coverage Impact:**
Excessive out-of-scope exclusions create blind spots in security posture assessment. Critical attack paths may traverse out-of-scope systems, and vulnerabilities in excluded assets remain unidentified. Organizations face residual risk from untested assets.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

## Remediation Guidance

Organizations should implement the following out-of-scope management practices:

1. **Explicit Exclusion Documentation:**
   - Define all out-of-scope assets using technical identifiers
   - Provide exclusion rationale for audit and risk management purposes
   - Document temporary versus permanent exclusions
   - Maintain version-controlled exclusion lists with change tracking

2. **Risk-Based Exclusion Decisions:**
   - Limit exclusions to assets with documented business or technical justification
   - Conduct risk assessment for untested critical systems
   - Develop alternative security validation methods for excluded assets
   - Document risk acceptance for coverage gaps created by exclusions

3. **Technical Boundary Enforcement:**
   - Implement network-level controls to prevent access to out-of-scope systems
   - Use firewall rules or access control lists to enforce boundaries
   - Deploy monitoring to detect inadvertent out-of-scope access
   - Provide testing isolated environments where appropriate

4. **Incident Procedures:**
   - Define notification requirements for inadvertent out-of-scope access
   - Establish escalation procedures for out-of-scope incidents
   - Document evidence preservation requirements if exclusions are violated
   - Conduct post-incident review of out-of-scope boundaries

5. **Coverage Gap Management:**
   - Identify alternative security controls for out-of-scope critical assets
   - Schedule periodic security reviews of permanently excluded systems
   - Document compensating controls mitigating risks from untested assets
   - Review out-of-scope designations annually and adjust based on risk changes
