# PCI-DSS Diagrams and Visual Documentation

**System:** CloudVault Federal Health Exchange (FHX)
**Repository:** cloudvault-pci-dss
**Folder:** diagrams
**Framework Reference:** PCI-DSS v4.0 Requirements 1.2.4, 12.5 / NIST SP 800-53 PL-8
**Document ID:** FHX-PCI-DIA-001
**Prepared By:** Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, Security+, CHP, CSCS, CISSP (in progress)
**Role:** ISSO / ISSM, CloudVault Federal Health Exchange
**Last Updated:** April 2025
**Status:** Active

---

## Purpose

PCI-DSS v4.0 Requirement 1.2.4 requires organizations to maintain an accurate and current network diagram that shows all connections to the CDE, including any wireless networks. This folder contains the FHX CDE boundary diagram, network segmentation diagram, and cardholder data flow diagram, all documented in narrative form with detailed descriptions suitable for audit review.

---

## Documents in This Folder

| Document | Description | Reference | Status |
|---|---|---|---|
| cde-boundary-diagram.md | CDE boundary diagram: all in-scope and connected components, network boundaries | PCI-DSS v4.0 Req 1.2.4 | Active |
| network-segmentation-diagram.md | Network segmentation diagram: VPC zones, Security Groups, NACLs, routing | PCI-DSS v4.0 Req 1.3 | Active |
| cardholder-data-flow-diagram.md | Cardholder data flow: from patient/agency to payment processor and back | PCI-DSS v4.0 Req 12.5.1 | Active |

---

## CDE Boundary Diagram Description

The CDE boundary encompasses the following components within the CloudVault FHX environment:

**Internet-Facing Layer:** AWS WAF (web application firewall) sits in front of all internet-facing CDE components, filtering malicious traffic. The AWS Application Load Balancer (ALB) distributes traffic to CDE components. Both are protected by AWS Shield Standard for DDoS mitigation.

**CDE Application Layer:** The FHX Payment API Gateway (running on Amazon API Gateway with Lambda backend) processes incoming payment transactions. The FHX Patient Portal payment section (iframe hosted by HealthPay Federal Solutions LLC) collects patient copayments through a PCI-compliant hosted payment page. Neither FHX component stores raw PAN data.

**CDE Data Layer:** The FHX Billing Database (RDS Aurora, CDE subnet) stores payment tokens, billing records, and transaction metadata. No raw cardholder data exists in this database. The database is accessible only from the CDE application layer via VPC endpoint.

**Payment Processor Integration:** HealthPay Federal Solutions LLC connects to FHX via AWS PrivateLink (dedicated private connection; no public internet transit). HealthPay handles PAN authorization and returns tokens to FHX.

**CDE Boundary:** The CDE boundary is enforced by AWS Security Groups and Network ACLs on the 10.1.10.0/24 CDE subnet. No traffic from outside the CDE is permitted except from: (1) the WAF-protected ALB for inbound payment transactions, and (2) the HealthPay PrivateLink for processor responses.

---

## Network Segmentation Diagram Description

FHX uses four VPC zones to implement effective network segmentation:

| Zone | CIDR | CDE Status | Connection to CDE |
|---|---|---|---|
| CDE VPC | 10.1.0.0/16 | In CDE | Self (internal traffic only) |
| Application VPC | 10.2.0.0/16 | Out of CDE | One-way via AWS Transit Gateway: App VPC can initiate payment requests to CDE; CDE cannot initiate connections back to App VPC |
| Management VPC | 10.3.0.0/16 | Out of CDE | No direct connection; management access via SSM Session Manager (no persistent CDE shell access) |
| Shared Services VPC | 10.4.0.0/16 | Out of CDE | DNS and logging access only; no CDE data transits Shared Services VPC |

Transit Gateway route tables enforce that only explicitly allowed routes exist between VPCs. No default routes are permitted. All inter-VPC traffic is logged to VPC Flow Logs.

---

## Cardholder Data Flow Description

The FHX cardholder data flow proceeds as follows:

**Patient Portal Payment Flow:**
1. Patient accesses payment page on the FHX patient portal (portal.fhx.gov/pay)
2. Browser loads HealthPay-hosted payment iframe (iframe source: healthpayfederal.com; hosted on HealthPay's PCI-compliant servers)
3. Patient enters card data directly into HealthPay iframe; card data never touches FHX servers
4. HealthPay authorizes payment and returns a payment token to FHX via API callback
5. FHX stores payment token and transaction metadata in the FHX Billing Database
6. FHX sends payment confirmation to patient and billing system

**Agency Billing Payment Flow:**
1. Agency billing system submits payment transaction to FHX Payment API Gateway via FHIR financial resource
2. FHX Payment API Gateway passes transaction to HealthPay via PrivateLink
3. HealthPay authorizes payment and returns token
4. FHX stores token in Billing Database and returns confirmation to agency billing system

**At no point in either flow does raw cardholder data (PAN, CVV2, expiry) traverse FHX-managed infrastructure beyond the WAF and API Gateway (transit only; not stored).**

---

## Related Documents

- [CDE Scope](../cde-scope/README.md)
- [Network Segmentation](../network-segmentation/README.md)
- [PCI-DSS Requirements](../requirements/README.md)
- [Vulnerability Scan Results](../scan-results/README.md)
- [SAQ](../saq/README.md)
- [FedRAMP Data Flow Diagram](https://github.com/enechi-njeze/cloudvault-fedramp-ato/blob/main/ssp-attachments/att-02-data-flow/README.md)

---

*Prepared by Enechi P.C. Njeze, CGRC, CISA, CISM, PMP, PMI-RMP, CompTIA Security+ SY0-701, CHP, CSCS, CISSP (in progress)*
*ISSO / ISSM, CloudVault Federal Health Exchange (FHX)*
*[LinkedIn](https://www.linkedin.com/in/enechi-njeze) | [Portfolio](https://github.com/enechi-njeze)*
