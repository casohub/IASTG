# INF-00.06 — Identification of Business-Critical Assets

## Summary

This test verifies that business-critical assets, systems, and data have been explicitly identified, documented, and communicated to ensure appropriate testing prioritization and impact management.

## Risk

Failure to identify business-critical assets results in inadequate protection of systems essential to organizational operations, revenue generation, regulatory compliance, and safety. Testing may inadvertently impact critical systems causing significant business disruption, financial loss, or regulatory violations. Conversely, security testing may focus on low-value assets while critical systems remain unassessed, leaving high-impact vulnerabilities unidentified. Organizations face catastrophic operational failures when critical asset dependencies are not understood during testing.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Testing scope has been defined (see INF-00.02)
- No technical access is required

## Test Objectives

- Verify explicit identification of business-critical assets within testing scope
- Confirm criticality criteria and classification methodology are documented
- Validate asset criticality ratings are based on business impact analysis
- Ensure critical asset dependencies and interconnections are mapped
- Confirm appropriate testing prioritization for critical assets

## Test Methodology

1. Review documentation for explicit identification of business-critical assets including:
   - Revenue-generating systems (e-commerce, billing, financial processing)
   - Operational technology and industrial control systems
   - Safety-critical systems
   - Regulatory compliance-dependent systems
   - Customer-facing services with SLA requirements
   - Core business process supporting systems
2. Verify criticality classification methodology is documented with measurable criteria:
   - Financial impact of unavailability (revenue loss per hour/day)
   - Operational impact metrics (processes affected, personnel impacted)
   - Regulatory and compliance consequences of compromise or unavailability
   - Reputational risk and customer impact
   - Recovery time objectives (RTO) and recovery point objectives (RPO)
3. Confirm business impact analysis (BIA) or equivalent assessment exists for critical assets:
   - Maximum tolerable downtime (MTD)
   - Data sensitivity and confidentiality requirements
   - Integrity requirements and data modification impacts
   - Availability requirements and redundancy provisions
4. Validate critical asset dependencies are documented:
   - Infrastructure dependencies (network, storage, compute)
   - Application dependencies (databases, authentication services, APIs)
   - Data flow dependencies and integration points
   - Supply chain and third-party service dependencies
5. Review testing prioritization and approach for critical assets:
   - Enhanced testing focus on critical asset security controls
   - Risk-appropriate testing constraints for critical systems
   - Testing timing considerations for critical asset availability windows
   - Contingency and rollback plans for critical asset testing
6. Confirm critical asset monitoring and incident response procedures:
   - Real-time monitoring during testing of critical assets
   - Defined incident response escalation for critical asset impacts
   - Business continuity plan activation criteria
7. Verify critical asset owner or responsible party identification for communication during testing
8. Document any critical assets excluded from testing due to risk intolerance

## Expected Secure Configuration

Organizations identifying business-critical assets should provide:

- Complete inventory of critical assets with unique identifiers
- Documented criticality classification methodology with quantitative criteria
- Business impact analysis results for each critical asset
- Dependency maps showing critical asset relationships and infrastructure requirements
- Asset ownership and responsibility documentation
- Testing prioritization matrix based on criticality and risk
- Special handling procedures for critical assets during testing
- Monitoring and incident response procedures specific to critical assets
- Recovery and continuity plans for critical asset failures
- Version-controlled critical asset documentation with change tracking

## Evidence to Collect

- List of business-critical assets with technical identifiers and business function descriptions
- Criticality classification criteria and methodology documentation
- Business impact analysis or risk assessment results for critical assets
- Dependency maps or architecture diagrams showing critical asset relationships
- Asset owner contact information for critical systems
- Testing prioritization and special handling procedures for critical assets
- Monitoring and incident response procedures documentation
- Communication records clarifying critical asset identification
- Risk acceptance documentation for critical assets excluded from testing
- Version and date of critical asset documentation

## Impact

**Business Continuity Impact:**
Unidentified critical assets may experience testing-induced failures without appropriate safeguards, causing major operational disruptions. Revenue-generating systems, financial processing, manufacturing operations, or customer-facing services may suffer unplanned outages resulting in significant financial losses and SLA violations.

**Security Posture Impact:**
Testing resources focused on non-critical assets while critical systems remain unassessed creates significant residual risk. Vulnerabilities in business-critical systems represent highest-impact attack targets. Failure to prioritize critical asset security testing leaves organizations exposed to attacks that cause maximum business damage.

**Regulatory and Compliance Impact:**
Systems processing regulated data or supporting compliance-critical functions require special testing considerations. Unidentified critical assets subject to PCI DSS, HIPAA, SOX, or other regulatory frameworks may be tested inappropriately, causing compliance violations, audit failures, and regulatory penalties.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

Note: Critical asset identification informs testing focus on techniques that impact high-value targets, particularly techniques in the Impact tactic (T1485-T1565) and Data Exfiltration (TA0010).

## Remediation Guidance

Organizations should implement the following critical asset identification practices:

1. **Asset Inventory and Classification:**
   - Maintain comprehensive asset inventory with criticality classifications
   - Implement automated discovery and inventory management systems
   - Document business function for each asset
   - Update asset criticality classifications based on business changes
   - Include cloud and hybrid infrastructure assets in inventory

2. **Business Impact Analysis:**
   - Conduct formal BIA for all in-scope systems
   - Define quantitative criteria for criticality classification
   - Document financial, operational, and regulatory impacts
   - Establish RTO and RPO for critical assets
   - Review and update BIA annually or when business processes change

3. **Dependency Mapping:**
   - Document technical and business dependencies for critical assets
   - Create visual dependency maps and architecture diagrams
   - Identify single points of failure in critical asset dependencies
   - Map data flows between critical systems
   - Document third-party and supply chain dependencies

4. **Testing Strategy for Critical Assets:**
   - Develop risk-appropriate testing approaches for critical systems
   - Implement additional safeguards for critical asset testing
   - Schedule testing during maintenance windows or low-impact periods
   - Establish enhanced monitoring during critical asset testing
   - Prepare rollback and recovery procedures before testing

5. **Governance and Communication:**
   - Assign critical asset owners with clear responsibilities
   - Establish communication protocols for critical asset testing
   - Define escalation procedures for critical asset incidents
   - Integrate critical asset identification with change management
   - Conduct regular critical asset identification reviews with business stakeholders
