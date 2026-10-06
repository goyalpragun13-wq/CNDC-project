# CNDC Enterprise Network Design & Implementation
## Comprehensive Report Section: Member 1 — Network Design & Infrastructure
**Author / Role**: Member 1 — Network Design Engineer  
**Organization**: NexaTech Solutions Pvt. Ltd.  
**Academic Course**: Computer Networks and Data Communications (CNDC)  
**Deliverables Covered**: Company Scenario, Network Requirements, Device Inventory, Physical Topology, Logical Topology, Base Cisco Packet Tracer Blueprint (`Base_Topology_v1.pkt`)

---

## Executive Summary
This document represents the foundational engineering documentation for the campus network infrastructure of **NexaTech Solutions Pvt. Ltd.** As **Member 1 (Network Design Engineer)**, the primary mandate is designing and establishing the physical topology, logical network architecture, hardware inventory, and baseline Packet Tracer implementation. This foundation ensures that subsequent configurations by **Member 2** (IP Addressing, VLANs, and Inter-VLAN Routing) and **Member 3** (Enterprise Services, Security ACLs, and Validation) can be integrated seamlessly without architectural conflicts or bandwidth bottlenecks.

---

## 1. Company Scenario & Organizational Profile

### 1.1 Business Context & Growth Trajectory
**NexaTech Solutions Pvt. Ltd.** is an emerging information technology, cloud integration, and software consulting enterprise headquartered in a newly commissioned, three-story modern office facility. The company operates cross-functional engineering, commercial, support, and administrative teams comprising **140 full-time employees**, alongside external clients, vendors, and visiting field engineers.

The business requires a 24/7 high-availability network to support mission-critical workflows, including:
- Software engineering, continuous integration/continuous delivery (CI/CD) pipelines, and database operations.
- Customer support call handling and real-time VoIP ticketing.
- Confidential payroll, ERP, and accounting transactions in Finance.
- Personnel management and hiring records in Human Resources.
- Strategic executive conferencing, boardroom video communications, and financial audits.
- Safe, segregated Internet access for visiting clients in reception and meeting areas.

### 1.2 Departmental Breakdown & User Census
The 140 employees are structured into 6 distinct organizational business units distributed across the 3 floors:

| Department Name | Headcount | Physical Location | Assigned VLAN | Subnet Assignment | Primary Responsibilities & Network Needs |
|---|---|---|---|---|---|
| **Human Resources (HR)** | 15 | First Floor (West Wing) | VLAN 10 | 10.10.10.0/24 | Confidential employee records, payroll queries, recruitment portals. |
| **Finance & Accounting** | 20 | First Floor (East Wing) | VLAN 20 | 10.10.20.0/24 | ERP financial ledgers, audit compliance, banking gateways. High security. |
| **Information Technology (IT)** | 25 | Second Floor (South Wing) | VLAN 30 | 10.10.30.0/24 | Network management, DevOps, systems administration, software development. |
| **Sales & Marketing** | 30 | First Floor (North Wing) | VLAN 40 | 10.10.40.0/24 | High-bandwidth outbound client demos, CRM access, market research. |
| **Customer Support Center**| 40 | Ground Floor (Main Hall) | VLAN 50 | 10.10.50.0/24 | 24/7 call center, live web chat, VoIP telephony, ticketing portals. |
| **Executive Management** | 10 | Second Floor (Executive Suite) | VLAN 60 | 10.10.60.0/24 | C-suite leadership, board meetings, video conferencing, executive briefings. |
| **Guest / Visitor Network** | Variable (30+) | Ground Floor (Lobby & Cafe) | VLAN 70 | 10.10.70.0/24 | Public Wi-Fi access for visitors, clients, and personal mobile devices. |
| **Enterprise Server Farm** | N/A (Dedicated) | Second Floor (MDF Server Room) | VLAN 100 | 10.10.100.0/24 | Dual enterprise servers: DHCP, DNS, Intranet Web Portal, FTP File Storage. |

---

## 2. Comprehensive Network Requirements

