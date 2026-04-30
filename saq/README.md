# Self-Assessment Questionnaire (SAQ-D)

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** saq
**Framework Reference:** PCI-DSS v4.0 SAQ-D for Merchants and Service Providers
**Document ID:** FHX-PCI-SAQ-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

The PCI-DSS Self-Assessment Questionnaire (SAQ) is a validation tool that allows eligible merchants and service providers to self-certify their compliance with PCI-DSS requirements. CloudVault FHX uses the SAQ-D form, which is applicable to all merchants and service providers that do not qualify for other SAQ types. SAQ-D covers all 12 PCI-DSS requirements and all applicable sub-requirements.

The SAQ is completed annually by the ISSO in collaboration with the DevOps Lead, Privacy Officer, and System Owner. The completed SAQ is reviewed by FHX legal counsel and submitted to the acquiring bank (Treasury Financial Agent) and payment brand (Visa Federal Government Program). The SAQ is supported by the evidence packages documented throughout this repository.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| saq-d-fy2025.md | Completed SAQ-D for FY2025 compliance period | PCI-DSS v4.0 SAQ-D | Active |
| attestation-of-compliance.md | Attestation of Compliance (AOC) signed by executive management | PCI-DSS v4.0 AOC | Active |
| saq-supporting-evidence-index.md | Index linking each SAQ question to supporting evidence | PCI-DSS v4.0 SAQ-D | Active |
| compensating-controls-worksheet.md | Documentation for any compensating controls used in lieu of defined approach | PCI-DSS v4.0 | Active |

---

## SAQ-D Summary: FY2025 Compliance Assessment

### Assessment Information

| Field | Value |
|---|---|
| Entity Name | CloudVault Federal Health Exchange (FHX) |
| Entity Type | Service Provider (federal government) |
| SAQ Type | SAQ-D |
| Assessment Period | January 1, 2025 to December 31, 2025 (in progress) |
| Primary Contact | Enechi P.C. Njeze, ISSO/ISSM |
| Assessment Date | April 30, 2025 (partial year; full assessment December 2025) |
| PCI-DSS Version | v4.0 |
| Assessor | Self-Assessment (supported by ClearPath Security Assessors LLC) |

### SAQ-D Compliance Summary (Mid-Year Status)

| Requirement | Short Title | Status | Notes |
|---|---|---|---|
| 1 | Network Security Controls | In Place | All Security Groups, NACLs, WAF controls operational |
| 2 | Secure Configurations | In Place | CIS Benchmark hardening, IaC enforcement, no defaults |
| 3 | Protect Stored Account Data | In Place | Tokenization; no raw PAN stored in FHX systems |
| 4 | Protect CHD in Transmission | In Place | TLS 1.3 minimum; certificate management active |
| 5 | Malware Protection | In Place | GuardDuty, EDR, email filtering, CI/CD scanning |
| 6 | Secure Systems and Software | In Place | SDLC, SAST/DAST, dependency scanning, patch management |
| 7 | Restrict Access by Business Need | In Place | RBAC, least privilege, quarterly reviews |
| 8 | User Identification and Authentication | In Place | MFA, PIV/CAC, unique IDs, no shared accounts |
| 9 | Physical Access Restrictions | In Place | Inherited from AWS GovCloud FedRAMP High |
| 10 | Logging and Monitoring | In Place | CloudTrail, CloudWatch, GuardDuty, daily log review |
| 11 | Security Testing | In Progress | Q1 2025 ASV scan complete; Q2 scan scheduled April 2025 |
| 12 | Organizational Policies and Programs | In Place | Security policy, training, vendor management, risk assessment |

**Overall Mid-Year Status:** 11 of 12 requirements fully in place. Requirement 11 in progress (routine scheduled scanning, not a deficiency).

---

## Compensating Controls

No compensating controls are used for FY2025. All requirements are met through defined approach implementations.

---

## SAQ Submission History

| Year | SAQ Type | Submission Date | Status | Receiving Party |
|---|---|---|---|---|
| FY2023 | SAQ-D | December 15, 2023 | Accepted | Treasury Financial Agent |
| FY2024 | SAQ-D | December 15, 2024 | Accepted | Treasury Financial Agent |
| FY2025 | SAQ-D | December 15, 2025 (target) | In Progress | Treasury Financial Agent |

---

## Attestation of Compliance

The FHX Attestation of Compliance (AOC) for the most recent completed assessment period (FY2024) was signed by:

- **Executive Sponsor:** Deputy Director Henry Kline (AO)
- **System Owner:** Dr. Patricia Owens
- **ISSO:** Enechi P.C. Njeze

The FY2025 AOC will be signed upon completion of the full-year assessment in December 2025.

---

## Related Documents

- [CDE Scope](../cde-scope/README.md)
- [PCI-DSS Requirements](../requirements/README.md)
- [Network Segmentation](../network-segmentation/README.md)
- [Vulnerability Scan Results](../scan-results/README.md)
- [PCI Diagrams](../diagrams/README.md)
- [FedRAMP Authorization Package](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
