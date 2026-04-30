# PCI-DSS v4.0 Requirements Documentation

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** requirements
**Framework Reference:** PCI-DSS v4.0 (March 2024) - All 12 Requirements
**Document ID:** FHX-PCI-REQ-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

This folder contains the detailed implementation documentation for all 12 PCI-DSS v4.0 requirements as they apply to the CloudVault FHX Cardholder Data Environment (CDE). PCI-DSS v4.0, published by the PCI Security Standards Council in March 2022 and effective March 2024, introduces a new customized implementation approach alongside the traditional defined approach. FHX uses the defined approach for all requirements.

For each requirement, this folder documents: the requirement description, the specific FHX controls that satisfy it, implementation evidence, responsible parties, and testing guidance. This documentation supports both internal compliance reviews and the annual Self-Assessment Questionnaire (SAQ-D) completion.

---

## Documents in This Folder

| Document | Description | PCI Requirement | Status |
|---|---|---|---|
| req-01-network-controls.md | Network security controls: Security Groups, NACLs, WAF | Req 1 | Active |
| req-02-secure-configs.md | Secure configurations: CIS hardening, Terraform IaC, default password elimination | Req 2 | Active |
| req-03-protect-stored-data.md | Protecting stored account data: tokenization, no CHD storage, encryption | Req 3 | Active |
| req-04-protect-transmission.md | Cryptography for transmission: TLS 1.3, no cleartext CHD | Req 4 | Active |
| req-05-malware-protection.md | Malware protection: GuardDuty, EDR, anti-malware controls | Req 5 | Active |
| req-06-secure-systems.md | Secure systems and software: SDLC, SAST/DAST, vulnerability management | Req 6 | Active |
| req-07-access-restriction.md | Restricting access by business need: RBAC, least privilege, need-to-know | Req 7 | Active |
| req-08-user-authentication.md | User identification and authentication: MFA, unique IDs, no shared accounts | Req 8 | Active |
| req-09-physical-access.md | Physical access restrictions: AWS GovCloud physical controls (inherited) | Req 9 | Active |
| req-10-logging-monitoring.md | Logging and monitoring: CloudTrail, CloudWatch, GuardDuty, log review | Req 10 | Active |
| req-11-security-testing.md | Security testing: ASV scans, penetration testing, internal scans | Req 11 | Active |
| req-12-policies-programs.md | Organizational policies and programs: security policy, training, vendor management | Req 12 | Active |

---

## PCI-DSS v4.0 Requirements Compliance Status

| Req | Title | FHX Implementation Summary | Status |
|---|---|---|---|
| 1 | Network security controls | AWS Security Groups, NACLs, WAF, and dedicated CDE VPC subnet; all CDE traffic logged to VPC Flow Logs | Compliant |
| 2 | Secure configurations | CIS Benchmark Level 2 hardening applied via AWS Systems Manager; Terraform IaC prevents configuration drift; no default credentials in production | Compliant |
| 3 | Protect stored account data | PAN never stored in FHX systems; tokenization by HealthPay Federal Solutions LLC; service codes and expiry not stored post-authorization | Compliant |
| 4 | Protect CHD in transmission | TLS 1.3 minimum for all payment data in transit; TLS 1.2 and below blocked at API Gateway; certificate validity monitored by AWS Certificate Manager | Compliant |
| 5 | Protect against malware | Amazon GuardDuty for threat detection; endpoint EDR on all CDE-connected workstations; email filtering for phishing; automated malware scan on all code in CI/CD pipeline | Compliant |
| 6 | Develop and maintain secure systems | Secure SDLC with SAST (Checkmarx), DAST (OWASP ZAP), dependency scanning (OWASP Dependency-Check); vulnerability patching per policy; no known exploitable vulnerabilities in CDE | Compliant |
| 7 | Restrict access by business need | IAM roles implement need-to-know for all CDE access; quarterly access reviews; privileged access requires dual approval; role documentation maintained | Compliant |
| 8 | Identify users and authenticate | MFA required for all CDE access; PIV/CAC for federal users; unique user IDs enforced; no shared accounts; passwords minimum 12 characters, MFA required | Compliant |
| 9 | Restrict physical access | AWS GovCloud data center physical controls inherited via FedRAMP High authorization; no physical media containing CHD exists outside AWS | Compliant |
| 10 | Log and monitor all access | CloudTrail data events logged for all CDE S3 access and API calls; CloudWatch alarms on anomalous patterns; daily log review; 12-month log retention | Compliant |
| 11 | Test security regularly | Quarterly ASV external vulnerability scans; quarterly internal vulnerability scans; annual penetration test of CDE and segmentation controls; weekly file integrity monitoring | In Progress (Q2 ASV scan scheduled) |
| 12 | Support security with policies | FHX Information Security Policy (FHX-POL-001) reviewed annually; security awareness training for all CDE personnel; BAA/service provider agreements maintained; annual risk assessment | Compliant |

**Overall PCI-DSS v4.0 Status:** 11 of 12 requirements fully compliant. Requirement 11 in progress pending Q2 2025 ASV scan (scheduled April 2025).

---

## Key PCI-DSS v4.0 Implementation Controls

### Tokenization (Requirement 3)

CloudVault FHX eliminates the risk of stored PAN data through tokenization. When a patient or agency submits payment, the FHX Payment API Gateway passes the transaction to HealthPay Federal Solutions LLC (PCI-DSS Level 1 Service Provider, validated 2024). HealthPay handles PAN authorization and returns a payment token to FHX. The token is meaningless if intercepted and cannot be reverse-engineered to recover the PAN. FHX stores only the token in the FHX clinical billing database; no raw PAN is ever written to an FHX-managed storage system.

### Encryption in Transit (Requirement 4)

All payment data in transit uses TLS 1.3. FHX API Gateway is configured to reject any connection negotiating TLS 1.2 or below. AWS Certificate Manager manages all TLS certificates and automatically renews them before expiry. Certificate transparency monitoring is enabled; unexpected certificate issuance for FHX domains triggers an alert to the ISSO within 1 hour.

### Monitoring and Alerting (Requirement 10)

All access to CDE components is logged to CloudTrail and CloudWatch Logs. Log retention is 12 months (CloudWatch: 3 months, then archived to S3). Daily log review is performed by the SOC. GuardDuty provides continuous behavioral analysis and alerts on payment-specific threat indicators. Any access to CHD outside of authorized business processes triggers an automated alert to the ISSO within 5 minutes.

---

## Related Documents

- [CDE Scope](../cde-scope/README.md)
- [Network Segmentation](../network-segmentation/README.md)
- [Vulnerability Scan Results](../scan-results/README.md)
- [Self-Assessment Questionnaire](../saq/README.md)
- [FedRAMP SSP](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/README.md)
- [HIPAA Safeguards](https://github.com/enechi-njeze/cloudvault-hipaa/blob/main/safeguards/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
