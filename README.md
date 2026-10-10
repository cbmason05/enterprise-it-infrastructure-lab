# Enterprise IT Infrastructure Lab

### Windows Server | Active Directory | Networking | Systems Administration | Azure Cloud

## Project Overview

Mason Technologies is a simulated organization with approximately 50 employees across four departments: IT, Human Resources, Finance, and Sales.

This project involves designing, deploying, securing, and managing an enterprise-style IT infrastructure using Windows Server, Active Directory, and Microsoft technologies.

The goal is to simulate real-world systems administration responsibilities, including identity management, network services, endpoint administration, access control, troubleshooting, and infrastructure automation.

The project is developed in multiple phases, progressing from traditional on-premises Windows Server infrastructure toward hybrid cloud administration, Microsoft Azure, and infrastructure automation.

**Current Status:** Phase 1 — Infrastructure deployed and validated; final employee and group counts pending confirmation

---

## Business Scenario

Mason Technologies requires a centralized IT environment to support its employees.

The organization needs to:

- Manage employee identities and authentication.
- Organize users by department.
- Enforce consistent workstation security policies.
- Automatically configure employee network connections.
- Provide centralized departmental file storage.
- Restrict access to sensitive company information.
- Support future integration with Microsoft Azure and cloud services.

To address these requirements, a Windows Server-based infrastructure was deployed using Oracle VirtualBox.

---

## Infrastructure Architecture

The Phase 1 environment consists of three virtual machines connected through an isolated internal network.
```text
MASON TECHNOLOGIES
ad.masontech.test
|
MasonTech-LAN
192.168.50.0/24
|
+-----------------+------------------+
|                 |                  |
|                 |                  |
DC01               FS01              CLIENT01
192.168.50.10      192.168.50.20       DHCP Assigned
|                 |                  |
Active Directory    File Server       Windows 11 Pro
DNS                 SMB Shares        Domain Joined
DHCP                NTFS Permissions  Group Policy
Group Policy        Department Files  Employee Access
```
### Virtual Machine Inventory

| Machine | Role | Network Configuration |
|---|---|---|
| DC01 | Active Directory Domain Controller, DNS, DHCP | 192.168.50.10 |
| FS01 | Windows Server File Server | 192.168.50.20 |
| CLIENT01 | Windows 11 Pro Domain Workstation | DHCP |

**Domain:** `ad.masontech.test`

**Network:** `192.168.50.0/24`

**Virtualization Platform:** Oracle VirtualBox

All virtual machines communicate over an isolated VirtualBox internal network named `MasonTech-LAN`.

---

## Active Directory Design

A centralized Active Directory domain was deployed to manage employee identities, computer accounts, and security groups.

### Organizational Unit Structure
```text
ad.masontech.test
|
+-- MasonTech
|
+-- IT
+-- HR
+-- Finance
+-- Sales
+-- Workstations
+-- Servers
+-- Service Accounts
```
### Department Structure

| Department | Planned Employee Accounts | Security Group |
|---|---:|---|
| IT | 10 | SG-IT |
| Human Resources | 10 | SG-HR |
| Finance | 10 | SG-Finance |
| Sales | 20 | SG-Sales |
| **Total** | **50** | **4 groups** |

Departmental security groups are used to assign access permissions to shared resources.

This approach reduces the need to manage permissions individually and supports role-based access control.

---

## Phase 1 — Windows Server Infrastructure

**Status: Infrastructure deployed and end-to-end tests passed; final account-count audit pending**

### 1. Virtual Machine Deployment

Deployed Windows Server and Windows 11 virtual machines using Oracle VirtualBox.

Configured virtual networking, static server IP addresses, and an isolated lab environment.

[View VM Setup Documentation](phase-1/01-vm-setup.md)

### 2. Active Directory Domain Services

Installed and configured Active Directory Domain Services.

Created the `ad.masontech.test` domain and organizational units, and developed departmental account provisioning and security group configuration. Final account and group membership totals remain to be verified.

Implemented centralized identity management.

[View Active Directory Documentation](phase-1/02-active-directory.md)

### 3. Group Policy Management

Configured domain security policies and departmental workstation restrictions.

Implemented:

- Domain password complexity requirements
- Minimum password length of 12 characters
- Account lockout after five failed sign-in attempts
- Workstation inactivity limit of 15 minutes
- Command Prompt restrictions for HR employees

Validated Group Policy application on CLIENT01.

[View Group Policy Documentation](phase-1/03-group-policy.md)

### 4. DHCP Server Configuration

Installed and authorized DHCP on DC01.

Configured the following workstation scope:

| Setting | Value |
|---|---|
| Scope | MasonTech-Workstations |
| Address Range | 192.168.50.100–192.168.50.150 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 192.168.50.10 |
| DNS Domain | ad.masontech.test |

Verified automatic IP address assignment on CLIENT01.

[View DHCP Documentation](phase-1/04-dhcp.md)

### 5. File Server Deployment

Deployed FS01 as a dedicated Windows Server file server.

Created departmental SMB shares:

- `\\FS01\IT`
- `\\FS01\HR`
- `\\FS01\Finance`
- `\\FS01\Sales`

Configured NTFS permissions and SMB share permissions using Active Directory security groups.

Validated authorized and unauthorized access from CLIENT01.

[View File Server Documentation](phase-1/05-file-server.md)

### 6. Windows 11 Workstation Integration

Deployed CLIENT01 running Windows 11 Pro.

Joined the workstation to the Active Directory domain and moved its computer account into the Workstations OU.

Validated:

- DHCP address assignment
- DNS resolution
- Domain user authentication
- Group Policy application
- Departmental file share access
- Unauthorized resource access prevention