### 2.1 Functional Requirements (FR)
1. **FR-01: Logical Departmental Isolation via 802.1Q VLANs**:
   - Broadcast domains must be separated per department to contain broadcast storms, optimize bandwidth, and isolate departmental traffic.
2. **FR-02: Wire-Speed Layer 3 Inter-VLAN Routing**:
   - The campus backbone must utilize Switch Virtual Interfaces (SVIs) on a Multilayer Core Switch to route permitted traffic between departments at Gigabit hardware switching speeds.
3. **FR-03: Centralized Identity and Network Services**:
   - Automated client configuration via centralized DHCP scopes. Private corporate DNS resolution for local resources (e.g., `intranet.nexatech.com`).
4. **FR-04: Strict Guest Network Quarantine**:
   - The Guest Wi-Fi network (VLAN 70) must provide outbound Internet transit while denying any access to internal departmental subnets or servers.
5. **FR-05: Stateful Perimeter Security & Edge Routing**:
   - The edge connection must incorporate a dedicated Cisco ASA Next-Generation Firewall and a Cisco 2911 ISR Router to enforce perimeter filtering, NAT/PAT translation, and protect against external reconnaissance.
6. **FR-06: Shared Intranet & Collaborative File Repository**:
   - Dedicated servers within VLAN 100 must host the internal HTTP website (`intranet.nexatech.com`) and an FTP repository for cross-department document sharing.

### 2.2 Non-Functional Requirements (NFR)
- **NFR-01: Scalability**: Each subnet is allocated a `/24` prefix (supporting up to 254 host addresses), leaving over 100% headroom for future staffing expansion.
- **NFR-02: Performance & Throughput**: 1 Gbps non-blocking 802.1Q trunk links connect access switches to the Core; 100 Mbps full-duplex access ports are provisioned to end stations.
- **NFR-03: Reliability & High Availability**: Central MDF Server Room is powered via dual online Uninterruptible Power Supplies (UPS). Rapid Spanning Tree Protocol (RSTP) ensures loop-free topology with sub-second failover.
- **NFR-04: Manageability & Documentation**: Standardized device hostname taxonomy (`SW-Core-3560`, `SW-ASW1-Floor1`, `R1-Edge-2911`), synchronized logging, and comprehensive port documentation.

---

## 3. Hardware Device Selection & Inventory

### 3.1 Device Selection Rationale

#### 1. Edge Router: Cisco 2911 Integrated Services Router (ISR)
- **Selection Reason**: The Cisco 2911 provides 3 integrated 10/100/1000 Gigabit Ethernet ports, hardware cryptographic acceleration, and expandable Enhanced High-Speed WAN Interface Cards (EHWIC). It acts as the gateway to the Internet Service Provider (ISP) and executes Network Address Translation (NAT/PAT).

#### 2. Perimeter Firewall: Cisco ASA 5506-X Security Appliance
- **Selection Reason**: Provides stateful packet filtering and adaptive security policies between the external untrusted WAN (Security Level 0) and the trusted internal LAN (Security Level 100).

#### 3. Core Layer 3 Switch: Cisco Catalyst 3560-24PS Multilayer Switch
- **Selection Reason**: Serves as the central collapsed core and distribution backbone. Its Layer 3 IP routing engine processes Inter-VLAN routing at line-rate switching speeds across SVIs, eliminating the throughput bottleneck of legacy "Router-on-a-Stick" topologies.

#### 4. Access Switches: 3x Cisco Catalyst 2960-24TT-L Switches
- **Selection Reason**: Provides 24 FastEthernet access ports for workstation drops and 2 dedicated GigabitEthernet uplinks for 802.1Q trunking to the Core Switch. Supports 802.1Q tagging, port security, and rapid STP.

#### 5. Dedicated Enterprise Servers: 2x Cisco Server-PT
- **Selection Reason**:
  - **Server 1 (`SRV-DHCP-DNS`)**: Centralizes DHCP dynamic IP allocation across all 8 VLAN pools and local DNS domain resolution.
  - **Server 2 (`SRV-WEB-FILE`)**: Hosts internal HTTP intranet portal (`intranet.nexatech.com`) and centralized FTP file repository.

