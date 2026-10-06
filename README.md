# NexaTech Solutions Pvt. Ltd. — Enterprise Network Design (Member 1)

## 📌 Project Overview
This repository contains the complete network design, infrastructure specifications, device inventories, and Cisco Packet Tracer blueprints for **NexaTech Solutions Pvt. Ltd.** (140 Employees, 3 Floors + Server Room).

* **Course**: Computer Networks & Data Communications (CNDC)
* **Assigned Role**: **Member 1 — Network Design Engineer**
* **Organization**: NexaTech Solutions Pvt. Ltd.

---

## 🗂️ Repository Structure

```
├── documents/
│   ├── Device_Inventory.docx             # Master hardware specification table & cabling standards (.docx)
│   ├── Project_Requirements_Final.docx   # Phase 1 common team planning document (.docx)
│   └── Member_1_Network_Design_Report.md # Full technical design report
├── packet_tracer/
│   ├── Packet_Tracer_Setup_Guide.md      # Step-by-step guide to assemble Base_Topology_v1.pkt
│   ├── Port_Connection_Matrix.csv        # Exact port-to-port physical cabling matrix
│   └── startup_configs/                  # Baseline CLI configuration scripts ready to paste
│       ├── R1_Edge_Router.txt            # Cisco 2911 ISR Edge Router
│       ├── FW_Perimeter_ASA.txt          # Cisco ASA 5506-X Firewall
│       ├── SW_Core_L3.txt                # Cisco Catalyst 3560 Core L3 Switch
│       ├── SW_Access_1_Floor1.txt        # Cisco Catalyst 2960 Floor 1 Switch
│       ├── SW_Access_2_Ground.txt        # Cisco Catalyst 2960 Ground Floor Switch
│       └── SW_Access_3_Floor2.txt        # Cisco Catalyst 2960 Floor 2 Switch
└── diagrams/
    ├── logical_topology.svg              # High-resolution vector logical network topology
    ├── physical_topology.svg             # High-resolution 3-floor physical building layout
    └── topology_diagrams.html            # Interactive dual-view topology viewer in browser
```

---

## 🏢 Enterprise Summary
* **Total Headcount**: 140 staff distributed across 3 floors.
* **Departments**:
  * Human Resources (HR): 15 staff (VLAN 10)
  * Finance: 20 staff (VLAN 20)
  * IT Engineering: 25 staff (VLAN 30)
  * Sales & Marketing: 30 staff (VLAN 40)
  * Customer Support: 40 staff (VLAN 50)
  * Executive Management: 10 staff (VLAN 60)
  * Guest Network: Isolated Wi-Fi (VLAN 70)
  * Central Server Farm: DHCP, DNS, Web, FTP (VLAN 100)

---

## ⚡ Cisco Packet Tracer Base File (`Base_Topology_v1.pkt`)
To construct the base file:
1. Open Cisco Packet Tracer and place devices per the inventory list.
2. Connect cables following `packet_tracer/Port_Connection_Matrix.csv`.
3. Open each device CLI and paste the respective script from `packet_tracer/startup_configs/`.
4. Save file as `Base_Topology_v1.pkt` and hand off to **Member 2** (Routing & IP Addressing Engineer).

---
*Created by Member 1 (Network Design Engineer).*
