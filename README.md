# Performing Network Activities in Azure

**Part 1 of the "Networking in Azure" Lab Series**

A practical, hands-on lab designed to teach core networking concepts in Microsoft Azure using Virtual Machines, Virtual Networks, Network Security Groups, and protocol analysis with Wireshark.

---

## 📋 Table of Contents

1. [Lab Overview](#-lab-overview)
2. [Prerequisites](#-prerequisites)
3. [Part 1: Create Virtual Machines](#part-1-create-our-virtual-machines)
4. [Part 2: Observe ICMP Traffic](#part-2-observe-icmp-traffic)
5. [Part 3: Advanced Traffic Analysis](#part-3-advanced-traffic-analysis)
6. [Lab Cleanup](#-lab-cleanup)
7. [Learning Objectives](#-learning-objectives)
8. [Additional Resources](#-additional-resources)

---

## 📘 Lab Overview

This lab guides you through building a small Azure networking environment and analyzing real network traffic including **ICMP, SSH, DHCP, DNS, and RDP**.

You will also learn how to control traffic flow using **Network Security Groups (NSGs)**.

---

## ✅ Prerequisites

- Active Microsoft Azure subscription
- [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466) (for macOS users)
- Basic understanding of Windows and Linux command line

---

## Part 1: Create our Virtual Machines

1. Go to the [Azure Portal](https://portal.azure.com/)

2. **Create a Resource Group**
   - Name example: `Networking-Lab-RG`

3. **Create a Windows 10 Virtual Machine**
   - Select the Resource Group created above
   - Let Azure create a new **Virtual Network** and **Subnet**
   - VM Size recommendation: `Standard_B2s` or `Standard_D2s_v3`

   ![Windows 10 VM Creation](screenshots/01-windows-vm-creation.png)

4. **Create a Linux (Ubuntu) Virtual Machine**
   - Use the **same Resource Group**
   - Use the **same Virtual Network and Subnet**
   - Authentication Type: **Password**
   - Username: `labuser` (recommended)

   ![Ubuntu VM Creation](screenshots/02-ubuntu-vm-creation.png)

5. Verify both VMs are in the **same Virtual Network / Subnet**

> 💡 **Important**: Do not delete the VMs after this part. Keep them running for Part 2.

---

## Part 2: Observe ICMP Traffic

6. Connect to your **Windows 10 VM** using Remote Desktop.

7. Install **Wireshark** on the Windows VM.

8. Open Wireshark and start a packet capture.

9. Apply filter: `icmp`

10. Get the private IP of your Ubuntu VM and ping it from the Windows VM.

    ![ICMP Traffic in Wireshark](screenshots/03-icmp-traffic.png)

11. Ping a public site:
    ```powershell
    ping www.google.com

Part 3: Advanced Traffic Analysis
Configuring a Firewall (Network Security Group)

Start a continuous ping:PowerShellping <ubuntu-private-ip> -t
Go to the Network Security Group of the Ubuntu VM:
Add rule to Deny inbound ICMP
Observe ping failing
Re-enable ICMP
Observe ping resuming
<img src="screenshots/04-nsg-icmp-rule.png" alt="NSG Rule Modification">

Observe SSH Traffic

In Wireshark, filter: ssh or tcp.port == 22
SSH into Ubuntu from Windows:PowerShellssh labuser@<ubuntu-private-ip><img src="screenshots/05-ssh-traffic.png" alt="SSH Traffic in Wireshark">

Observe DHCP Traffic

Filter: dhcp
Renew IP address:PowerShellipconfig /renew

Observe DNS Traffic

Filter: dns
Run:PowerShellnslookup google.com
nslookup disney.com

Observe RDP Traffic

Filter: tcp.port == 3389
Question: Why is there constant RDP traffic?

Answer: RDP continuously streams the desktop screen, so traffic flows constantly (unlike on-demand protocols like ping or SSH).

🧹 Lab Cleanup (DON’T FORGET!)

Close Remote Desktop connection
Delete the Resource Group (Networking-Lab-RG)
Verify that all resources are deleted in Azure

Additional Resources
https://learn.microsoft.com/en-us/azure/virtual-network/
https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview
https://www.wireshark.org/docs/