#### 6. Wireless Access Points: 3x Cisco AccessPoint-PT / WAP300N
- **Selection Reason**: Provides 802.11b/g/n wireless mobility across each floor. The Ground Floor AP serves visitor guest Wi-Fi (VLAN 70), while 1st and 2nd Floor APs provide corporate mobility.

### 3.2 Master Hardware Inventory Table

| Device Identifier | Hardware Model | Category | Port Capacity | Qty | Location | Primary Role & Assignment |
|---|---|---|---|---|---|---|
| `R1-Edge-2911` | Cisco 2911 ISR | Edge Router | 3x GE, 4x EHWIC slots | 1 | Floor 2 MDF Server Room | WAN Termination, ISP NAT/PAT, Edge Routing |
| `FW-Perimeter-5506` | Cisco ASA 5506-X | Firewall | 8x GE, 1x Console | 1 | Floor 2 MDF Server Room | Stateful Inspection, Outside/Inside Security Boundaries |
| `SW-Core-3560` | Cisco Catalyst 3560-24PS | Core L3 Switch | 24x 10/100, 2x GE SFP | 1 | Floor 2 MDF Server Room | Inter-VLAN SVIs, Default Gateways, Backbone Trunks |
| `SW-ASW1-Floor1` | Cisco Catalyst 2960-24TT | Access Switch | 24x 10/100, 2x GE | 1 | 1st Floor IDF-2 Rack | Floor 1 Access: HR (VLAN 10) & Finance (VLAN 20) |
| `SW-ASW2-Ground` | Cisco Catalyst 2960-24TT | Access Switch | 24x 10/100, 2x GE | 1 | Ground Floor IDF-1 Rack | Ground Floor Access: Support (VLAN 50), Sales (VLAN 40) |
| `SW-ASW3-Floor2` | Cisco Catalyst 2960-24TT | Access Switch | 24x 10/100, 2x GE | 1 | 2nd Floor IDF-3 Rack | Floor 2 Access: IT (VLAN 30), Management (VLAN 60) |
| `SRV-DHCP-DNS` | Server-PT | Enterprise Server | 1x Fast/Gigabit NIC | 1 | Floor 2 MDF Rack 2 | Centralized DHCP Multi-Scope Pool & DNS Server |
| `SRV-WEB-FILE` | Server-PT | Enterprise Server | 1x Fast/Gigabit NIC | 1 | Floor 2 MDF Rack 2 | Corporate Intranet Web & FTP Document Repository |
| `WAP-GF-Guest` | AccessPoint-PT | Wireless AP | 1x FastEthernet, 802.11n | 1 | Ground Floor Reception Ceiling | Dedicated Isolated Guest Wireless Network (VLAN 70) |
| `WAP-1F-Corp` | AccessPoint-PT | Wireless AP | 1x FastEthernet, 802.11n | 1 | 1st Floor Center Ceiling | Floor 1 Corporate Mobility for Sales & HR |
| `WAP-2F-Exec` | AccessPoint-PT | Wireless AP | 1x FastEthernet, 802.11n | 1 | 2nd Floor Boardroom Ceiling | Floor 2 Executive & Engineering Mobility |
| Representative PCs | PC-PT Workstation | Client Endpoint | 1x FastEthernet | 14 | All 3 Floors (Desks) | Workstation simulation across departments (2-3 per Dept) |
| Representative Laptops | Laptop-PT Mobile | Client Endpoint | 1x FastEth, 1x Wi-Fi NIC | 4 | Executive, IT NOC, Lounge | Mobile user & guest client wireless simulation |

---

## 4. Physical Building Network Structure

### 4.1 3-Floor Architectural Layout & Demarcation
The corporate building is a 3-story reinforced concrete structure. To conform with TIA/EIA-568-C commercial building telecommunications standards, the network is physically partitioned into a **Main Distribution Facility (MDF)** and three **Intermediate Distribution Frames (IDFs)**:

