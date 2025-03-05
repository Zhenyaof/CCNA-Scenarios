# CCNA Scenarios

# About This Repository

Welcome to the CCNA Scenarios repository, a resource for networking enthusiasts, students, and professionals to practice CCNA-level concepts. It includes **lab scenarios**, **configuration examples**, and **troubleshooting exercises** to enhance networking skills. Users can also find **subnetting practice** and **routing & switching concepts** like VLANs and STP. This repository is ideal for **CCNA candidates**, **networking students**, and **IT professionals**. Labs can be run on **Cisco Packet Tracer, GNS3, or EVE-NG** by following provided instructions. Contributions are encouraged through **pull requests and issue submissions**.
**Important Notice**

This repository contains the IP addresses used in scenarios, along with their passwords. Please make sure you handle this information correctly and use it for learning purposes.



# Scenario 1 Summary

This guide provides the necessary steps to configure and secure SSH access on Cisco devices, including key generation, configuration, and enabling secure remote management.

## Key Steps

1. **Enable SSH on the Cisco Device**  
   Ensure the device has a hostname and domain name configured (required for key generation):

   ```bash
   configure terminal
   hostname <your_device_name>
   ip domain-name example.com
   ```

2. **Generate RSA Keys**  
   To generate the RSA keys for SSH access, use the following command:

   ```bash
   crypto key generate rsa usage-keys label ssh-key modulus 2048
   ```

3. **Configure SSH Version**  
   Ensure SSH version 2 is used for enhanced security:

   ```bash
   ip ssh version 2
   ```

4. **Create a Local User Account**  
   Create a local username and password for SSH login:

   ```bash
   username admin privilege 15 secret yourpassword
   ```

5. **Enable SSH on VTY Lines**  
   Configure the VTY lines (virtual terminal) to accept SSH connections:

   ```bash
   line vty 0 4
   transport input ssh
   login local
   ```

6. **Configure a Timeout and Authentication for SSH**  
   Set timeouts and enforce local user authentication:

   ```bash
   exec-timeout 10 0
   ```

7. **Test SSH Connectivity**  
   From a remote device, test the SSH connection using the following command:

   ```bash
   ssh admin@<device_ip_address>
   ```

8. **Disable Telnet (Optional but recommended)**  
   For better security, disable Telnet access:

   ```bash
   no transport input telnet
   ```

# Scenario 2 Summary

This guide covers how to configure IPv6 on Cisco devices, enabling IPv6 routing and assigning IPv6 addresses to interfaces.

## Key Steps

1. **Enable IPv6 Routing**  
   First, ensure IPv6 routing is enabled on the device:

   ```bash
   configure terminal
   ipv6 unicast-routing
   ```

2. **Assign IPv6 Address to an Interface**  
   Enable IPv6 on the desired interface by assigning an IPv6 address:

   ```bash
   interface <interface_name>
   ipv6 address <ipv6_address>/64
   no shutdown
   ```

3. **Enable IPv6 on Multiple Interfaces (Optional)**  
   If you have more interfaces that need IPv6 configuration, repeat the above step for each.

4. **Check IPv6 Configuration**  
   To verify that IPv6 has been properly configured on the device:

   ```bash
   show ipv6 interface brief
   ```

5. **Configure IPv6 Static Routing (Optional)**  
   If static routing is needed, configure IPv6 routes:

   ```bash
   ipv6 route <destination_network> <next_hop_ipv6_address>
   ```

## Why Use IPv6?
IPv6 is necessary due to the limitations of IPv4 addressing, with its finite number of available IP addresses. IPv6 offers a vast address space (allowing for trillions of unique addresses), better security features (like mandatory IPsec), and improved routing efficiency. It's essential for supporting the growing number of devices and services connected to the internet.


# Scenarios 3 , 4 , 5 Summary

This guide combines configuring VLANs, trunking, inter-VLAN routing, and EtherChannel on Cisco devices. It covers the key steps for setting up VLANs, 802.1Q trunking, inter-VLAN routing, and EtherChannel for load balancing and redundancy.

## Key Steps

### 1. **Configure VLANs**  
   First, create the required VLANs on the switch:

   ```bash
   configure terminal
   vlan 10
   name Sales
   vlan 20
   name Marketing
   ```

   Repeat the above steps for all VLANs you need to configure.

### 2. **Assign VLANs to Switch Ports**  
   Assign VLANs to specific switch ports. For example:

   ```bash
   interface range fastethernet 0/1 - 10
   switchport mode access
   switchport access vlan 10
   ```

   Repeat for other ports and VLANs.

