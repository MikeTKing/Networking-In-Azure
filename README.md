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
  
<img width="1872" height="918" alt="image" src="https://github.com/user-attachments/assets/4c5ff81f-f771-4bc8-a4de-1dd801f31dcc" />


3. **Create a Windows 10 Virtual Machine**
   - Select the Resource Group created above
   - Let Azure create a new **Virtual Network** and **Subnet**
   - VM Size recommendation: `Standard_B2s` or `Standard_D2s_v3`

   

4. **Create a Linux (Ubuntu) Virtual Machine**
   - Use the **same Resource Group**
   - Use the **same Virtual Network and Subnet**
   - Authentication Type: **Password**
   - Username: `labuser` 

   

5. Verify both VMs are in the **same Virtual Network / Subnet**

<img width="1876" height="924" alt="image" src="https://github.com/user-attachments/assets/4775045e-7f5e-4655-9a7e-480c43b6cf78" />


> 💡 **Important**: Do not delete the VMs after this part. Keep them running for Part 2.

---

## Part 2: Observe ICMP Traffic

6. Connect to your **Windows 10 VM** using Remote Desktop.

7. Install **Wireshark** on the Windows VM.

8. Open Wireshark and start a packet capture.

<img width="972" height="662" alt="image" src="https://github.com/user-attachments/assets/8b4511ec-0802-421d-8db5-bde3a27bea5e" />

9. Apply filter: `icmp`

<img width="1891" height="961" alt="image" src="https://github.com/user-attachments/assets/36731a32-5875-4303-8af4-6a8b36f65e15" />


11. Get the private IP of your Ubuntu VM and ping it from the Windows VM.

    <img width="1488" height="900" alt="image" src="https://github.com/user-attachments/assets/0b35fd55-e775-4f5b-9549-0ab3df246c06" />


12. Ping a public site:
    ```powershell
    ping www.google.com

Part 3: Advanced Traffic Analysis
Configuring a Firewall (Network Security Group)

Start a continuous ping:PowerShellping <ubuntu-private-ip> -t
Go to the Network Security Group of the Ubuntu VM:
Add rule to Deny inbound ICMP

<img width="1871" height="928" alt="image" src="https://github.com/user-attachments/assets/7d35a806-4831-4265-a26b-a114dd4b4a91" />

Observe ping failing

<img width="1107" height="100" alt="image" src="https://github.com/user-attachments/assets/b272abfc-6c9f-444b-8d43-77e53e8526d1" />


Re-enable ICMP
Observe ping resuming
<img width="1094" height="331" alt="image" src="https://github.com/user-attachments/assets/5dc1bc0e-f235-43b2-93ad-06a4afb31181" />



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