```
+-----------------------------------------------------------------------------------+
| SECOND FLOOR (LEVEL 2)                                                            |
|  [IT Department - 25 Staff]    [MDF SERVER ROOM]      [Executive Mgmt - 10 Staff] |
|   - NOC & Dev Stations         - Core 42U Rack         - CEO & Boardrooms         |
|   - IDF-3 (SW-ASW3)            - Server 42U Rack       - WAP-2F-Exec AP           |
+--------------------------------------|--------------------------------------------+
                                       | Vertical Cable Riser Shaft
+--------------------------------------|--------------------------------------------+
| FIRST FLOOR (LEVEL 1)                |                                            |
|  [HR Dept - 15 Staff]          [IDF-2 RACK]           [Finance Dept - 20 Staff]   |
|  [Sales Dept - 30 Staff]       - SW-ASW1-Floor1       [WAP-1F-Corp AP]            |
|   - 48-Port Patch Panel        - Fiber LIU Box                                    |
+--------------------------------------|--------------------------------------------+
                                       | Vertical Cable Riser Shaft
+--------------------------------------|--------------------------------------------+
| GROUND FLOOR (LEVEL 0)               |                                            |
|  [Customer Support - 40 Staff] [IDF-1 RACK]           [Reception & Visitor Lounge]|
|   - 24/7 Call Center Pods      - SW-ASW2-Ground       - WAP-GF-Guest (VLAN 70)    |
|   - High-Density RJ45 drops    - 48-Port Patch Panel  - Visitor Kiosk             |
+-----------------------------------------------------------------------------------+
```

### 4.2 Floor-by-Floor Infrastructure Breakdown

#### 1. Ground Floor (Level 0)
- **Functional Areas**: Reception lobby, visitor waiting lounge, and Customer Support Call Center (40 staff).
- **Wiring Closet (IDF-1)**: Houses a 12U wall-mounted locked equipment rack with `SW-ASW2-Ground`, 48-port Cat6 patch panel, and 1000VA UPS.
- **Wireless Coverage**: Ceiling-mounted `WAP-GF-Guest` delivers isolated guest Internet connectivity to reception and lounge areas.
- **Horizontal Runs**: 40+ dual Cat6 drops routed via under-floor raceways to support agent desks.

#### 2. First Floor (Level 1)
- **Functional Areas**: Sales & Marketing (30 staff), Finance & Accounting (20 staff), and Human Resources (15 staff). Total: 65 staff.
- **Wiring Closet (IDF-2)**: Houses a 12U wall-mounted rack containing `SW-ASW1-Floor1`, 48-port Cat6 patch panel, and 1000VA UPS.
- **Wireless Coverage**: Central ceiling-mounted `WAP-1F-Corp` providing mobility for Sales and HR laptops.
- **Horizontal Runs**: Structured Cat6 drops terminated on RJ-45 faceplates at modular cubicles.

#### 3. Second Floor (Level 2)
- **Functional Areas**: IT Engineering (25 staff), Executive Management (10 staff), and the Central MDF Server Room. Total: 35 staff.
- **Wiring Closet (IDF-3)**: Houses a 12U wall-mounted rack containing `SW-ASW3-Floor2` serving IT desks and executive suites.
- **Wireless Coverage**: Ceiling-mounted `WAP-2F-Exec` providing high-throughput wireless coverage for the boardroom and IT lab.

#### 4. Main Distribution Facility (MDF - Server Room)
- **Physical Specifications**: Centrally located on the Second Floor adjacent to the IT department. Features biometric access control, fire suppression, and precision air conditioning maintaining 20°C (68°F).
- **Core Rack 1 (42U Server Rack)**:
  - Top (40U-41U): Fiber Optic Light Interface Unit (LIU) terminating vertical riser backbones and external ISP fiber.
  - 38U: Cisco 2911 ISR Edge Router (`R1-Edge-2911`).
  - 36U: Cisco ASA 5506-X Firewall (`FW-Perimeter-5506`).
  - 34U: Cisco Catalyst 3560-24PS Core Multilayer Switch (`SW-Core-3560`).
  - 31U-33U: Modular 24-port Cat6 Patch Panels.
  - Bottom (1U-4U): 3000VA Online Double-Conversion UPS with battery pack.
