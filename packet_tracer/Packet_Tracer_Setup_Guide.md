# Cisco Packet Tracer Base Topology Construction Guide
**File Target**: Base_Topology_v1.pkt  
**Author**: Member 1 (Network Design Engineer)  
**Organization**: NexaTech Solutions Pvt. Ltd.  

---

## 1. Objective
This guide walks you through building the baseline Cisco Packet Tracer topology (Base_Topology_v1.pkt) from scratch in under 10 minutes. This base file contains all placed devices, physical cabling, port assignments, and baseline device configurations, ready for Member 2 (IP Addressing & Routing) and Member 3 (Services & Security).

---

## 2. Cisco Packet Tracer Device Selection Palette

Select the exact hardware models from the Cisco Packet Tracer bottom toolbar:

| Device Role | Packet Tracer Category | Model to Drag | Display Name / Label | Notes / Modules |
|---|---|---|---|---|
| **Edge Router** | Network Devices > Routers | **2911** | R1-Edge-2911 | 3 onboard GigabitEthernet ports |
| **Perimeter Firewall** | Network Devices > Security | **5506-X** | FW-Perimeter-5506 | 8 onboard GigabitEthernet ports |
| **Core Layer 3 Switch** | Network Devices > Switches | **3560-24PS** | SW-Core-3560 | Multilayer Switch (Layer 3 Routing) |
| **Access Switch 1** | Network Devices > Switches | **2960-24TT** | SW-ASW1-Floor1 | Floor 1 (HR & Finance) |
| **Access Switch 2** | Network Devices > Switches | **2960-24TT** | SW-ASW2-Ground | Ground Floor (Support & Sales) |
| **Access Switch 3** | Network Devices > Switches | **2960-24TT** | SW-ASW3-Floor2 | Floor 2 (IT & Management) |
| **Central Server 1** | End Devices > End Devices | **Server-PT** | SRV-DHCP-DNS | DHCP & DNS Services |
| **Central Server 2** | End Devices > End Devices | **Server-PT** | SRV-WEB-FILE | Intranet Web & FTP File Sharing |
| **Floor 1 AP** | Network Devices > Wireless | **AccessPoint-PT** | WAP-1F-Corp | Corporate Wi-Fi Floor 1 |
| **Ground Floor AP** | Network Devices > Wireless | **AccessPoint-PT** | WAP-GF-Guest | Guest Wi-Fi Ground Floor |
| **Floor 2 AP** | Network Devices > Wireless | **AccessPoint-PT** | WAP-2F-Exec | Executive Wi-Fi Floor 2 |
| **HR Workstations** | End Devices > End Devices | **PC-PT** | PC-HR-01, PC-HR-02 | VLAN 10 |
| **Finance Workstations**| End Devices > End Devices | **PC-PT** | PC-FIN-01, PC-FIN-02| VLAN 20 |
| **Support Workstations**| End Devices > End Devices | **PC-PT** | PC-SUP-01, PC-SUP-02, PC-SUP-03 | VLAN 50 |
| **Sales Workstations**  | End Devices > End Devices | **PC-PT** | PC-SALES-01, PC-SALES-02, PC-SALES-03 | VLAN 40 |
| **IT Workstations**     | End Devices > End Devices | **PC-PT**, **Laptop-PT** | PC-IT-Admin-01, PC-IT-Admin-02, Laptop-IT-Eng | VLAN 30 |
| **Management Devices**  | End Devices > End Devices | **PC-PT**, **Laptop-PT** | PC-MGMT-CEO, Laptop-MGMT-Dir | VLAN 60 |
| **Guest Wireless**      | End Devices > End Devices | **Laptop-PT**, **Smartphone-PT** | Laptop-Guest-01, Smartphone-Guest | VLAN 70 (W-NIC module installed) |
| **External WAN / ISP**  | WAN Emulation | **Cloud-PT** | ISP_Cloud | Simulated Public Internet |

---

## 3. Recommended Canvas Layout (Hierarchical Arrangement)

Arrange your workspace canvas in Packet Tracer cleanly following the three-tier enterprise design:

`
                            [ ISP_Cloud ]
                                  |
                           [ R1-Edge-2911 ]
                                  |
                        [ FW-Perimeter-5506 ]
                                  |
                         [ SW-Core-3560 ]
                         /       |      \            [ SRV-DHCP-DNS ]
                        /        |       \           [ SRV-WEB-FILE ]
                       /         |               [ SW-ASW2-Ground ] [ SW-ASW1-Floor1 ] [ SW-ASW3-Floor2 ]
       (Ground Floor)       (First Floor)       (Second Floor)
       /     |     \        /     |    \         /    |          SUP   SALES  WAP-GF   HR    FIN  WAP-1F    IT   MGMT  WAP-2F
     PCs   PCs     Guest   PCs   PCs   Corp     PCs  PCs    Exec
`

- **Top Tier (WAN & Perimeter)**: Place ISP_Cloud, R1-Edge-2911, and FW-Perimeter-5506 at the top center.
- **Middle Tier (Enterprise Core & Server Farm)**: Place SW-Core-3560 in the center. Place SRV-DHCP-DNS and SRV-WEB-FILE directly to its right side.
- **Lower-Middle Tier (Distribution / Access Layer)**: Place the three Access Switches horizontally below the Core Switch:
  - Left: SW-ASW2-Ground (Ground Floor)
  - Center: SW-ASW1-Floor1 (1st Floor)
  - Right: SW-ASW3-Floor2 (2nd Floor)
