# cloudvault-pci-dss

![Framework](https://img.shields.io/badge/Framework-PCI--DSS%20v4.0-blue)
![Standard](https://img.shields.io/badge/Standard-12%20Requirements-orange)
![FedRAMP](https://img.shields.io/badge/FedRAMP-High-red)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![Cert](https://img.shields.io/badge/Cert-CISA-green)

## System Overview

**System Name:** CloudVault Federal Health Exchange (FHX)
**System Owner:** Dr. Patricia Owens
**ISSO / ISSM:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**AO:** Deputy Director Henry Kline
**CISO:** Angela Torres
**Privacy Officer:** Sandra Voss
**DevOps Lead:** Trevor L. Abrams
**Classification:** UNCLASSIFIED // CUI
**Impact Level:** HIGH (FIPS 199) | FedRAMP High (421 controls)
**Cloud Platform:** AWS GovCloud (US-West)
**Last Updated:** April 2025

---

## Purpose

CloudVault FHX processes payment card data in limited circumstances: federal health program billing transactions and patient copayment processing for participating agencies. While FHX is not a primary payment processor, these functions trigger PCI-DSS applicability for the Cardholder Data Environment (CDE) components.

This repository documents the FHX PCI-DSS v4.0 compliance program: the scoping of the CDE, implementation of all 12 PCI-DSS requirements within scope, network segmentation controls that reduce CDE scope, vulnerability scanning results, and the Self-Assessment Questionnaire (SAQ). For federal systems, PCI-DSS compliance exists alongside FedRAMP and FISMA obligations; this program integrates all three frameworks.

---

## Repository Structure

```
cloudvault-pci-dss/
├── cde-scope/              # Cardholder Data Environment scoping documentation
├── requirements/           # PCI-DSS v4.0 requirements documentation (all 12)
├── network-segmentation/   # Network segmentation controls reducing CDE scope
├── scan-results/           # Quarterly ASV scans and internal vulnerability scans
├── saq/                    # Self-Assessment Questionnaire (SAQ-D)
├── diagrams/               # CDE boundary diagram and network segmentation diagram
└── README.md               # This file
```

---

## Deliverables

| Document | Folder | Framework Reference | Status |
|---|---|---|---|
| CDE Scoping Document | cde-scope/ | PCI-DSS v4.0 Req 12.5 | In Progress |
| PCI-DSS Requirements Documentation | requirements/ | PCI-DSS v4.0 Requirements 1-12 | In Progress |
| Network Segmentation Assessment | network-segmentation/ | PCI-DSS v4.0 Req 1.3, 11.4 | In Progress |
| ASV Scan Results (Quarterly) | scan-results/ | PCI-DSS v4.0 Req 11.3 | In Progress |
| Internal Vulnerability Scan Results | scan-results/ | PCI-DSS v4.0 Req 11.3.1 | In Progress |
| Self-Assessment Questionnaire (SAQ-D) | saq/ | PCI-DSS v4.0 SAQ-D | In Progress |
| CDE Boundary and Network Diagrams | diagrams/ | PCI-DSS v4.0 Req 1.2 | In Progress |

---

## PCI-DSS v4.0 Requirements Coverage

| Requirement | Title | FHX Implementation Approach | Status |
|---|---|---|---|
| 1 | Install and maintain network security controls | AWS Security Groups, NACLs, WAF, VPC isolation of CDE | In Progress |
| 2 | Apply secure configurations to all system components | AWS Config, CIS Benchmark hardening, Terraform IaC | In Progress |
| 3 | Protect stored account data | Tokenization of PAN; AES-256 encryption for all stored cardholder data | In Progress |
| 4 | Protect cardholder data with strong cryptography during transmission | TLS 1.3 for all cardholder data in transit | In Progress |
| 5 | Protect all systems and networks from malicious software | AWS GuardDuty, endpoint EDR, email filtering | In Progress |
| 6 | Develop and maintain secure systems and software | Secure SDLC, SAST/DAST, dependency scanning | In Progress |
| 7 | Restrict access to system components and cardholder data by business need | IAM role-based access, least privilege, quarterly reviews | In Progress |
| 8 | Identify users and authenticate access to system components | MFA, PIV/CAC, unique user IDs, no shared accounts | In Progress |
| 9 | Restrict physical access to cardholder data | AWS GovCloud physical controls (inherited FedRAMP) | In Progress |
| 10 | Log and monitor all access to system components and cardholder data | CloudTrail, CloudWatch, GuardDuty, daily log review | In Progress |
| 11 | Test security of systems and networks regularly | Quarterly ASV scans, annual penetration test, internal scans | In Progress |
| 12 | Support information security with organizational policies and programs | FHX Information Security Policy, Security Awareness Training, BAAs | In Progress |

---

## Laws, Regulations, and Standards

| Standard | Applicability | FHX Implementation |
|---|---|---|
| PCI-DSS v4.0 (March 2024) | Payment card processing functions within FHX | Full SAQ-D compliance program for CDE components |
| NIST SP 800-53 Rev 5 | Federal baseline (421 controls, FedRAMP High) | PCI controls mapped to NIST families where applicable |
| FISMA 2014 | Federal information security management | FHX ATO maintained under FISMA; PCI integrated into annual assessment |
| HIPAA Security Rule | PHI processed alongside cardholder data | Dual compliance managed by ISSO; controls harmonized where possible |
| OMB Circular A-130 | Federal information resources management | PCI program supports A-130 information security program |

---

## Certifications

| Certification | Holder | Relevance |
|---|---|---|
| CGRC (Certified in Governance, Risk, and Compliance) | Enechi P.C. Njeze | GRC program design and compliance framework integration |
| CISA (Certified Information Systems Auditor) | Enechi P.C. Njeze | PCI-DSS audit methodology and control testing |
| CISM (Certified Information Security Manager) | Enechi P.C. Njeze | Security management and CDE control oversight |
| PMP (Project Management Professional) | Enechi P.C. Njeze | PCI compliance program management |
| PMI-RMP (Risk Management Professional) | Enechi P.C. Njeze | PCI risk assessment and prioritization |
| CompTIA Security+ SY0-701 | Enechi P.C. Njeze | Technical security control implementation |
| CHP (Certified HIPAA Professional) | Enechi P.C. Njeze | Dual HIPAA/PCI compliance program management |
| CSCS (Certified Security Compliance Specialist) | Enechi P.C. Njeze | Multi-framework compliance integration |
| CISSP (in progress) | Enechi P.C. Njeze | Broad security architecture and control knowledge |

---

## Related Repositories

- [cloudvault-fedramp-ato](https://github.com/enechi-njeze/cloudvault-fedramp-ato): FedRAMP ATO package and SSP
- [cloudvault-hipaa](https://github.com/enechi-njeze/cloudvault-hipaa): HIPAA compliance program
- [cloudvault-sox-itgc](https://github.com/enechi-njeze/cloudvault-sox-itgc): SOX 404 ITGC program
- [cloudvault-privacy](https://github.com/enechi-njeze/cloudvault-privacy): Privacy compliance (GDPR/CCPA)
- [cloudvault-vuln-mgmt](https://github.com/enechi-njeze/cloudvault-vuln-mgmt): Vulnerability management
- [cloudvault-master-controls](https://github.com/enechi-njeze/cloudvault-master-controls): Unified cross-framework controls

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