- **Server Rack 2 (42U Server Rack)**:
  - 28U-29U: Infrastructure Server 1 (`SRV-DHCP-DNS`).
  - 26U-27U: Application Server 2 (`SRV-WEB-FILE`).
  - Bottom (1U-4U): 3000VA Online UPS backup.

### 4.3 Structured Cabling Specifications
- **Horizontal Workstation Cabling**: Category 6 (Cat6) 4-pair Unshielded Twisted Pair (UTP) rated for 1000BASE-T Gigabit Ethernet. Maximum run lengths are kept under 60 meters (well within the 90-meter TIA/EIA-568-C permanent link standard).
- **Vertical Backbone Risers**: Multi-Mode OM3 50/125 µm optical fiber cables (backed up by Cat6A solid copper pairs) running through a dedicated vertical riser conduit connecting the MDF on Floor 2 directly to IDF-1 (Ground Floor) and IDF-2 (Floor 1).
- **Color Coding Standard**:
  - Blue: General Department Workstations.
  - Yellow: Server Farm Links.
  - Green: Uplink / Trunk Cables.
  - Purple: Wireless Access Points.
  - Red: WAN and Firewall Transit Connections.

---

## 5. Logical Network Topology Architecture

### 5.1 Hierarchical Enterprise Network Design
The logical topology adopts the industry-standard **Cisco Collapsed Core-Distribution Architecture**, optimal for medium-sized enterprises with up to 500 users:

```
[ INTERNET / ISP WAN ]
         |
    (Gi0/0 Public WAN)
[ R1-Edge-2911 ISR ]
    (Gi0/1 Perimeter Transit)
         |
    (Gi1/1 Outside - Sec 0)
[ FW-Perimeter-5506 ASA ]
    (Gi1/2 Inside - Sec 100)
         |
    (Gi0/1 Core Transit)
[ SW-Core-3560 Multilayer Switch ] <====== [ SERVER FARM - VLAN 100 ]
     |           |            |              - SRV-DHCP-DNS (Fa0/1)
  (Gi0/3)     (Gi0/2)      (Gi0/4)           - SRV-WEB-FILE (Fa0/2)
  Trunk       Trunk        Trunk
     |           |            |
[SW-ASW2-GND] [SW-ASW1-FL1] [SW-ASW3-FL2]
  Ground        Floor 1      Floor 2
  - Supp (50)   - HR (10)    - IT (30)
  - Sales (40)  - Fin (20)   - Mgmt (60)
  - Guest (70)  - Corp AP    - Exec AP
```

### 5.2 Functional Layers

#### 1. WAN & Perimeter Security Layer
- External ISP uplink terminates on `R1-Edge-2911` (`GigabitEthernet0/0`).
- Transit link between `R1-Edge-2911` (`Gi0/1`) and `FW-Perimeter-5506` (`Gi1/1`).
- The firewall enforces perimeter stateful inspection, separating the untrusted public Internet from internal corporate traffic.

#### 2. Collapsed Core & Distribution Layer
- Executed on `SW-Core-3560`.
- All default gateways terminate as **Switch Virtual Interfaces (SVIs)** on the Core Switch.
- Hardware Application-Specific Integrated Circuits (ASICs) route traffic between VLANs at full wire speed (Gigabit line rate), ensuring sub-millisecond inter-departmental latencies.
- Dynamic routing / default routing sends outbound external packets upward to the ASA Firewall and Edge Router.

#### 3. Dedicated Server Farm (VLAN 100)
- Servers attach directly to dedicated switched ports on `SW-Core-3560` (`Fa0/1` and `Fa0/2`), assigned to VLAN 100.
- Locating servers directly at the Core optimizes throughput for all floors and enables centralized access control lists (ACLs) to regulate database and file access.

#### 4. Access Layer
- Three access switches (`SW-ASW1-Floor1`, `SW-ASW2-Ground`, `SW-ASW3-Floor2`) provide local Layer 2 switching.
- Dedicated 802.1Q Gigabit uplinks transport multiple VLAN tags simultaneously over a single physical cable pair.

