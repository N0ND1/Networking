# Networking
Small Enterprise Network

1. Network Model Selection
I first evaluated different network models and chose Router-on-a-Stick because:
It allows inter-VLAN routing using a single router interface
It is cost-effective and commonly used in small to medium enterprises
It clearly demonstrates VLAN tagging and Layer 2–Layer 3 interaction

2. Logical Planning & Diagramming
Before configuring any devices:
I created a network topology diagram using draw.io
This helped me:
Visualize device placement
Plan VLANs and IP addressing
Decide which protocols would run on each device
Avoid configuration conflicts during implementation

🧩 Network Requirements Implemented
Multiple departments separated using VLANs
Inter-VLAN routing via router subinterfaces
Centralized DHCP
DNS relay configuration
Internet access using NAT
Access Control Lists (ACLs) for inter-department security
Trunking between switches
Redundant links using EtherChannel
Dedicated management VLAN

🗂️ VLAN Design
VLAN ID	Name	Purpose
10	IT	Administrators and servers
20	Sales	Sales department users
30	Accounting	Financial and sensitive data
40	Guest	Internet-only access
99	Management	Network device management

🌐 IP Addressing Scheme
Base Network: 192.168.10.0/24 (VLSM applied)
VLAN	Subnet	Default Gateway
IT (10)	192.168.10.0/26	192.168.10.1
Sales (20)	192.168.10.64/26	192.168.10.65
Accounting (30)	192.168.10.128/26	192.168.10.129
Guest (40)	192.168.10.192/27	192.168.10.193
Management (99)	192.168.10.224/27	192.168.10.225
This approach ensures efficient IP utilization and clear separation between departments.

🛠️ Implementation Steps
Step 1: Physical Topology Setup
Deployed:
1 Router
2 Switches
End devices per VLAN
Connected devices using appropriate copper cabling
Implemented redundant links between switches

Step 2: Basic Device Configuration
Assigned hostnames to all devices
Disabled DNS lookup on CLI to prevent command delays
Ensured clean and readable CLI environments

Step 3: VLAN Creation (Switch Configuration)
Created VLANs for each department on both switches
Ensured VLAN consistency across the switching layer
This step establishes logical segmentation within the network.

Step 4: Access Port Assignment
Assigned switch access ports to the appropriate VLANs
Ensured end devices were isolated within their department VLANs
Prevented unnecessary trunk negotiation on access ports

Step 5: Trunk Configuration
Configured trunk links:
Between Router ↔ Switch
Between Switch 1 ↔ Switch 2
Explicitly allowed only required VLANs on trunks
This enables VLAN traffic to traverse between devices securely and efficiently.

Step 6: Inter-VLAN Routing (Router-on-a-Stick)
Configured router subinterfaces for each VLAN
Applied IEEE 802.1Q encapsulation
Assigned default gateway IP addresses per VLAN
This allows devices in different VLANs to communicate through the router.

Step 7: DHCP Configuration
Configured centralized DHCP pools on the router
Created separate pools for IT, Sales, and Accounting VLANs
Excluded reserved IP ranges for network infrastructure
This ensures automated and conflict-free IP address assignment.

Step 8: NAT Configuration
Configured NAT overload (PAT) on the router
Enabled internet access for all internal VLANs using a single public IP
Defined inside and outside interfaces correctly
This allows private IP addresses to communicate with external networks.

Step 9: Access Control Lists (ACLs)
Implemented security rules based on department requirements:
IT: Full network access
Sales: Restricted from Accounting VLAN
Accounting: Limited access to IT and internet only
Guest: Internet-only access
ACLs were applied inbound on the appropriate router subinterfaces to enforce least privilege access.

Step 10: Redundancy Using EtherChannel
Configured EtherChannel between Switch 1 and Switch 2
Bundled multiple physical links into a single logical channel
Improved bandwidth and ensured fault tolerance

Step 11: Management VLAN Configuration
Configured VLAN 99 for device management
Assigned management IP addresses to switches
Restricted management access to the management VLAN only
This improves security by isolating control-plane traffic.

✅ Verification & Testing
The following checks were performed:
VLAN verification (show vlan brief)
Trunk verification (show interfaces trunk)
Subinterface status (show ip interface brief)
DHCP address assignment
Inter-VLAN connectivity testing using ping
Internet access verification
ACL behavior validation
