# Network Segmentation Controls

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** network-segmentation
**Framework Reference:** PCI-DSS v4.0 Requirements 1.3, 11.4 / NIST SP 800-53 SC-7
**Document ID:** FHX-PCI-SEG-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

Network segmentation is one of the most important scope reduction and risk mitigation strategies in a PCI-DSS program. While not required by PCI-DSS, effective network segmentation that isolates the CDE from all other networks reduces the number of system components subject to full PCI-DSS requirements, reduces the risk of compromise, and simplifies the annual assessment.

For CloudVault FHX, robust network segmentation is implemented through AWS VPC architecture, Security Groups, Network ACLs, and strict routing controls. This folder documents the segmentation architecture, the controls that enforce segmentation, the annual segmentation validation test results, and the controls that detect segmentation failures.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| segmentation-architecture.md | Full description of CDE segmentation architecture in AWS GovCloud | PCI-DSS v4.0 Req 1.3 | Active |
| segmentation-controls.md | Security Group rules, NACL rules, and routing controls enforcing segmentation | PCI-DSS v4.0 Req 1.3.1 | Active |
| segmentation-test-results.md | Annual penetration test results validating segmentation effectiveness | PCI-DSS v4.0 Req 11.4.5 | Active |
| segmentation-monitoring.md | Controls that detect unauthorized traffic to or from the CDE | PCI-DSS v4.0 Req 10.6 | Active |

---

## CDE Network Architecture

The FHX network is divided into four VPC zones in AWS GovCloud (us-gov-west-1):

| VPC Zone | CIDR | Purpose | CDE Status |
|---|---|---|---|
| FHX CDE VPC | 10.1.0.0/16 | Hosts all CDE components: Payment API Gateway, patient portal payment section, billing database (tokens only) | In CDE |
| FHX Application VPC | 10.2.0.0/16 | Hosts FHIR exchange platform, clinical records, non-payment APIs | Out of CDE |
| FHX Management VPC | 10.3.0.0/16 | Hosts bastion/jump host, monitoring, DevOps pipeline | Connected (out of CDE) |
| FHX Shared Services VPC | 10.4.0.0/16 | Hosts DNS, Active Directory, shared logging | Connected (out of CDE) |

**VPC Peering:** No direct VPC peering exists between the FHX CDE VPC and any other VPC. Inter-VPC communication occurs only through AWS Transit Gateway with strict route table controls and Security Group restrictions. CDE components can only receive traffic from: (1) the payment processor integration endpoint (HealthPay Federal Solutions LLC via dedicated private link), and (2) the WAF-protected API Gateway in the Application VPC for payment initiation.

---

## Segmentation Controls

### AWS Security Groups (CDE Inbound Rules)

The following inbound rules apply to the CDE Payment API Gateway Security Group:

| Rule | Source | Protocol | Port | Purpose |
|---|---|---|---|---|
| Allow | FHX Application VPC NAT Gateway SG | TCP | 443 | Payment initiation API calls from FHX patient portal |
| Allow | HealthPay Federal Solutions LLC ASN (private link) | TCP | 443 | Payment processor responses |
| Deny | All other sources | All | All | Default deny all |

### Network ACLs (CDE Subnet)

| Rule | Direction | Source/Destination | Action |
|---|---|---|---|
| 100 | Inbound | FHX Application VPC NAT Gateway (10.2.255.0/24) | Allow TCP 443 |
| 110 | Inbound | HealthPay private link (10.5.0.0/24) | Allow TCP 443 |
| 32767 | Inbound | All | Deny |
| 100 | Outbound | HealthPay private link (10.5.0.0/24) | Allow TCP 443 |
| 110 | Outbound | FHX Application VPC (10.2.0.0/16) | Allow TCP 443 (responses) |
| 32767 | Outbound | All | Deny |

---

## Segmentation Validation

Per PCI-DSS v4.0 Requirement 11.4.5, segmentation controls must be validated through penetration testing at least annually, and after any significant infrastructure change.

**FY2025 Annual Segmentation Test:**

| Test | Date | Result | Performed By |
|---|---|---|---|
| External to CDE penetration test | February 2025 | No unauthorized paths to CDE identified | ClearPath Security Assessors LLC |
| Internal segmentation test (FHX Application VPC to CDE) | February 2025 | All traffic correctly blocked by Security Groups and NACLs; only authorized paths allowed | ClearPath Security Assessors LLC |
| Internal segmentation test (FHX Management VPC to CDE) | February 2025 | No direct management access to CDE; all management through authorized jump host with MFA | ClearPath Security Assessors LLC |
| Wireless segmentation validation | February 2025 | No wireless networks exist in CDE; AWS GovCloud physical facilities do not allow wireless in data center areas | ClearPath Security Assessors LLC |

**Conclusion:** The FHX CDE is effectively isolated from all non-CDE networks. No unauthorized access paths were identified.

---

## Segmentation Monitoring

The following automated controls monitor for segmentation failures or unauthorized traffic to the CDE:

| Control | Implementation | Alert Threshold | Response |
|---|---|---|---|
| VPC Flow Log analysis | CloudWatch Logs Insights analyzes VPC Flow Logs every 5 minutes for traffic to CDE from unauthorized sources | Any traffic from non-whitelisted source | ISSO PagerDuty alert within 5 minutes; SOC investigation immediately |
| Security Group change alerting | AWS Config rule detects any Security Group modification affecting CDE subnet | Any change | Change must have approved Change Request; unauthorized changes trigger P1 incident |
| NACL change alerting | AWS Config rule detects any NACL modification affecting CDE subnet | Any change | Same as Security Group changes |
| GuardDuty network threat detection | Continuous ML-based analysis of network traffic patterns in CDE VPC | Any GuardDuty finding of MEDIUM or higher | Automated alert to ISSO; triage within 1 hour |

---

## Related Documents

- [CDE Scope](../cde-scope/README.md)
- [PCI-DSS Requirements](../requirements/README.md)
- [Vulnerability Scan Results](../scan-results/README.md)
- [PCI Diagrams](../diagrams/README.md)
- [FedRAMP Network Architecture](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/ssp-attachments/att-02-data-flow/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