### 5.3 Logical VLAN Design Table

| VLAN ID | VLAN Name | Subnet Range | Gateway IP (SVI) | Usable IP Pool | Floor / Switch Association | Purpose & Access Rules |
|---|---|---|---|---|---|---|
| **VLAN 10** | `HR_Dept` | 10.10.10.0/24 | 10.10.10.1 | 10.10.10.2 - 10.10.10.254 | Floor 1 / SW-ASW1 | HR personnel endpoints & printers. |
| **VLAN 20** | `Finance_Dept` | 10.10.20.0/24 | 10.10.20.1 | 10.10.20.2 - 10.10.254 | Floor 1 / SW-ASW1 | Accounting workstations & financial ERP. |
| **VLAN 30** | `IT_Dept` | 10.10.30.0/24 | 10.10.30.1 | 10.10.30.2 - 10.10.254 | Floor 2 / SW-ASW3 | IT engineering, DevOps, admin management. |
| **VLAN 40** | `Sales_Dept` | 10.10.40.0/24 | 10.10.40.1 | 10.10.40.2 - 10.10.254 | Ground Floor / SW-ASW2 | Sales reps, CRM systems, presentations. |
| **VLAN 50** | `Cust_Support`| 10.10.50.0/24 | 10.10.50.1 | 10.10.50.2 - 10.10.254 | Ground Floor / SW-ASW2 | Call center desks, CRM ticketing, VoIP. |
| **VLAN 60** | `Management` | 10.10.60.0/24 | 10.10.60.1 | 10.10.60.2 - 10.10.254 | Floor 2 / SW-ASW3 | Executive C-suite, boardroom terminals. |
| **VLAN 70** | `Guest_Network`| 10.10.70.0/24| 10.10.70.1 | 10.10.70.2 - 10.10.254 | Ground Floor / SW-ASW2 | Isolated visitor Wi-Fi; strictly internet-only. |
| **VLAN 100**| `Server_Farm` | 10.10.100.0/24| 10.10.100.1 | 10.10.100.2 - 10.10.100.254| Floor 2 MDF / SW-Core | DHCP, DNS, Web intranet, FTP file server. |
| **VLAN 99** | `Native_Mgmt` | 10.10.99.0/24 | 10.10.99.1 | 10.10.99.2 - 10.10.99.254 | All Trunks (Tagged) | Native VLAN for 802.1Q trunk security. |

---

## 6. Base Cisco Packet Tracer Topology Blueprint

### 6.1 Port-to-Port Cabling Matrix
The following table provides the exact wiring specifications to construct `Base_Topology_v1.pkt`:

