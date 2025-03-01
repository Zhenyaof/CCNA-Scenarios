# CCNA Scenarios

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

Here’s a unified **Cisco DHCP configuration** guide covering both **Basic DHCPv4** and **DHCP Implementation** on routers and switches.  

---

# Scenarios 6 and 7 Summary 

This guide covers configuring **Basic DHCPv4** and **Full DHCP Implementation** on Cisco devices to automatically assign IP addresses to clients.  

## Key Steps  

### 1. **Enable and Configure a DHCP Server on a Router**  
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

### 2. **Verify DHCP Pool and Active Leases**  
   Check configured DHCP pools:  

   ```bash
   show ip dhcp pool
   ```

   View assigned IPs and active leases:  

   ```bash
   show ip dhcp binding
   ```

### 3. **Enable DHCP Relay (If DHCP Server is on Another Network)**  
   If the DHCP server is not on the same network as clients, configure the router interface as a relay:  

   ```bash
   interface gigabitEthernet 0/0
   ip helper-address <dhcp_server_ip>
   ```

### 4. **Configure a Switch to Forward DHCP Requests**  
   If using a switch, enable **DHCP Snooping** for security and to prevent rogue DHCP servers:  

   ```bash
   configure terminal
   ip dhcp snooping
   ip dhcp snooping vlan 1
   interface gigabitEthernet 0/1
   ip dhcp snooping trust
   ```

### 5. **Verify DHCP Relay and Snooping**  
   Check relay status:  

   ```bash
   show ip interface <interface_name>
   ```

   Check DHCP snooping status:  

   ```bash
   show ip dhcp snooping
   ```

### 6. **Test DHCP Assignment**  
   On a client device, ensure it gets an IP automatically:

   ```bash
   ipconfig /renew   # (Windows)
   ```

   or  

   ```bash
   dhclient eth0   # (Linux)
   ```

---

## Why Use DHCP?  

- **Automates IP Address Management**: Eliminates the need for manual IP assignments.  
- **Efficient and Scalable**: Ideal for large networks where devices frequently connect and disconnect.  
- **Supports Centralized IP Management**: With DHCP relay, a single DHCP server can serve multiple subnets.  
- **Improves Security**: Features like DHCP snooping prevent unauthorized DHCP servers from assigning incorrect IPs.  