- **Bottom Tier (Access Layer Devices)**: Place the corresponding department PCs and Wireless Access Points beneath each Access Switch.

---

## 4. Physical Cabling Instructions

Use the **Copper Straight-Through Cable** (Solid black line in Connections toolbar) for all connections:

### Backbone & Infrastructure Connections
1. Connect ISP_Cloud (Ethernet 6) to R1-Edge-2911 (GigabitEthernet0/0).
2. Connect R1-Edge-2911 (GigabitEthernet0/1) to FW-Perimeter-5506 (GigabitEthernet1/1).
3. Connect FW-Perimeter-5506 (GigabitEthernet1/2) to SW-Core-3560 (GigabitEthernet0/1).
4. Connect SW-Core-3560 (GigabitEthernet0/2) to SW-ASW1-Floor1 (GigabitEthernet0/1) *(Trunk Uplink)*.
5. Connect SW-Core-3560 (GigabitEthernet0/3) to SW-ASW2-Ground (GigabitEthernet0/1) *(Trunk Uplink)*.
6. Connect SW-Core-3560 (GigabitEthernet0/4) to SW-ASW3-Floor2 (GigabitEthernet0/1) *(Trunk Uplink)*.
7. Connect SW-Core-3560 (FastEthernet0/1) to SRV-DHCP-DNS (FastEthernet0).
8. Connect SW-Core-3560 (FastEthernet0/2) to SRV-WEB-FILE (FastEthernet0).

### Access Switch 1 (Floor 1 - HR & Finance) Cabling
- GigabitEthernet0/2 -> WAP-1F-Corp (Port 0)
- FastEthernet0/1 -> PC-HR-01 (FastEthernet0)
- FastEthernet0/2 -> PC-HR-02 (FastEthernet0)
- FastEthernet0/3 -> PC-FIN-01 (FastEthernet0)
- FastEthernet0/4 -> PC-FIN-02 (FastEthernet0)

### Access Switch 2 (Ground Floor - Support & Sales) Cabling
- GigabitEthernet0/2 -> WAP-GF-Guest (Port 0)
- FastEthernet0/1 -> PC-SUP-01 (FastEthernet0)
- FastEthernet0/2 -> PC-SUP-02 (FastEthernet0)
- FastEthernet0/3 -> PC-SUP-03 (FastEthernet0)
- FastEthernet0/4 -> PC-SALES-01 (FastEthernet0)
- FastEthernet0/5 -> PC-SALES-02 (FastEthernet0)
- FastEthernet0/6 -> PC-SALES-03 (FastEthernet0)

### Access Switch 3 (Floor 2 - IT & Management) Cabling
- GigabitEthernet0/2 -> WAP-2F-Exec (Port 0)
- FastEthernet0/1 -> PC-IT-Admin-01 (FastEthernet0)
- FastEthernet0/2 -> PC-IT-Admin-02 (FastEthernet0)
- FastEthernet0/3 -> Laptop-IT-Eng (FastEthernet0)
- FastEthernet0/4 -> PC-MGMT-CEO (FastEthernet0)
- FastEthernet0/5 -> Laptop-MGMT-Dir (FastEthernet0)

---

## 5. Applying Baseline Configurations

For each switch and router:
1. Click on the device in Packet Tracer.
2. Select the **CLI** tab.
3. If prompted with Continue with configuration dialog? [yes/no]:, type 
o and press Enter.
4. Press Enter to reach the prompt (Router> or Switch>).
5. Open the corresponding file in cndc_network_project_member1/packet_tracer/startup_configs/:
   - R1-Edge-2911: Copy all contents of R1_Edge_Router.txt and paste into the CLI.
   - FW-Perimeter-5506: Copy all contents of FW_Perimeter_ASA.txt and paste into the CLI.
   - SW-Core-3560: Copy all contents of SW_Core_L3.txt and paste into the CLI.
   - SW-ASW1-Floor1: Copy all contents of SW_Access_1_Floor1.txt and paste into the CLI.
   - SW-ASW2-Ground: Copy all contents of SW_Access_2_Ground.txt and paste into the CLI.
   - SW-ASW3-Floor2: Copy all contents of SW_Access_3_Floor2.txt and paste into the CLI.

---

## 6. Verification and Validation Checklist

Run the following commands on the switches to verify baseline readiness:

### On Core Switch (SW-Core-3560):
`	ext
SW-Core-3560# show ip interface brief
SW-Core-3560# show interfaces trunk
SW-Core-3560# show vlan brief
`
*Expected Result*: Gi0/2, Gi0/3, and Gi0/4 show as active 802.1Q trunks. VLANs 10, 20, 30, 40, 50, 60, 70, 100 are listed in the VLAN database.

### On Access Switches (SW-ASW1, SW-ASW2, SW-ASW3):
`	ext
SW-ASW1-Floor1# show interfaces trunk
SW-ASW1-Floor1# show interfaces status
`
*Expected Result*: Gi0/1 shows as trunking. Workstation ports show connected with their assigned VLAN tags.

---

## 7. Saving the Topology File
1. In Cisco Packet Tracer, click **File > Save As...**
2. Name the file: Base_Topology_v1.pkt
3. Save it in your project deliverables folder.
4. This file is now fully prepared to hand off to **Member 2** for IP addressing, subnets, SVI configuration, and inter-VLAN routing!
