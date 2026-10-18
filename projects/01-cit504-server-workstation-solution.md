# 🖥️ Server & Workstation Solution for a Branch Office

**Course:** CIT 504, Introduction to Information Technology · Assessment 1

## Scenario
ABC Print Services is opening a new sales office in Taupō, staffed by four people. I evaluated server and workstation options for Domain Controller functionality and sales support, aligned to the managed-service model of its IT provider, ITSTW.

## What I did
1. **Identified requirements** for CPU, RAM, storage, operating system and networking. For each I explained the "half / recommended / double" options and why the recommended one fits.
2. **Researched and compared vendors**: four servers (HPE ProLiant and Dell PowerEdge) and four workstations (PB Tech custom builds and business-class HP/Dell).
3. **Made a recommendation** with a cost breakdown and risk reasoning.

## Key decisions

| Component | Server | Workstation |
|---|---|---|
| **CPU** | Quad-core Intel Xeon (ECC support for AD database integrity) | Quad-core Core i5 (12th gen+) or Ryzen 5 |
| **RAM** | 16–32 GB | 16 GB |
| **Storage** | 2 × 1 TB enterprise drives in **RAID 1**, plus backups (RAID is not a backup) | 512 GB NVMe SSD |
| **OS** | Windows Server 2022 Standard | Windows 11 Pro (domain join, Group Policy, BitLocker) |
| **Network** | Dual Gigabit NICs with NIC Teaming | Gigabit Ethernet + Wi-Fi 6 |

## Recommendation
- **Server:** HPE ProLiant ML110 Gen11, chosen for ECC memory, RAID redundancy and **iLO 6 remote management**. That suits a Hamilton-based support team looking after a remote site. Estimated total deployment (including firewall, UPS and setup) is about **NZ$6,700**. Alternative: Dell PowerEdge T140.
- **Workstation:** PB Tech Intel Core i5 custom build, about **NZ$2,200 per unit** including peripherals and Microsoft 365. Alternative: HP ProDesk 400 G9.

## Takeaways
- Right-sizing hardware to the workload (neither under- nor over-specifying) is a cost and reliability decision.
- Remote-management tooling reduces technician travel and fits managed-service delivery.
- Redundancy (RAID, dual NICs) supports business continuity but doesn't replace backups.

**Tools / topics:** Windows Server 2022 · Active Directory · DNS/DHCP · RAID · NIC teaming · Windows 11 Pro · vendor comparison · cost-benefit analysis

---

[⬅ Back to my profile](../README.md)
