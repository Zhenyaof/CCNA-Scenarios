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
