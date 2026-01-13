# INF-03.08 — Service-to-Service Trust Abuse Possibilities

## Summary

This test identifies trust relationships between services, APIs, and applications that enable authentication bypass, privilege escalation, or lateral movement through service account credentials or authentication delegation.

## Risk

Service-to-service trust relationships create authentication pathways enabling compromise propagation across application tiers and infrastructure components. Applications trusting internal services without validation enable request spoofing and privilege escalation. Service accounts with excessive permissions across multiple systems provide lateral movement paths. API authentication using shared secrets or weak tokens enables service impersonation. Microservices communicating without mutual authentication allow unauthorized service interaction. Organizations face cascading compromise when attackers exploit service trust relationships to access downstream systems and data.

## Preconditions

- Written authorization exists and scope is defined (see INF-00.01, INF-00.02)
- Service discovery and enumeration completed (see INF-01.03)
- Authentication mechanisms identified (see INF-02.01)
- Testing includes service relationship analysis

## Test Objectives

- Identify trust relationships between services and applications
- Assess authentication and authorization between trusted services
- Test for service impersonation and authentication bypass
- Evaluate service account credential exposure and reuse
- Validate least privilege implementation in service communications

## Test Methodology

1. Enumerate service-to-service communication patterns:
   - Web applications to database servers
   - API gateways to backend services
   - Microservices inter-service communications
   - Application servers to authentication services
   - Monitoring systems to monitored services
2. Identify service authentication mechanisms:
   - API keys and tokens
   - Service principal names (SPNs) and Kerberos delegation
   - Mutual TLS (mTLS) certificates
   - Shared secrets and pre-shared keys
   - OAuth 2.0 client credentials
   - IP-based trust (source IP authentication)
3. Test for implicit trust and authentication bypass:
   - Verify backend services validate requests from frontend services
   - Test for IP-based trust enabling request spoofing
   - Attempt direct backend service access bypassing frontend authentication
   - Test for HTTP header-based authentication without validation
   - Assess internal service authentication requirements
4. Analyze service account credentials:
   - Identify service accounts used for inter-service authentication
   - Assess service account privilege levels across systems
   - Test for service account credential reuse
   - Verify service account password strength and rotation
   - Identify hardcoded service credentials in configurations
5. Test API authentication and authorization:
   - Enumerate API keys and their scope of access
   - Test for API key reuse across services
   - Verify API rate limiting and throttling
   - Test for authorization bypass through API manipulation
   - Assess API key rotation and lifecycle management
6. Evaluate microservice security:
   - Test for mutual TLS (mTLS) between microservices
   - Verify service mesh authentication and authorization
   - Assess service identity and certificate management
   - Test for unauthorized service registration
   - Verify service-to-service encryption
7. Test trust relationship exploitation paths:
   - Attempt service impersonation through credential theft
   - Test for privilege escalation through trusted service chains
   - Assess lateral movement capabilities via service accounts
   - Verify request validation and input sanitization at service boundaries
   - Test for server-side request forgery (SSRF) enabling trust abuse
8. Assess zero trust implementation:
   - Verify mutual authentication between all services
   - Test for least privilege service permissions
   - Assess explicit authorization at each service boundary
   - Verify request signing or token validation
   - Test for defense in depth with multiple validation layers

## Expected Secure Configuration

Organizations implementing service-to-service communications should maintain:

- Mutual TLS (mTLS) authentication between all services
- Service mesh or API gateway enforcing authentication and authorization
- Zero trust architecture with explicit verification at each service boundary
- Unique service identities using certificates or managed identities
- Principle of least privilege on service account permissions
- API keys with limited scope and regular rotation
- Request signing and validation preventing spoofing
- Elimination of IP-based trust without additional authentication
- Comprehensive logging of service-to-service communications
- Regular audit of service trust relationships and permissions

## Evidence to Collect

- Service-to-service communication map showing trust relationships
- Service authentication mechanism identification
- Service account credential analysis and privilege assessment
- Implicit trust testing results showing authentication bypasses
- API authentication and authorization testing results
- Microservice mutual authentication validation
- Service impersonation attempt results
- Privilege escalation path analysis through service trust
- Zero trust implementation assessment
- Service-to-service encryption verification

## Impact

**Lateral Movement and Privilege Escalation Impact:**
Compromised frontend service credentials enable access to backend databases and APIs. Service accounts with permissions across multiple tiers provide lateral movement paths. Attackers leverage trusted service relationships bypassing authentication at downstream services. Single compromised service leads to cascading access across application infrastructure.

**Data Access and Exfiltration Impact:**
Trust relationships enable unauthorized data access through service impersonation. Backend services trusting frontend requests without validation expose sensitive data. Excessive service account permissions provide broader data access than intended. Attackers abuse trusted service APIs for data exfiltration appearing as legitimate traffic.

**Authentication Bypass and Unauthorized Access Impact:**
IP-based trust enables request spoofing from compromised systems. Weak service authentication allows unauthorized service interaction. Hardcoded credentials in service configurations provide persistent authentication. Missing mutual authentication enables man-in-the-middle attacks between services.

## MITRE ATT&CK Mapping

- **T1550** - Use Alternate Authentication Material (service credentials)
- **T1078.003** - Valid Accounts: Local Accounts (service account abuse)
- **T1021** - Lateral Movement: Remote Services
- **T1550.001** - Use Alternate Authentication Material: Application Access Token
- **T1557** - Man-in-the-Middle (service communication interception)

## Remediation Guidance

Organizations should implement the following service trust security practices:

1. **Zero Trust Service Architecture:**
   - Implement explicit authentication and authorization at every service boundary
   - Eliminate implicit trust between services
   - Require mutual authentication for all service communications
   - Deploy service mesh enforcing zero trust policies (Istio, Linkerd, Consul Connect)
   - Verify every request regardless of source
   - Implement defense in depth with multiple validation layers

2. **Mutual TLS (mTLS) Implementation:**
   - Deploy mutual TLS for all service-to-service communications
   - Use certificate-based service identity
   - Implement automated certificate lifecycle management
   - Rotate service certificates regularly (90 days maximum)
   - Validate certificates and certificate chains on each connection
   - Use service mesh or API gateway providing mTLS enforcement

3. **Service Identity and Authentication:**
   - Implement unique service identities using certificates or managed identities
   - Deploy Azure Managed Identity, AWS IAM Roles, or Google Service Accounts
   - Eliminate shared service credentials
   - Use short-lived tokens with explicit expiration
   - Implement OAuth 2.0 client credentials flow for API authentication
   - Deploy SPIFFE/SPIRE for service identity in heterogeneous environments

4. **API Security Hardening:**
   - Implement API keys with limited scope and permissions
   - Deploy API key rotation (90 days maximum)
   - Use API gateway enforcing authentication and rate limiting
   - Implement request signing preventing tampering
   - Deploy API versioning isolating security improvements
   - Monitor API usage patterns detecting anomalous behavior

5. **Service Account Management:**
   - Apply principle of least privilege to all service accounts
   - Grant minimum permissions required per service
   - Use separate service accounts per service and tier
   - Eliminate service account reuse across security boundaries
   - Implement service account credential rotation
   - Remove hardcoded credentials from configurations
   - Deploy secrets management for service credentials

6. **Request Validation and Authorization:**
   - Validate all requests at service boundaries regardless of source
   - Implement input validation and sanitization
   - Deploy authorization checks on every service operation
   - Use attribute-based access control (ABAC) or policy-based authorization
   - Prevent server-side request forgery (SSRF) enabling trust abuse
   - Log authorization decisions for audit and anomaly detection