[View Client Workstation Documentation](phase-1/06-client-workstation.md)

---

## Security Implementation

The environment incorporates several foundational enterprise security practices.

| Security Control | Implementation |
|---|---|
| Centralized authentication | Active Directory |
| Password policies | Default Domain Policy |
| Account lockout | Group Policy |
| Workstation inactivity lock | Workstation security GPO |
| Department restrictions | HR Group Policy |
| Role-based access control | AD security groups |
| File system access control | NTFS permissions |
| Network file access control | SMB share permissions |
| Network isolation | VirtualBox internal network |

These controls demonstrate centralized identity administration and least-privilege access management.

---

## End-to-End Testing

The completed environment was tested using CLIENT01.

| Test | Result |
|---|---|
| DHCP address assignment | Passed |
| DNS name resolution | Passed |
| DC01 connectivity | Passed |
| FS01 connectivity | Passed |
| Active Directory domain join | Passed |
| Employee domain authentication | Passed |
| Workstation Group Policy application | Passed |
| HR Group Policy restriction | Passed |
| Authorized HR file access | Passed |
| Unauthorized Finance access denied | Passed |

### Example Access Control Test

Authenticated to CLIENT01 using the domain account:

`MASONTECH\hr01`

**Authorized Access**

Successfully accessed:

`\\FS01\HR`

Created a test file in the departmental share.

**Unauthorized Access**

Attempted to access:

`\\FS01\Finance`

Access was denied as intended.

This confirmed that Active Directory group membership, SMB share permissions, and NTFS permissions were functioning together.

---

## Technologies and Skills Demonstrated

### Systems Administration

- Windows Server
- Active Directory Domain Services
- Domain Controller Deployment
- Organizational Units
- User and Group Administration
- Group Policy Management
- Windows 11 Domain Integration

### Networking

- TCP/IP
- DNS
- DHCP
- Static IP Configuration
- Network Troubleshooting
- Internal Virtual Networking

### Identity and Security

- Active Directory Authentication
- Security Groups
- Role-Based Access Control
- Least Privilege
- NTFS Permissions
- SMB Share Permissions
- Account Security Policies

### Tools

- Oracle VirtualBox
- Windows PowerShell
- Active Directory Users and Computers
- Group Policy Management Console
- DHCP Management Console
- Server Manager
- Windows Administrative Tools

---

## Project Roadmap

### Phase 1 — On-Premises Windows Server Infrastructure

**Completed**

- Active Directory
- DNS
- DHCP
- Group Policy
- File Server
- Windows 11 Workstation
- End-to-End Integration Testing

### Phase 2 — Microsoft Azure and Hybrid Infrastructure

**Planned**

- Azure Resource Groups
- Azure Virtual Networks and Subnets
- Azure Virtual Machines
- Network Security Groups
- Microsoft Entra ID
- Azure Role-Based Access Control
- Hybrid Identity Integration
- Azure Storage and Monitoring

### Phase 3 — Infrastructure Automation

**Planned**

- PowerShell Administration Scripts
- Python Automation
- Terraform Infrastructure as Code
- Ansible Configuration Management
- Git-Based Infrastructure Documentation

### Phase 4 — Cloud and DevOps Infrastructure

**Planned**

- Docker
- Kubernetes
- CI/CD Concepts
- Jenkins
- Prometheus
- Grafana
- Application Deployment and Monitoring

---

## Challenges and Troubleshooting

Several technical challenges were encountered during implementation.

**DNS Resolution on FS01**

FS01 initially communicated with DC01 by IP address but could not resolve domain names.

The DNS client configuration was corrected to use DC01 at `192.168.50.10`, restoring internal name resolution.

**Windows 11 Offline Installation**

CLIENT01 required an offline installation process because the lab network was isolated from the internet.

**Active Directory Domain Join**

CLIENT01 initially encountered an access-denied error when joining the domain. After troubleshooting, the domain join succeeded.

**Group Policy Application**

CLIENT01 was moved into the Workstations OU to ensure the appropriate computer policies applied.

These challenges provided practical experience with troubleshooting Windows Server, Active Directory, DNS, and endpoint configuration.

---

## Project Documentation

| Document | Description |
|---|---|
| [01 — VM Setup](phase-1/01-vm-setup.md) | Virtual machines, operating systems, and networking |
| [02 — Active Directory](phase-1/02-active-directory.md) | Domain deployment, OUs, users, and groups |
| [03 — Group Policy](phase-1/03-group-policy.md) | Security policies and departmental restrictions |
| [04 — DHCP](phase-1/04-dhcp.md) | DHCP server, scope configuration, and validation |
| [05 — File Server](phase-1/05-file-server.md) | SMB shares, NTFS permissions, and access control |
| [06 — Client Workstation](phase-1/06-client-workstation.md) | Windows 11 domain integration and testing |

Screenshot evidence is included within the six Phase 1 technical documents, alongside the relevant implementation and verification steps.

---

## Project Outcomes

Phase 1 successfully established a functioning, isolated Windows domain environment.

The project demonstrated the ability to:

1. Deploy and configure Windows Server infrastructure.
2. Implement centralized Active Directory identity management.
3. Configure essential network services.
4. Manage users, computers, and security policies.
5. Deploy a dedicated file server.
6. Implement and validate departmental access controls.
7. Troubleshoot domain connectivity and authentication issues.
8. Validate infrastructure functionality using a domain-joined workstation.

The completed environment provides a foundation for expanding into Azure administration, hybrid identity, and infrastructure automation.

---

## Disclaimer

Mason Technologies is a fictional organization created for educational and portfolio purposes.

All users, departments, infrastructure, and company resources in this project are simulated within a personal virtual lab.

The project is not associated with a real business or production environment.