### 3. **Configure 802.1Q Trunking on Switch Ports**  
   To allow multiple VLANs to pass between switches, configure trunking on the interfaces that connect the switches:

   ```bash
   interface gigabitEthernet 0/1
   switchport mode trunk
   switchport trunk encapsulation dot1q
   ```

   This will configure the port for 802.1Q trunking.

### 4. **Configure Router for Inter-VLAN Routing**  
   Enable routing on a router (or Layer 3 switch) to route traffic between VLANs. This is typically done by creating sub-interfaces for each VLAN:

   ```bash
   interface gigabitEthernet 0/0.10
   encapsulation dot1Q 10
   ip address 192.168.10.1 255.255.255.0

   interface gigabitEthernet 0/0.20
   encapsulation dot1Q 20
   ip address 192.168.20.1 255.255.255.0
   ```

   The router will now be able to route traffic between VLAN 10 and VLAN 20.

### 5. **Configure EtherChannel for Link Aggregation**  
   EtherChannel provides load balancing and redundancy by bundling multiple physical links into a single logical link. Configure EtherChannel on both switches:

   ```bash
   interface range gigabitEthernet 0/1 - 2
   channel-group 1 mode active
   ```

   On the other switch:

   ```bash
   interface range gigabitEthernet 0/1 - 2
   channel-group 1 mode active
   ```

   This will configure the EtherChannel using LACP (Link Aggregation Control Protocol) for automatic negotiation.

### 6. **Verify Configuration**  
   Check VLAN configuration:

   ```bash
   show vlan brief
   ```

   Verify trunking:

   ```bash
   show interfaces trunk
   ```

   Verify Inter-VLAN Routing:

   ```bash
   ping 192.168.10.1
   ping 192.168.20.1
   ```

   Verify EtherChannel:

   ```bash
   show etherchannel summary
   ```

### 7. **Optional: Configure Spanning Tree Protocol (STP) to Avoid Loops**  
   If needed, configure STP to prevent loops in the network. This is automatically enabled, but you can adjust it as needed:

   ```bash
   spanning-tree vlan 10 priority 4096
   ```

## Why Use These Configurations?

- **VLANs**: VLANs help segment network traffic, improve security, and reduce congestion by isolating traffic based on departments or functions (e.g., Sales, Marketing).
- **Trunking**: 802.1Q trunking allows multiple VLANs to pass over a single link, making it efficient for interconnecting switches.
- **Inter-VLAN Routing**: This allows devices in different VLANs to communicate with each other through a router or Layer 3 switch.
- **EtherChannel**: EtherChannel aggregates multiple physical links to increase bandwidth and provide redundancy for critical links.



# Scenarios 6 and 7 Summary

This guide covers **Basic DHCPv4**, **Full DHCP Implementation**, and **Stateless & Stateful DHCPv6** to dynamically assign IP addresses in both IPv4 and IPv6 networks.  

---

##  1. Configuring DHCPv4  

### **1.1 Enable and Configure a DHCP Server on a Router**  
   Define a DHCP pool and specify key parameters:  

   ```bash
   configure terminal
   ip dhcp excluded-address 192.168.1.1 192.168.1.10  # Exclude static IPs

   ip dhcp pool LAN
   network 192.168.1.0 255.255.255.0
   default-router 192.168.1.1
   dns-server 8.8.8.8
   lease 7  # Lease time (days)
   ```

### **1.2 Verify DHCP Pool and Active Leases**  
   Check configured DHCP pools:  

   ```bash
   show ip dhcp pool
   ```

   View assigned IPs and active leases:  

   ```bash
   show ip dhcp binding
   ```

### **1.3 Enable DHCP Relay (If DHCP Server is on Another Network)**  
   If the DHCP server is not on the same network as clients, configure the router interface as a relay:  

   ```bash
   interface gigabitEthernet 0/0
   ip helper-address <dhcp_server_ip>
   ```

---

##  2. Configuring Stateless & Stateful DHCPv6  

### **2.1 Enable IPv6 Routing**  
   IPv6 routing must be enabled for DHCPv6 to function:  

   ```bash
   configure terminal
   ipv6 unicast-routing
   ```

