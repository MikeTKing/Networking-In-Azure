
# Performing Network Activities in Azure

A hands-on lab demonstrating core networking concepts using Azure Virtual Machines, Virtual Networks, Network Security Groups, and protocol analysis with Wireshark.

**Part of the "Networking in Azure" series.**

---

## 📋 Lab Overview

This lab walks through the creation of a multi-VM environment in Microsoft Azure and explores real-world network traffic including:

- **ICMP** (Ping)
- **SSH**
- **DHCP**
- **DNS**
- **RDP**

Students will also learn how to control traffic using **Network Security Groups (NSGs)**.

---

## 🛠️ Prerequisites

- Microsoft Azure account with credits (Free tier works for this lab)
- Windows, Mac, or Linux computer with internet access
- For Mac users: [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466) installed

---

## 🧪 Lab Steps

### Part 1: Create our Virtual Machines

1. Go to [Azure Portal](https://portal.azure.com/)

2. **Create a Resource Group**
   - Name: `Networking-Lab-RG` (or similar)

3. **Create a Windows 10 Virtual Machine**
   - Select the Resource Group created above
   - Allow Azure to create a new **Virtual Network** and **Subnet**
   - Recommended size: `Standard_B2s` or `Standard_D2s_v3`

4. **Create a Linux (Ubuntu) Virtual Machine**
   - Use the **same Resource Group**
   - Use the **same Virtual Network** and Subnet as the Windows VM
   - Authentication type: **Password**
   - Username: `labuser` (recommended)

5. Verify both VMs are in the **same Virtual Network / Subnet**

> 💡 Keep both VMs running for Part 2.

---

### Part 2: Observe ICMP Traffic

6. Connect to your **Windows 10 VM** using Remote Desktop.

7. Install **Wireshark** inside the Windows VM.

8. Open Wireshark and start a packet capture.

9. Filter for **ICMP** traffic.

10. Ping the **private IP** of the Ubuntu VM from the Windows VM and observe request/reply packets.

11. Ping a public website (`www.google.com`) and observe outbound ICMP traffic.

---

### Part 3: Advanced Traffic Analysis & Firewall Rules

#### Configuring a Firewall (Network Security Group)

12. Start a continuous ping from Windows VM → Ubuntu VM:
   ```powershell
   ping <ubuntu-private-ip> -t

Modify the Network Security Group attached to the Ubuntu VM:
Deny inbound ICMP traffic
Observe ping failing in Wireshark and command line
Re-enable ICMP traffic
Observe ping resuming


Observe SSH Traffic

In Wireshark, filter for: ssh or tcp.port == 22
From Windows VM, SSH into Ubuntu VM:

PowerShellssh labuser@<ubuntu-private-ip>

Run commands in the SSH session and observe encrypted traffic in Wireshark.

Observe DHCP Traffic

Filter in Wireshark: dhcp
Renew IP address on Windows VM:

PowerShellipconfig /renew
Observe DNS Traffic

Filter in Wireshark: dns
Run:

PowerShellnslookup google.com
nslookup disney.com
Observe RDP Traffic

Filter in Wireshark: tcp.port == 3389
Question: Why is RDP traffic constantly spamming compared to other protocols?

Answer: RDP provides a live graphical desktop stream, so data is continuously transmitted between client and server.

🧹 Lab Cleanup (Important!)

Disconnect from Remote Desktop
Delete the Resource Group created at the beginning of the lab
Confirm the Resource Group and all resources are deleted


🎯 Learning Objectives

Understand Azure Virtual Network and Subnet architecture
Analyze common network protocols using Wireshark
Work with Network Security Groups (NSGs)
Differentiate between various network traffic patterns
Practice proper Azure resource cleanup


📚 Additional Resources

Azure Virtual Network Documentation
Wireshark Documentation
Network Security Groups Overview