| Source Device | Source Port | Destination Device | Destination Port | Cable Media Type | Link Operational Role |
|---|---|---|---|---|---|
| `ISP_Cloud` | Ethernet 6 | `R1-Edge-2911` | GigabitEthernet0/0 | Copper Straight-Through | WAN Gateway Link |
| `R1-Edge-2911` | GigabitEthernet0/1 | `FW-Perimeter-5506`| GigabitEthernet1/1 | Copper Straight-Through | Perimeter Transit (Outside) |
| `FW-Perimeter-5506`| GigabitEthernet1/2 | `SW-Core-3560` | GigabitEthernet0/1 | Copper Straight-Through | Internal Transit (Inside) |
| `SW-Core-3560` | GigabitEthernet0/2 | `SW-ASW1-Floor1` | GigabitEthernet0/1 | Copper Straight-Through | Vertical 802.1Q Trunk Link |
| `SW-Core-3560` | GigabitEthernet0/3 | `SW-ASW2-Ground` | GigabitEthernet0/1 | Copper Straight-Through | Vertical 802.1Q Trunk Link |
| `SW-Core-3560` | GigabitEthernet0/4 | `SW-ASW3-Floor2` | GigabitEthernet0/1 | Copper Straight-Through | Vertical 802.1Q Trunk Link |
| `SW-Core-3560` | FastEthernet0/1 | `SRV-DHCP-DNS` | FastEthernet0 | Copper Straight-Through | Dedicated Server Access (VLAN 100) |
| `SW-Core-3560` | FastEthernet0/2 | `SRV-WEB-FILE` | FastEthernet0 | Copper Straight-Through | Dedicated Server Access (VLAN 100) |
| `SW-ASW1-Floor1` | GigabitEthernet0/2 | `WAP-1F-Corp` | Port 0 | Copper Straight-Through | Corporate Wi-Fi AP Link |
| `SW-ASW1-Floor1` | FastEthernet0/1 | `PC-HR-01` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 10) |
| `SW-ASW1-Floor1` | FastEthernet0/2 | `PC-HR-02` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 10) |
| `SW-ASW1-Floor1` | FastEthernet0/3 | `PC-FIN-01` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 20) |
| `SW-ASW1-Floor1` | FastEthernet0/4 | `PC-FIN-02` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 20) |
| `SW-ASW2-Ground` | GigabitEthernet0/2 | `WAP-GF-Guest` | Port 0 | Copper Straight-Through | Guest Wi-Fi AP Link (VLAN 70) |
| `SW-ASW2-Ground` | FastEthernet0/1 | `PC-SUP-01` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 50) |
| `SW-ASW2-Ground` | FastEthernet0/2 | `PC-SUP-02` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 50) |
| `SW-ASW2-Ground` | FastEthernet0/3 | `PC-SUP-03` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 50) |
| `SW-ASW2-Ground` | FastEthernet0/4 | `PC-SALES-01` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 40) |
| `SW-ASW2-Ground` | FastEthernet0/5 | `PC-SALES-02` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 40) |
| `SW-ASW2-Ground` | FastEthernet0/6 | `PC-SALES-03` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 40) |
| `SW-ASW3-Floor2` | GigabitEthernet0/2 | `WAP-2F-Exec` | Port 0 | Copper Straight-Through | Executive Wi-Fi AP Link |
| `SW-ASW3-Floor2` | FastEthernet0/1 | `PC-IT-Admin-01` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 30) |
| `SW-ASW3-Floor2` | FastEthernet0/2 | `PC-IT-Admin-02` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 30) |
| `SW-ASW3-Floor2` | FastEthernet0/3 | `Laptop-IT-Eng` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 30) |
| `SW-ASW3-Floor2` | FastEthernet0/4 | `PC-MGMT-CEO` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 60) |
| `SW-ASW3-Floor2` | FastEthernet0/5 | `Laptop-MGMT-Dir` | FastEthernet0 | Copper Straight-Through | Workstation Drop (VLAN 60) |

---

## 7. Baseline Device Startup Configurations
To ensure fast and error-free assembly in Cisco Packet Tracer, all devices have pre-configured startup scripts ready to be pasted directly into their respective CLI terminals:

### 7.1 Core Layer 3 Switch (`SW-Core-3560`)
```text
enable
configure terminal
hostname SW-Core-3560
no ip domain-lookup
ip routing

! Pre-configure VLAN Database
vlan 10
 name HR_Dept
vlan 20
 name Finance_Dept
vlan 30
 name IT_Dept
vlan 40
 name Sales_Dept
vlan 50
 name Cust_Support
vlan 60
 name Management
vlan 70
 name Guest_Network
vlan 100
 name Server_Farm
vlan 99
 name Management_Native
exit

! Core to Firewall Transit Link
interface GigabitEthernet0/1
 description Transit Link to ASA Firewall Gi1/2
 no shutdown
 exit

! Vertical Backbone 802.1Q Trunk Links
interface GigabitEthernet0/2
 description Uplink Trunk to SW-ASW1-Floor1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface GigabitEthernet0/3
 description Uplink Trunk to SW-ASW2-Ground
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface GigabitEthernet0/4
 description Uplink Trunk to SW-ASW3-Floor2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

! Server Farm Dedicated Host Ports (VLAN 100)
interface FastEthernet0/1
 description Server 1: Centralized DHCP + DNS (SRV-DHCP-DNS)
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown
 exit

interface FastEthernet0/2
 description Server 2: Intranet Web + File Sharing (SRV-WEB-FILE)
 switchport mode access
 switchport access vlan 100
 spanning-tree portfast
 no shutdown
 exit

end
write memory
```

