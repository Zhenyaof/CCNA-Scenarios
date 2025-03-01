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

# Cisco IPv6 Configuration Summary

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