### **2.2 Configure Stateful DHCPv6** (Similar to DHCPv4 - Assigns Full IP Address + Other Configurations)  
   - Clients get their **IPv6 address + default gateway + DNS from the DHCPv6 server**  
   - Used when the router **does not** provide SLAAC-based addressing  

   ```bash
   ipv6 dhcp pool DHCPv6-STATEFUL
   address prefix 2001:DB8:1:1::/64
   dns-server 2001:4860:4860::8888
   ```

   Apply to an interface:  

   ```bash
   interface gigabitEthernet 0/1
   ipv6 address 2001:DB8:1:1::1/64
   ipv6 dhcp server DHCPv6-STATEFUL
   ```

### **2.3 Configure Stateless DHCPv6** (Clients Get Only Additional Info, Not an IP)  
   - **IPv6 address is assigned via SLAAC (Router Advertisement)**
   - DHCPv6 only provides **DNS server information**  

   ```bash
   ipv6 dhcp pool DHCPv6-STATELESS
   dns-server 2001:4860:4860::8888
   ```

   Apply to an interface:  

   ```bash
   interface gigabitEthernet 0/1
   ipv6 address 2001:DB8:1:1::1/64
   ipv6 nd other-config-flag  # Enables Stateless DHCPv6
   ipv6 dhcp server DHCPv6-STATELESS
   ```

### **2.4 Verify DHCPv6 Configuration**  
   ```bash
   show ipv6 dhcp pool
   show ipv6 interface brief
   ```

---

## Why Use DHCP?  

- **IPv4: Automates IP Address Management** → Eliminates manual IP assignments.  
- **IPv6 Stateless (SLAAC + DHCPv6)** → Only gives clients DNS info while letting routers handle addressing.  
- **IPv6 Stateful (Full DHCPv6)** → Works like DHCPv4, assigning full addresses + other config data.  
- **Supports Centralized IP Management** → A single DHCP server can serve multiple subnets via relay.


# Scenario 8 Summary

This guide covers how to configure and secure switchports using VLAN trunking (802.1Q) and various security best practices. The implementation includes configuring access ports, securing unused switchports, applying port security measures, enabling DHCP snooping, and deploying PortFast with BPDU Guard. The final step ensures verification of end-to-end connectivity.

## Key Steps

### Implement 802.1Q Trunking
Configure VLAN trunks to allow traffic from multiple VLANs:
```sh
interface GigabitEthernet1/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 99
```

### Configure Access Ports
Assign switchports to specific VLANs:
```sh
interface GigabitEthernet1/0/2
 switchport mode access
 switchport access vlan 10
 switchport nonegotiate
```

### Secure and Disable Unused Switchports
Shut down unused ports to prevent unauthorized access:
```sh
interface range GigabitEthernet1/0/10-24
 switchport mode access
 switchport access vlan 999
 shutdown
```

### Implement Port Security
Enable MAC address restrictions on access ports:
```sh
interface GigabitEthernet1/0/3
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
```

### Implement DHCP Snooping Security
Prevent rogue DHCP servers and mitigate address spoofing:
```sh
ip dhcp snooping
ip dhcp snooping vlan 10,20,30
interface GigabitEthernet1/0/1
 ip dhcp snooping trust
```

### Implement PortFast and BPDU Guard
Enable PortFast on access ports to speed up network convergence:
```sh
interface GigabitEthernet1/0/4
 spanning-tree portfast
 spanning-tree bpduguard enable
```

### Verify End-to-End Connectivity
Check configuration and connectivity status:
```sh
show vlan brief
show interfaces trunk
show port-security
show dhcp snooping binding
show spanning-tree summary
```

## Why Secure Switchports?
Switch security ensures network integrity by preventing unauthorized access, mitigating risks like rogue DHCP servers, and protecting against accidental or malicious topology changes. Implementing these measures enhances overall network stability and security.



# Scenario 9 Summary

This guide details the configuration of a wireless network, covering router login, SSID and security setup, DHCP configuration, administrative credential updates, client connection, and optional access point integration.

## Key Steps

### Part 1: Log into the Wireless Router
- Connect the computer to the router via Ethernet.
- Configure the computer’s IP to match the router’s default network.
- Access the router's web interface using its IP address and login credentials.

### Part 2: Configure Basic Wireless Settings
- **SSID Setup**: Set the network name to `CCNAlab`.
- **Security**: Enable WPA2 with AES encryption and use `Cisco12345!` as the password.
- **DHCP Settings**: Assign the router’s IP, enable DHCP, and define the address range.
- **Admin Password**: Change the default password to `Cisco123!` and re-login.

