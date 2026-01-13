# INF-00.07 — Identification of Regulatory and Compliance Requirements

## Summary

This test verifies that applicable regulatory frameworks, compliance obligations, and data protection requirements have been identified and documented to ensure testing activities maintain compliance and address relevant security controls.

## Risk

Failure to identify applicable regulatory requirements results in testing activities that violate compliance frameworks, leading to audit failures, regulatory penalties, and legal liability. Systems processing regulated data may be tested inappropriately, causing data protection violations, mandatory breach notifications, and loss of regulatory certifications. Testing that does not address compliance-relevant security controls fails to validate organizational adherence to regulatory obligations, creating false assurance and exposing organizations to enforcement actions.

## Preconditions

- Written authorization document exists (see INF-00.01)
- Testing scope has been defined (see INF-00.02)
- Business-critical assets have been identified (see INF-00.06)
- No technical access is required

## Test Objectives

- Verify identification of all applicable regulatory and compliance frameworks
- Confirm systems processing regulated data are identified within testing scope
- Validate testing approach addresses compliance-relevant security controls
- Ensure testing activities maintain compliance with data protection requirements
- Confirm compliance reporting requirements are documented

## Test Methodology

1. Review organizational documentation to identify applicable regulatory frameworks including but not limited to:
   - **Payment Card Industry Data Security Standard (PCI DSS)** for payment card data
   - **Health Insurance Portability and Accountability Act (HIPAA)** for protected health information
   - **General Data Protection Regulation (GDPR)** for EU personal data
   - **Sarbanes-Oxley Act (SOX)** for financial reporting systems
   - **Federal Information Security Management Act (FISMA)** for federal systems
   - **Network and Information Security Directive 2 (NIS2)** for critical infrastructure in EU
   - **ISO 27001** certification requirements
   - **NIST Cybersecurity Framework** adoption
   - Industry-specific regulations (financial services, healthcare, energy sector)
2. Verify systems processing regulated data types are identified:
   - Payment card data (PAN, CVV, cardholder information)
   - Protected Health Information (PHI)
   - Personally Identifiable Information (PII)
   - Financial records subject to SOX
   - Critical infrastructure operational data
   - Classified or controlled unclassified information (government systems)
3. Confirm compliance-relevant security controls are included in testing scope:
   - Access controls and authentication mechanisms
   - Encryption and data protection measures
   - Logging and monitoring capabilities
   - Network segmentation and isolation
   - Vulnerability management processes
   - Incident response procedures
4. Review data handling requirements for testing activities:
   - Restrictions on data exfiltration or copying
   - Data masking or anonymization requirements for test data
   - Encryption requirements for data in transit and at rest during testing
   - Data retention and destruction requirements for testing artifacts
   - Cross-border data transfer restrictions
5. Validate testing approach maintains compliance:
   - Testing does not violate regulatory data protection requirements
   - Testing personnel meet background check or clearance requirements
   - Testing activities do not trigger mandatory breach notification thresholds
   - Testing documentation meets compliance audit evidence requirements
6. Confirm compliance reporting requirements are documented:
   - Testing results format and content for compliance audits
   - Mapping of findings to specific regulatory control requirements
   - Timeline for compliance reporting deliverables
   - Regulatory audit coordination procedures
7. Review any specific regulatory constraints on testing methods:
   - Restrictions on vulnerability exploitation in regulated environments
   - Requirements for regulatory body notification of testing activities
   - Limitations on social engineering or phishing in certain industries
8. Document compliance certifications that may be affected by testing or findings

## Expected Secure Configuration

Organizations identifying regulatory requirements should provide:

- Complete list of applicable regulatory frameworks with jurisdiction and scope
- Identification of systems and data subject to regulatory requirements
- Mapping of compliance-relevant security controls to testing scope
- Data handling and protection requirements for testing activities
- Compliance reporting requirements and deliverable specifications
- Regulatory constraints on testing methods and techniques
- Background check or clearance requirements for testing personnel
- Breach notification thresholds and incident reporting procedures
- Compliance audit coordination procedures
- Version-controlled compliance documentation with change tracking

## Evidence to Collect

- List of applicable regulatory frameworks and compliance obligations
- Identification of systems processing regulated data types
- Mapping of compliance controls to testing scope
- Data handling requirements and restrictions documentation
- Compliance reporting requirements and templates
- Testing method constraints imposed by regulations
- Personnel clearance or background check documentation
- Breach notification thresholds and procedures
- Communication records clarifying compliance requirements
- Version and date of compliance requirements documentation

## Impact

**Regulatory Penalty Impact:**
Non-compliance with regulatory requirements results in enforcement actions, monetary penalties, and sanctions. PCI DSS violations lead to increased transaction fees and potential loss of payment processing capabilities. HIPAA violations incur fines up to millions of dollars. GDPR violations result in fines up to 4% of global annual revenue. Regulatory penalties create significant financial and reputational damage.

**Certification and Audit Impact:**
Testing that violates compliance requirements causes audit failures, loss of certifications, and required re-assessments. Organizations may lose ISO 27001, SOC 2, or FedRAMP certifications, blocking business opportunities and customer contracts. Compliance audit failures require costly remediation and re-certification efforts.

**Operational and Business Impact:**
Failure to validate compliance-relevant controls leaves regulatory gaps unidentified. Organizations face compliance violations from unknown security control deficiencies. Business operations dependent on regulatory compliance may be suspended during enforcement proceedings. Customer trust and business relationships suffer when compliance violations become public.

## MITRE ATT&CK Mapping

- Not Applicable (Governance / Pre-engagement control)

Note: Regulatory requirements influence testing of specific MITRE ATT&CK techniques, particularly those affecting regulated data: Collection (TA0009), Exfiltration (TA0010), and Impact (TA0040) tactics.

## Remediation Guidance

Organizations should implement the following regulatory compliance identification practices:

1. **Compliance Framework Identification:**
   - Conduct comprehensive review of applicable regulations based on industry, geography, and data types
   - Maintain current list of regulatory obligations with jurisdiction and applicability scope
   - Subscribe to regulatory updates and amendments
   - Document rationale for determined regulatory applicability or non-applicability
   - Review regulatory obligations annually or when business activities change

2. **Data Classification and Mapping:**
   - Implement data classification scheme aligned with regulatory definitions
   - Tag systems and assets with applicable regulatory frameworks
   - Map data flows for regulated data types
   - Document data protection requirements for each regulatory framework
   - Maintain data inventory showing regulated data locations and processing

3. **Compliance Control Mapping:**
   - Map security controls to specific regulatory requirements
   - Identify which controls are compliance-critical versus general security
   - Document testing requirements for each compliance control
   - Establish testing frequency based on regulatory audit cycles
   - Maintain traceability matrix between controls, tests, and regulations

4. **Compliance-Aware Testing Approach:**
   - Develop testing procedures that maintain regulatory compliance
   - Implement data protection measures for handling regulated data during testing
   - Establish personnel clearance verification for testing regulated systems
   - Define acceptable testing methods within regulatory constraints
   - Document compliance rationale for testing decisions

5. **Compliance Reporting Integration:**
   - Develop compliance reporting templates aligned with regulatory audit requirements
   - Map testing findings to specific regulatory control deficiencies
   - Establish timeline for compliance deliverable production
   - Integrate testing results with compliance management systems
   - Maintain audit trail documentation meeting regulatory evidence requirements