### 7.2 Access Switch 1 (`SW-ASW1-Floor1`)
```text
enable
configure terminal
hostname SW-ASW1-Floor1
no ip domain-lookup

vlan 10
 name HR_Dept
vlan 20
 name Finance_Dept
vlan 99
 name Management_Native
exit

interface GigabitEthernet0/1
 description Trunk Uplink to SW-Core-3560 Gi0/2
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface GigabitEthernet0/2
 description Uplink to WAP-1F-Corp
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface range FastEthernet0/1 - 2
 description HR Department Workstations
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/3 - 4
 description Finance Department Workstations
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/5 - 24
 shutdown
 exit

end
write memory
```

### 7.3 Access Switch 2 (`SW-ASW2-Ground`)
```text
enable
configure terminal
hostname SW-ASW2-Ground
no ip domain-lookup

vlan 40
 name Sales_Dept
vlan 50
 name Cust_Support
vlan 70
 name Guest_Network
vlan 99
 name Management_Native
exit

interface GigabitEthernet0/1
 description Trunk Uplink to SW-Core-3560 Gi0/3
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface GigabitEthernet0/2
 description Dedicated Access to WAP-GF-Guest
 switchport mode access
 switchport access vlan 70
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/1 - 3
 description Customer Support Workstations
 switchport mode access
 switchport access vlan 50
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/4 - 6
 description Sales Department Workstations
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/7 - 24
 shutdown
 exit

end
write memory
```

### 7.4 Access Switch 3 (`SW-ASW3-Floor2`)
```text
enable
configure terminal
hostname SW-ASW3-Floor2
no ip domain-lookup

vlan 30
 name IT_Dept
vlan 60
 name Management
vlan 99
 name Management_Native
exit

interface GigabitEthernet0/1
 description Trunk Uplink to SW-Core-3560 Gi0/4
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface GigabitEthernet0/2
 description Uplink to WAP-2F-Exec
 switchport mode trunk
 switchport trunk native vlan 99
 no shutdown
 exit

interface range FastEthernet0/1 - 3
 description IT Engineering Workstations
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/4 - 5
 description Executive Management Workstations
 switchport mode access
 switchport access vlan 60
 spanning-tree portfast
 no shutdown
 exit

interface range FastEthernet0/6 - 24
 shutdown
 exit

end
write memory
```

### 7.5 Edge Router (`R1-Edge-2911`) & Firewall (`FW-Perimeter-5506`)
*(Refer to configuration scripts stored in `packet_tracer/startup_configs/R1_Edge_Router.txt` and `FW_Perimeter_ASA.txt`)*

---

## 8. Integration & Team Hand-off Checklist

| Checkpoint | Target Member | Artifact Transferred | Verification Criteria | Status |
|---|---|---|---|---|
| **Phase 1: Planning** | Members 1, 2, 3 | `Project_Requirements_Final.docx` | Common scenario, headcounts, VLAN mapping agreed upon | **COMPLETED** |
| **Phase 2: Topology** | Member 1 -> Member 2 | `Base_Topology_v1.pkt` & Connection Matrix | All 6 devices cabled, labeled, trunks open, links green/amber | **READY FOR HAND-OFF** |
| **Phase 3: Subnetting**| Member 2 | `IP_and_VLAN_Plan.docx` | Subnets allocated within 10.10.0.0/16 with gateway IPs | Pending Member 2 |
| **Phase 4: Routing** | Member 2 -> Member 3 | `Network_Config_v2.pkt` | SVIs configured, IP routing active, Inter-VLAN pings working | Pending Member 2 |
| **Phase 5: Services** | Member 3 | `Services_Security_v3.pkt` | DHCP pools running, DNS resolving, Guest ACL active | Pending Member 3 |
| **Phase 6: Integration**| All Members | Final Consolidated Project Report | All sections combined, formatted, viva preparation complete | Pending Phase 6 |

---
*Report prepared and submitted by Member 1 (Network Design Engineer).*
