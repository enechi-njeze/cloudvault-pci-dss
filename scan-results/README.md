# Vulnerability Scan Results

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** scan-results
**Framework Reference:** PCI-DSS v4.0 Requirements 11.3 and 11.4
**Document ID:** FHX-PCI-SCN-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

PCI-DSS v4.0 Requirement 11 requires regular testing of the security of systems and networks within and connected to the CDE. This includes: quarterly external vulnerability scans conducted by a PCI SSC-approved ASV (Approved Scanning Vendor), quarterly internal vulnerability scans, and an annual penetration test that includes testing of the CDE and segmentation controls.

This folder documents all FHX vulnerability scan results, ASV scan attestations, internal scan reports, and penetration test findings for the current PCI assessment period. All high and critical vulnerabilities identified in CDE components must be remediated before the next quarterly scan.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| asv-q1-2025.md | Q1 2025 ASV scan results and remediation status | PCI-DSS v4.0 Req 11.3.2 | Active |
| asv-q2-2025.md | Q2 2025 ASV scan results (scheduled April 2025) | PCI-DSS v4.0 Req 11.3.2 | Scheduled |
| internal-scan-q1-2025.md | Q1 2025 internal vulnerability scan results | PCI-DSS v4.0 Req 11.3.1 | Active |
| pentest-fy2025.md | FY2025 annual penetration test results (February 2025) | PCI-DSS v4.0 Req 11.4 | Active |
| scan-remediation-tracker.md | Remediation tracker for all open vulnerability findings | PCI-DSS v4.0 Req 11.3.1.1 | Active |

---

## Scanning Program Overview

### External ASV Scans (Requirement 11.3.2)

External vulnerability scans of all CDE-facing internet-accessible IP addresses and domains are conducted quarterly by FHX's PCI SSC-approved ASV: TrustScan Federal LLC (ASV ID: 5000-TF).

**Scanned Assets:**

| Asset | IP / Domain | Protocol | Last Scan | Status |
|---|---|---|---|---|
| FHX Payment API Gateway | fhx-pay.fhx.gov | HTTPS (443) | January 2025 | Pass |
| FHX Patient Portal (payment section) | portal.fhx.gov/pay | HTTPS (443) | January 2025 | Pass |
| FHX CDE Load Balancer | 54.239.x.x (masked) | TCP 443 | January 2025 | Pass |

**Scan Cadence:**

| Quarter | Scan Date | Result | Findings | Status |
|---|---|---|---|---|
| Q1 2025 | January 15, 2025 | Pass | 2 Medium (remediated before next scan) | Complete |
| Q2 2025 | April 15, 2025 (scheduled) | Pending | TBD | Scheduled |
| Q3 2025 | July 15, 2025 (scheduled) | Pending | TBD | Scheduled |
| Q4 2025 | October 15, 2025 (scheduled) | Pending | TBD | Scheduled |

### Internal Vulnerability Scans (Requirement 11.3.1)

Internal vulnerability scans of all CDE components are conducted quarterly using Amazon Inspector and Tenable.io (FedRAMP authorized). Scans are run against all EC2 instances, containers, and databases within the CDE VPC.

**Q1 2025 Internal Scan Summary:**

| Severity | Count | Remediated | Open | Accepted Risk |
|---|---|---|---|---|
| Critical | 0 | 0 | 0 | 0 |
| High | 1 | 1 | 0 | 0 |
| Medium | 7 | 5 | 2 | 0 |
| Low | 14 | 9 | 5 | 0 |

The 1 High finding (CVE-2024-XXXX: OpenSSL buffer overflow in a deprecated library) was remediated within 72 hours via emergency patch deployment. The 2 open Medium findings are scheduled for remediation in the Q2 patch cycle (April 2025). The 5 open Low findings are tracked in the remediation tracker with target dates in Q3 2025.

### Annual Penetration Test (Requirement 11.4)

FHX's annual penetration test is conducted by ClearPath Security Assessors LLC, an independent qualified security assessor. The FY2025 penetration test was conducted in February 2025 and covered: external penetration testing of all CDE-facing assets, internal penetration testing of the CDE from inside the FHX Application VPC, segmentation validation testing, and application-layer testing of the Payment API Gateway.

**FY2025 Penetration Test Findings:**

| Finding ID | Severity | Description | Remediated | Date |
|---|---|---|---|---|
| PT-2025-001 | Medium | TLS session renegotiation enabled on Payment API Gateway (potential DoS risk) | Yes | March 2025 |
| PT-2025-002 | Low | HTTP security headers missing on patient portal payment page (non-PCI critical) | Yes | March 2025 |
| PT-2025-003 | Informational | Payment API returns verbose error messages in development-mode edge cases | In Progress | Target: May 2025 |

**Segmentation Test Result:** All segmentation controls were validated as effective. No paths from non-CDE networks to CDE components were identified.

---

## Related Documents

- [CDE Scope](../cde-scope/README.md)
- [Network Segmentation](../network-segmentation/README.md)
- [PCI-DSS Requirements](../requirements/README.md)
- [Self-Assessment Questionnaire](../saq/README.md)
- [Vulnerability Management Program](https://github.com/enechi-njeze/cloudvault-vuln-mgmt/blob/main/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
