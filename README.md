# Active Directory Home Lab

Built an Active Directory home lab using **Windows Server**, **Windows 10 Pro**, **DNS**, **Windows Defender Firewall**, and **VirtualBox**.  
This project was designed to simulate a small business Windows environment and practice core IT support tasks such as domain setup, user and group management, client domain join, DNS configuration, and firewall troubleshooting.

## Project Overview

In this lab, I:

- Installed and configured **Windows Server**
- Promoted the server to a **domain controller**
- Created the **`lab.local`** domain
- Created custom **Organizational Units (OUs)** for users and computers
- Created test user accounts and a **HelpDesk** security group
- Configured static IP addressing and DNS for the server and client
- Joined a **Windows 10 Pro** client to the domain
- Troubleshot a one-way connectivity issue caused by **Windows Defender Firewall**

## Technologies Used

- Windows Server
- Windows 10 Pro
- Active Directory Domain Services (AD DS)
- DNS
- Windows Defender Firewall
- VirtualBox

## Objectives

The goal of this project was to practice:

- Active Directory deployment
- Domain controller promotion
- DNS configuration for domain environments
- OU, user, and group creation
- Client domain join
- Basic Windows firewall troubleshooting
- IT support style connectivity testing using `ping` and `nslookup`

## Lab Environment

### Virtual Machines

**Domain Controller**
- Hostname: `DC1`
- Role: Domain Controller / DNS Server
- IP Address: `192.168.56.10`
- Domain: `lab.local`

**Client Machine**
- Hostname: `Client1`
- OS: Windows 10 Pro
- IP Address: `192.168.56.11`
- Joined to domain: `lab.local`

### Virtual Network

- Hypervisor: VirtualBox
- Network Type: **Internal Network**
- Network Name: `adlab`

This allowed both virtual machines to communicate on the same isolated network without using my physical home network.

## Build Process

### 1. Installed Windows Server
Created a Windows Server virtual machine in VirtualBox and completed the initial setup using the Desktop Experience version.

### 2. Renamed the Server
Renamed the server to:

`DC1`

This followed a standard naming convention for a domain controller.

### 3. Installed Active Directory Domain Services
Used **Server Manager** to install the **Active Directory Domain Services (AD DS)** role and the required supporting features.

### 4. Promoted the Server to a Domain Controller
Promoted the server to a domain controller and created a new forest with the domain:

`lab.local`

### 5. Created Custom OUs, Users, and Group
Inside **Active Directory Users and Computers**, I created custom OUs:

- `Corp_Users`
- `Corp_Computers`

I also created test user accounts and a security group:

- Users:
  - John Smith
  - Mark Black
- Group:
  - `HelpDesk` (Global Security Group)

### 6. Configured Networking
Configured both virtual machines with static IP settings.

**Server (`DC1`)**
- IP Address: `192.168.56.10`
- Subnet Mask: `255.255.255.0`
- Preferred DNS: `192.168.56.10`

**Client (`Client1`)**
- IP Address: `192.168.56.11`
- Subnet Mask: `255.255.255.0`
- Preferred DNS: `192.168.56.10`

The client used the domain controller as its DNS server so it could resolve the `lab.local` domain.

### 7. Tested Connectivity
Verified network communication from the client to the server using:

- `ping`
- `nslookup`

The client successfully resolved:

`lab.local -> 192.168.56.10`

### 8. Joined the Client to the Domain
Joined the Windows 10 Pro client to:

`lab.local`

After restarting, the system showed that `Client1` was successfully joined to the domain.

## Troubleshooting

### Issue: One-Way Ping Connectivity
During testing, the **client could ping the server**, but the **server could not ping the client**.

### Root Cause
The issue was caused by **Windows Defender Firewall** on the client, which was blocking inbound ICMP echo requests.

### Fix
Enabled the inbound firewall rule for:

`File and Printer Sharing (Echo Request - ICMPv4-In)`

After enabling the rule, the server was able to successfully ping the client.

### Commands Used

```cmd
ipconfig
ping 192.168.56.10
ping 192.168.56.11
nslookup lab.local
