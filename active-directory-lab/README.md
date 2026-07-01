# Active Directory Homelab on AWS EC2

**Platform:** Amazon Web Services (EC2)  
**Server OS:** Windows Server 2022  
**Instance Type:** t3.micro  
**Domain:** lab.local  
**Category:** Systems Administration / Identity Management  
**Difficulty:** Beginner–Intermediate  
**Time to Complete:** ~3–4 hours  

---

## Overview

Built a fully functional Active Directory Domain Controller from scratch on AWS EC2. This lab simulates a real enterprise environment including domain configuration, departmental Organizational Units, user account creation, security group management, and Group Policy enforcement.

---

## Phase 1 — AWS EC2 Instance

Launched a Windows Server 2022 t3.micro instance on AWS EC2 in the us-east-1 (N. Virginia) region. The instance was named `AD-Domain-Controller` and configured with a custom security group restricting RDP access to a single trusted IP address.

![EC2 Instance Running](./screenshots/01-ec2-instance-running_png.png)

**Key configuration details:**
- Instance ID: i-087e6184042a4a171
- Instance type: t3.micro
- Region: us-east-1a
- Access: RDP restricted to trusted IP only (port 3389)

---

## Phase 2 — Active Directory Domain Services

After connecting to the server via RDP, installed the Active Directory Domain Services (AD DS) role through Server Manager and promoted the server to a Domain Controller for the `lab.local` forest.

### Organizational Unit Structure

Created a departmental OU structure to mirror a real business environment:

![AD Users and Computers - OU Structure](./screenshots/02-ad-users-and-computers_png.png)

**OUs created under lab.local:**
- HR Department
- IT Department
- Management
- Workstations

This structure allows Group Policies and permissions to be applied at the department level, reflecting how enterprise environments organize and manage users.

---

## Phase 3 — User Account Creation

Created domain user accounts within each OU. Users were assigned logon names following standard naming conventions (first initial + last name) and scoped to their respective departments.

![Creating User Account - John Smith](./screenshots/03-user-accounts_png.png)

**Example user created:**
- Name: John Smith
- Logon name: jsmith
- Domain: @lab.local
- OU: lab.local/IT Department

---

## Phase 4 — Security Groups

Created Security Groups within each OU to manage permissions and access control at the group level rather than the individual user level — a best practice in enterprise AD environments.

![Creating Security Group - IT-Staff](./screenshots/04-security-groups_png.png)

**Groups created:**
- IT-Staff (Global Security) — IT Department OU
- HR-Staff (Global Security) — HR Department OU
- Managers (Global Security) — Management OU

Users were then added to their respective groups, allowing permissions to be inherited by group membership.

---

## Phase 5 — Group Policy Configuration

Created and linked a Group Policy Object (GPO) to the IT Department OU to enforce desktop configuration standards across all users in that department.

![Group Policy Management Editor - Personalization Settings](./screenshots/05-gpo-configuration_png.png)

**GPO configured:** IT-Desktop-Policy  
**Path:** User Configuration → Policies → Administrative Templates → Control Panel → Personalization  
**Policy enforced:** Prevent changing desktop background

This demonstrates how IT administrators use GPOs to enforce consistent standards across users and departments without manual configuration on each machine.

---

## Skills Demonstrated

- AWS EC2 instance provisioning and security group configuration
- Windows Server 2022 administration via RDP
- Active Directory Domain Services (AD DS) installation and domain promotion
- Organizational Unit (OU) design and creation
- Domain user account creation and management
- Security group creation and membership management
- Group Policy Object (GPO) creation, linking, and configuration
- Least-privilege and group-based access control concepts

---

## What's Next

- Join a second EC2 instance to the domain as a workstation
- Use PowerShell to bulk-create user accounts via script
- Configure shared network folders with group-based permissions
- Set up DNS and DHCP manually to reinforce networking concepts

---

*Part of my [IT Troubleshooting & Homelab Portfolio](../README.md) — documenting real-world projects as I work toward CompTIA A+ and Network+ certification.*
