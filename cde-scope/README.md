# Cardholder Data Environment (CDE) Scope

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** cde-scope
**Framework Reference:** PCI-DSS v4.0 Requirement 12.5 / PCI SSC Scoping Guidance
**Document ID:** FHX-PCI-CDE-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

Defining the Cardholder Data Environment (CDE) is the most critical step in any PCI-DSS compliance program. The CDE is the people, processes, and technology that store, process, or transmit cardholder data (CHD) or sensitive authentication data (SAD). Every component within the CDE scope is subject to all 12 PCI-DSS requirements. Components outside the CDE scope are still subject to PCI-DSS if they can impact the security of the CDE (connected components), but may be subject to fewer requirements.

Accurate scoping is a risk management discipline, not just an administrative exercise. Under-scoping leaves payment card data unprotected. Over-scoping wastes resources and creates audit complexity. For CloudVault FHX, which processes payment data in a limited but clearly defined set of functions, the scoping process must be rigorous and well-documented.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| cde-scope-document.md | Full CDE scoping analysis with system inventory, data flows, and scope determination for each component | PCI-DSS v4.0 Req 12.5.2 | Active |
| cde-network-diagram.md | CDE boundary network diagram narrative | PCI-DSS v4.0 Req 1.2.4 | Active |
| cardholder-data-inventory.md | Inventory of all locations where CHD is stored, processed, or transmitted | PCI-DSS v4.0 Req 12.5.1 | Active |
| scope-confirmation-letter.md | Annual scope confirmation signed by ISSO and System Owner | PCI-DSS v4.0 Req 12.5.2 | Active |

---

## Cardholder Data Processed by FHX

FHX processes the following cardholder data types in the context of federal health billing and patient copayment functions:

| Data Type | PCI Classification | FHX Processing | Storage | Encryption |
|---|---|---|---|---|
| Primary Account Number (PAN) | CHD | Processed during payment transactions; not stored post-authorization | Not stored (tokenized) | TLS 1.3 during transmission; tokenized at authorization |
| Cardholder Name | CHD | Captured with payment for billing association | Not stored (tokenized) | TLS 1.3 |
| Service Code | CHD | Processed during authorization | Not stored | TLS 1.3 |
| Expiration Date | CHD | Processed during authorization | Not stored | TLS 1.3 |
| CVV2/CVC2 | SAD | Never stored; processed only during authorization and immediately discarded per PCI requirement | Never stored | Processor handles; FHX never sees |
| Full magnetic stripe data | SAD | Never stored; handled entirely by payment processor gateway | Never stored | Payment processor handles |
| PIN/PIN Block | SAD | Not applicable; FHX does not process PIN-authenticated transactions | Not applicable | Not applicable |

**Key Scope Reduction Strategy:** FHX uses a PCI-compliant payment processor (HealthPay Federal Solutions LLC) that handles all PAN storage and authorization. FHX never stores raw PANs. FHX receives tokens from the payment processor, which are meaningless if intercepted. This tokenization approach dramatically reduces CDE scope by eliminating stored CHD from FHX systems.

---

## CDE System Inventory

| System Component | CDE Status | Justification | PCI Requirements |
|---|---|---|---|
| FHX Payment API Gateway | In CDE: CHD system | Receives payment transactions from agency billing systems; passes to payment processor | All 12 requirements |
| FHX Payment Processor Integration (HealthPay Federal Solutions LLC) | Connected: service provider | Receives CHD from FHX Payment API; handles authorization and tokenization | Requirements 1, 2, 8, 10, 12 (service provider management) |
| FHX Patient Portal (payment section) | In CDE: CHD system | Collects patient copayment via payment form; uses iframe hosted by HealthPay | All 12 requirements |
| FHX Clinical Records Database (RDS Aurora) | Connected: can impact CDE | Receives tokens from payment processor; stores billing tokens but no raw CHD | Requirements 1, 2, 6, 7, 8, 10, 12 |
| FHX IAM (AWS IAM) | Connected: can impact CDE | Controls access to CDE components | Requirements 7, 8, 10 |
| FHX CloudTrail Logs | Connected: can impact CDE | Logs access to CDE components | Requirements 10, 11 |
| FHX Core FHIR Exchange Platform | Out of scope | Does not process, store, or transmit CHD; network-segmented from CDE | None (maintain segmentation) |
| AWS GovCloud Infrastructure | Connected: inherited controls | Hosts all FHX components including CDE | FedRAMP High (inherited) |

---

## Network Segmentation Summary

FHX uses network segmentation to isolate CDE components from non-CDE components. The CDE is confined to a dedicated VPC subnet (10.1.10.0/24) with the following controls enforcing segmentation:

- AWS Security Group rules allow inbound traffic to CDE subnets only from the FHX Payment API Gateway and the Patient Portal payment iframe (specific source SGs)
- AWS Network Access Control Lists (NACLs) block all traffic from non-CDE VPC subnets to the CDE subnet
- VPC peering between CDE and non-CDE VPCs is prohibited; communication occurs only through API Gateway with strict authorization controls
- CloudWatch monitors for any unexpected traffic to or from the CDE subnet and alerts the ISSO within 5 minutes

The segmentation architecture is validated annually and after any significant infrastructure change. See [network-segmentation/README.md](../network-segmentation/README.md) for full segmentation documentation.

---

## Annual Scope Confirmation

The CDE scope is confirmed annually per PCI-DSS v4.0 Requirement 12.5.2. The most recent scope confirmation was completed on March 31, 2025 and was signed by the ISSO (Enechi P.C. Njeze) and System Owner (Dr. Patricia Owens). The scope confirmation concluded that: (1) all CHD locations have been identified and documented, (2) all CDE system components have been inventoried, (3) network segmentation controls are effective and validated by penetration testing, and (4) the CDE scope accurately reflects the current state of the FHX payment environment.

---

## Related Documents

- [PCI-DSS Requirements](../requirements/README.md)
- [Network Segmentation](../network-segmentation/README.md)
- [Vulnerability Scan Results](../scan-results/README.md)
- [Self-Assessment Questionnaire](../saq/README.md)
- [FedRAMP SSP: Network Architecture](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/ssp-attachments/att-02-data-flow/README.md)
- [HIPAA PHI Inventory](https://github.com/enechi-njeze/cloudvault-hipaa/blob/main/phi-inventory/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
