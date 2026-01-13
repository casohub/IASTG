# IASTG - Infrastructure, Active Directory, and Linux Security Testing Guide

A comprehensive, modular security testing methodology for infrastructure, identity, and host-based environments.

## Overview

The IASTG (Infrastructure, Active Directory, and Linux Security Testing Guide) provides a structured, repeatable, and technology-agnostic methodology for security testing across:

- **Infrastructure (INF-STG):** Network, identity, and common infrastructure controls
- **Active Directory (AD-STG):** Identity-centric controls for Microsoft environments
- **Linux/Unix (LNX-STG):** Host-centric controls for Linux/Unix systems

Each module can be executed independently or combined for comprehensive assessments.

## Project Structure

```
IASTG/
├── document/
│   ├── 0-Infrastructure-Testing/
│   │   ├── 00-Pre-Engagement-Governance/
│   │   ├── 01-Network-Discovery-Mapping/
│   │   ├── 02-Authentication-Access-Controls/
│   │   ├── 03-Network-Services-Exposure/
│   │   ├── 04-Lateral-Movement-Pivoting/
│   │   ├── 05-Persistence-Post-Compromise/
│   │   └── 06-Defensive-Controls-Detection/
│   ├── 1-Active-Directory-Testing/
│   │   ├── 01-AD-Enumeration/
│   │   ├── 02-Credential-Exposure-Weaknesses/
│   │   ├── 03-Privilege-Escalation/
│   │   ├── 04-AD-Lateral-Movement/
│   │   ├── 05-Domain-Persistence/
│   │   ├── 06-AD-Certificate-Services/
│   │   └── 07-Trusts-Hybrid-Identity/
│   ├── 2-Linux-Unix-Testing/
│   │   ├── 01-Host-Enumeration/
│   │   ├── 02-Authentication-Secrets-Management/
│   │   ├── 03-Privilege-Escalation/
│   │   ├── 04-Lateral-Movement-Pivoting/
│   │   ├── 05-Persistence-Techniques/
│   │   └── 06-Logging-Hardening-Detection/
│   └── 99-Reporting-Closure/
└── assets/
```

## Module Status

### Infrastructure Testing (INF-STG)
- ✅ **INF-00:** Pre-Engagement & Governance (9 controls)
- ✅ **INF-01:** Network Discovery & Mapping (9 controls)
- ✅ **INF-02:** Authentication & Access Controls (9 controls)
- ✅ **INF-03:** Network Services & Exposure (9 controls)
- ✅ **INF-04:** Lateral Movement & Pivoting (9 controls)
- 🔄 **INF-05:** Persistence & Post-Compromise (In Progress)
- 📋 **INF-06:** Defensive Controls & Detection (Planned)

### Active Directory Testing (AD-STG)
- 📋 **AD-01 through AD-07:** (Planned - 63 controls)

### Linux/Unix Testing (LNX-STG)
- 📋 **LNX-01 through LNX-06:** (Planned - 54 controls)

### Reporting & Closure
- 📋 **IASTG-99:** (Planned - 9 controls)

## Usage

Each control is documented as a standalone chapter following a standardized template:

1. **Summary** - Brief description of what is tested
2. **Risk** - Security risks if control fails
3. **Preconditions** - Requirements to execute the test
4. **Test Objectives** - Specific validation goals
5. **Test Methodology** - Step-by-step testing procedures
6. **Expected Secure Configuration** - Baseline for pass/fail
7. **Evidence to Collect** - Required documentation
8. **Impact** - Consequences of control failure
9. **MITRE ATT&CK Mapping** - Relevant techniques
10. **Remediation Guidance** - Defensive recommendations

## Contributing

This is a living document. Contributions, feedback, and improvements are welcome.

## License

To be determined.

## Acknowledgments

Inspired by OWASP Testing Guide principles and structured for enterprise operational use.

