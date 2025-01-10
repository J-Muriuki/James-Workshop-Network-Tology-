# James-Workshop-Network-Topology-
It is a network design using packet tracer for a business like environment.
**James Muriuki**

**Part A: Network Design and Configuration Report**

**1\. Introduction**

In this section, I will provide an overview of the network design, the configurations that have been applied to the devices, and the challenges faced during the design and implementation of the network infrastructure. The design was based on the requirements to create a secure and efficient corporate network with remote access, redundancy, and various VLAN configurations to segment different departments and functions within my company, "James Workshop." Which provides design and art services.

**2\. Network Design**

The network design for **James Workshop** incorporates several critical elements to ensure the functionality, security, and redundancy required for a corporate network. Below are the main components of the design:

**2.1 Network Topology**

The network consists of three main buildings: Reception, Boardroom, and Main Office. In addition, there is a **Guest Wi-Fi** network for visitors. The design incorporates a combination of Layer 2 and Layer 3 devices to manage traffic between different VLANs and ensure secure communication.

-   **Router**: The network uses a **Cisco 2911 Router** to handle routing between VLANs and provide connectivity to the internet.
-   **Switches**: Three **Layer 2 Switches** (Switch 1, Switch 2, and Switch 3) are used to segment the network into various VLANs and connect devices within each department.
-   **VLANs**: Several VLANs are configured to separate the network into logical segments, including:

-   **VLAN 10**: Reception
-   **VLAN 20**: Boardroom
-   **VLAN 30**: Main Office
-   **VLAN 40**: Guest Wi-Fi

This segmentation ensures that the network traffic from different departments does not interfere with each other, and it provides a way to implement security policies.

Figure of the network topology

**2.2 IP Addressing Scheme**

An appropriate IP addressing scheme has been designed to allow communication between devices within the same VLAN and across VLANs, as needed. The address ranges for each VLAN are as follows:

-   **VLAN 10 (Reception)**: 192.168.10.0/24
-   **VLAN 20 (Boardroom)**: 192.168.20.0/24
-   **VLAN 30 (Main Office)**: 192.168.30.0/24
-   **VLAN 40 (Guest Wi-Fi)**: 192.168.40.0/24

Each device within the VLANs is assigned an appropriate IP address, and routers and switches are configured to handle routing between VLANs using Inter-VLAN Routing.

**3\. Configurations**

**3.1 VLAN Configuration on Switches**

VLANs have been configured on all switches in the network using the following steps:

1.  **Creating VLANs**: On each switch, the following VLANs were created using the command:

Switch(config)# vlan 10

Switch(config-vlan) # name Reception

1.  **Assigning VLANs to Ports**: Ports on each switch were assigned to the corresponding VLAN based on their usage. For instance, for vlan 10 of Reception:

Switch(config)# interface range fa0/1 - 4

Switch(config-if-range) # switchport mode access

Switch(config-if-range) # switchport access vlan 10

1.  **Trunk Ports**: Trunk ports were configured to allow the communication of multiple VLANs between switches:

Switch(config)# interface gi0/1

Switch(config-if)# switchport mode trunk

Switch(config-if)# switchport trunk allowed vlan 10,20,30,40

**3.2 Router Configuration (Inter-VLAN Routing)**

On the router, **Subinterfaces** were created for each VLAN to enable routing between them. Each subinterface was assigned an IP address corresponding to the gateway for that VLAN.

Example for VLAN 10:

Router(config)# interface gigabitEthernet 0/0.10

Router(config-subif)# encapsulation dot1Q 10

Router(config-subif)# ip address 192.168.10.1 255.255.255.0

**3.3 OSPF Configuration**

Open Shortest Path First (OSPF) was chosen as the dynamic routing protocol to ensure efficient and scalable routing between VLANs. OSPF was configured on the router to allow seamless routing between the different subnets. I did this on both the routers to enable redundancy mechanisms in the network.

Router(config)# router ospf 1

Router(config-router) # network 192.168.10.0 0.0.0.255 area 0

**3.4 Security Configurations**

-   **Port Security** was enabled on the switches to prevent unauthorized devices from accessing the network. This was done with the vlan associated with boardroom since important meetings are held using that network.

Switch(config)# interface fa0/5 - 10

Switch(config-if)# switchport port-security

Switch(config-if) # switchport port-security maximum 1

-   **Access Control Lists (ACLs)** were implemented to restrict access to certain network resources based on IP addresses. This was implemented on the guest Wi-Fi  on Vlan 40 to limit what they can do on my network thus they can access the other vlans on the network.
-   **SSH **was configured on the network allowing access to the router remotely.

**4\. Challenges Faced**

During the design and configuration of the network, several challenges were encountered:

1.  **VLAN Configuration Issues**: Initially, there were problems with assigning VLANs to specific ports on the switches. Some ports were not accepting the VLAN configuration due to misconfiguration of the switchport modes or incorrect port assignments. This was resolved by ensuring proper mode configuration for each port (access vs. trunk).
2.  **Inter-VLAN Routing Issues**: The router's subinterfaces were not correctly configured initially, which led to routing problems between VLANs. After carefully reviewing the configuration, the subinterfaces were correctly configured with the appropriate IP addresses and encapsulation.
3.  **OSPF Neighbor Relationships**: OSPF neighbor relationships were not established between some routers, due to misconfigurations in network settings and area assignments. After correcting these settings, the routers were able to establish OSPF adjacencies.
4.  **Port Security and MAC Address Limitation**: Configuring **Port Security** resulted in some issues when devices exceeded the maximum allowed MAC addresses on ports. This required troubleshooting and adjusting the port security settings to accommodate the number of devices connected.

**Conclusion**

The network design for **James Workshop** has been successfully implemented, with configurations for VLANs, routing, and security all functioning as expected. The main challenges faced were related to VLAN and OSPF configurations, which were resolved through careful troubleshooting and verification of configurations. The final network is secure, scalable, and provides efficient communication between various departments, as well as remote access capabilities.