### Part 3: Connect a Wireless Client
- Plug in a wireless adapter, connect to `CCNAlab`, and enter the Wi-Fi password.
- Verify the client’s network configuration using `ipconfig`.

### Part 4: Connect an Access Point (Optional)
- Extend the wireless network by connecting an AP to the router via Ethernet.
- Disable the AP’s DHCP server and configure it within the same subnet.

## Why wireless network
A properly configured wireless network enhances security, prevents unauthorized access, and ensures reliable connectivity for devices.


# Scenario 10 Summary

This guide explains how to configure static and default routes on Cisco devices to ensure proper network connectivity and traffic forwarding.

## Key Steps

### Part 1: Configure Static Routes
- Enter global configuration mode:
  ```sh
  configure terminal
  ```
- Define a static route:
  ```sh
  ip route <destination_network> <subnet_mask> <next_hop_ip>
  ```
- Example:
  ```sh
  ip route 192.168.2.0 255.255.255.0 192.168.1.1
  ```
- Exit configuration mode and save settings:
  ```sh
  end
  write memory
  ```

### Part 2: Configure a Default Route
- Configure a default route to forward unknown traffic to a specified gateway:
  ```sh
  ip route 0.0.0.0 0.0.0.0 <next_hop_ip>
  ```
- Example:
  ```sh
  ip route 0.0.0.0 0.0.0.0 192.168.1.254
  ```
- Save the configuration.

### Part 3: Verify Routing Configuration
- Display the routing table:
  ```sh
  show ip route
  ```
- Test connectivity using ping:
  ```sh
  ping <destination_ip>
  ```
- If issues occur, use:
  ```sh
  traceroute <destination_ip>
  ```

## Why Configure Static and Default Routes?
Static and default routes provide manual control over network traffic, ensuring efficient and predictable routing, particularly in smaller networks where dynamic routing is unnecessary. They help optimize traffic flow and maintain connectivity in environments without frequent topology changes.



#Scenario 11 Summary

This guide provides the necessary steps to configure OSPF in a single area on Cisco routers, including basic configuration, verifying OSPF, and adjusting key settings like Router ID, passive interfaces, and metrics.

### Key Steps

#### Build the Network and Configure Basic Device Settings
1. **Configure Basic Device Settings**  
   Set the hostname, configure IP addresses on interfaces, and enable interfaces.

   ```bash
   hostname R1
   ip address 192.168.12.1 255.255.255.0
   no shutdown
   ```

#### Configure and Verify OSPF Routing
2. **Enable OSPF Routing**  
   Enable OSPF on each router and configure the network statements for area 0.

   ```bash
   router ospf 1
   network 192.168.12.0 0.0.0.255 area 0
   ```

3. **Verify OSPF Configuration**  
   Use the following commands to verify OSPF neighbors and routes.

   ```bash
   show ip ospf neighbor
   show ip route ospf
   ```

#### Change Router ID Assignments
4. **Set Router ID**  
   Manually configure the Router ID to ensure a unique identifier.

   ```bash
   router ospf 1
   router-id 1.1.1.1
   ```

5. **Verify Router ID**  
   Check the Router ID to ensure it's correctly configured.

   ```bash
   show ip ospf
   ```

#### Configure OSPF Passive Interfaces
6. **Set Passive Interfaces**  
   Configure interfaces where OSPF should not form neighbor relationships.

   ```bash
   router ospf 1
   passive-interface gigabitethernet0/1
   ```

7. **Verify Passive Interfaces**  
   Use the command to verify the passive interfaces configuration.

   ```bash
   show ip ospf interface
   ```

#### Change OSPF Metrics
8. **Adjust OSPF Cost**  
   Change the OSPF cost on interfaces to influence routing decisions.

   ```bash
   interface gigabitethernet0/0
   ip ospf cost 10
   ```

9. **Verify OSPF Metric**  
   Check the updated OSPF metric.

   ```bash
   show ip ospf interface gigabitethernet0/0
   ```

---

### Why Use OSPF?

OSPF (Open Shortest Path First) is a widely used link-state routing protocol that provides fast convergence and scalability, making it ideal for larger, more complex networks. OSPF allows routers to dynamically exchange routing information, ensuring that the best paths are always used for data transmission. It also supports hierarchical network design through the use of areas, which helps in optimizing routing efficiency and reducing overhead. By using OSPF, network administrators can ensure efficient, scalable, and reliable routing across various types of networks.

