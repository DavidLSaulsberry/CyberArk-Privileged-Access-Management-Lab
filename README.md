# CyberArk Privileged Access Management Lab

**VMware Workstation | CyberArk PAM | Active Directory | Windows Server**

Hi,

Here is a hands-on CyberArk Privileged Access Management (PAM) lab where I designed and deployed a multi-server enterprise PAM environment using VMware Workstation, Active Directory, and CyberArk components including the Digital Vault, CPM, PSM, and PVWA.

The project focused on privileged account security, vault architecture, PAM infrastructure deployment, Active Directory integration, static networking, and enterprise-grade privileged access management configuration.

---

## Table of Contents

1. [Lab Overview](#lab-overview)

2. [Environment](#environment)

3. [Architecture](#architecture)

4. [Part 1 — Virtual Machine Deployment](#part-1--virtual-machine-deployment)

5. [Part 2 — Active Directory & Networking Configuration](#part-2--active-directory--networking-configuration)

6. [Part 3 — CyberArk Server Configuration](#part-3--cyberark-server-configuration)

7. [Part 4 — CyberArk Vault & Component Validation](#part-4--cyberark-vault--component-validation)

8. [Part 5 — CyberArk Web Portal & PAM Operations](#part-5--cyberark-web-portal--pam-operations)

9. [Key Lessons Learned](#key-lessons-learned)

10. [Tools Used](#tools-used)

11. [What I'd Improve in Production](#what-id-improve-in-production)

---

## Lab Overview

The goal of this lab was to build a realistic CyberArk Privileged Access Management environment similar to what would exist in a production enterprise network.

The environment included:

* **Windows Server Active Directory Domain Controller**

* **CyberArk Digital Vault**

* **CyberArk CPM (Central Policy Manager)**

* **CyberArk PSM (Privileged Session Manager)**

* **CyberArk PVWA (Password Vault Web Access)**

* **Static enterprise networking configuration**

* **Domain-based authentication and server communication**

The project provided hands-on experience with:

* PAM architecture design

* Secure privileged credential storage

* Active Directory integration

* Vault services and validation

* Privileged session infrastructure

* CyberArk component deployment

* Service account management

* Enterprise server hardening preparation

---

## Environment

| Component           | Detail               |
| ------------------- | -------------------- |
| Hypervisor          | VMware Workstation   |
| Domain Controller   | Windows Server       |
| PAM Platform        | CyberArk             |
| Vault Component     | Digital Vault        |
| Password Management | CPM                  |
| Session Isolation   | PSM                  |
| Web Interface       | PVWA                 |
| Authentication      | Active Directory     |
| Networking          | Static IP Addressing |
| Domain              | `pitythefool.com`    |
| Scripting           | PowerShell           |
| Operating System    | Windows Server 2022  |

---

## Architecture

The CyberArk PAM lab was built using multiple virtual machines separated by server role.

> **<img width="917" height="445" alt="Screenshot 2025-12-01 182216" src="https://github.com/user-attachments/assets/4cf48128-04bc-4b18-95a6-c86054a503f9" />**
> — VMware Workstation showing the Active Directory, Vault, and CyberArk component virtual machines

---

## Part 1 — Virtual Machine Deployment

The lab environment was deployed in VMware Workstation using dedicated virtual machines for each CyberArk component.

### Virtual Machines Created

* WIN-ADDC — Active Directory Domain Controller

* WIN-VAULT — CyberArk Digital Vault

* WIN-CPM — Central Policy Manager

* WIN-PSM — Privileged Session Manager

* WIN-PVWA — Password Vault Web Access

Each server was isolated by role to follow enterprise PAM architecture best practices.

### Initial Configuration Tasks

* Installed Windows Server on all VMs

* Configured static IP addressing

* Assigned unique hostnames

* Verified VM-to-VM communication

* Configured DNS settings

* Validated network connectivity between systems

### Snapshot Management

Snapshots were created throughout deployment to preserve clean rollback points before major CyberArk component installations.

This allowed quick recovery during troubleshooting and configuration testing.

---

## Part 2 — Active Directory & Networking Configuration

A dedicated Active Directory domain controller was configured to support centralized authentication and domain services.

### Domain Configuration

```

pitythefool.com

```

The domain controller (`WIN-ADDC`) was assigned:

```

IP Address: 192.168.100.10

```

### Static Networking

Each CyberArk server was assigned a unique static IP address.

| Server    | IP Address       |
| --------- | ---------------- |
| WIN-ADDC  | `192.168.100.10` |
| WIN-VAULT | `192.168.100.11` |
| WIN-CPM   | `192.168.100.12` |
| WIN-PVWA  | `192.168.100.13` |
| WIN-PSM   | `192.168.100.14` |

### DNS Configuration

All CyberArk servers used the domain controller as their preferred DNS server to ensure:

* Proper domain resolution

* Active Directory communication

* Component discovery

* Reliable authentication

### Domain Join

The following servers were joined to the domain:

* WIN-CPM

* WIN-PVWA

* WIN-PSM

The Vault server remained outside the domain in a workgroup configuration for security hardening purposes, following CyberArk best practices.

### Connectivity Validation

Network communication between all systems was validated using:

```cmd

ping WIN-ADDC
ping WIN-VAULT
ping WIN-CPM
ping WIN-PSM
ping WIN-PVWA

```

---

## Part 3 — CyberArk Server Configuration

Each CyberArk component server was configured according to its enterprise role.

### Digital Vault

The Digital Vault acts as the core secure credential storage system within the PAM architecture.

Responsibilities included:

* Secure privileged credential storage

* Encryption and vault services

* Credential isolation

* Secure authentication handling

### CPM — Central Policy Manager

The CPM server was configured to:

* Rotate privileged passwords

* Enforce credential policies

* Manage account password changes

* Automate credential lifecycle operations

### PSM — Privileged Session Manager

The PSM server was configured to:

* Proxy privileged sessions

* Isolate administrator access

* Secure remote privileged connections

* Provide monitored session access

### PVWA — Password Vault Web Access

The PVWA server provided:

* Web-based administrative access

* Credential management interface

* PAM administration

* Reporting and audit visibility

---

## Part 4 — CyberArk Vault & Component Validation

After deployment, CyberArk services and vault operations were validated to confirm all PAM infrastructure components were functioning properly.

### PrivateArk Validation

The PrivateArk client successfully connected to the production vault environment and displayed:

* System safes

* Vault internal structures

* PVWA reports

* Administrative components

* Secure vault containers

> **<img width="512" height="356" alt="Screenshot 2025-12-01 182637" src="https://github.com/user-attachments/assets/91012a7a-9bb6-4599-b122-827905b81110" />**
> — PrivateArk client connected to the production vault showing internal vault structures and administrative components

### Service Validation

Windows services were validated directly through Command Prompt using:

```cmd

sc query "PrivateArk Server"
sc query "PrivateArk Database"
sc query "CyberArk Logic Container"

```

All critical CyberArk services successfully returned:

```

STATE              : 4  RUNNING

```

> **<img width="627" height="347" alt="Screenshot 2025-12-01 182919" src="https://github.com/user-attachments/assets/4691d5a2-12fd-402a-b212-b94c92a4f27a" />**
> — Command Prompt validating that CyberArk Vault services and components are actively running

---

## Part 5 — CyberArk Web Portal & PAM Operations

The CyberArk Password Vault Web Access (PVWA) portal was successfully deployed and configured.

The dashboard displayed operational status and connectivity for:

* Web Portal

* CPM

* Discovery Accounts

* PSM

* PSM for SSH

* Application User Instances

### PAM Administrative Features

The environment included access to:

* Accounts

* Policies

* Applications

* Reports

* User Provisioning

* Administration

* System Health Monitoring

### Health Validation

All major CyberArk infrastructure components reported healthy operational status and active connectivity.

> **<img width="1726" height="903" alt="Screenshot 2025-12-01 182140" src="https://github.com/user-attachments/assets/1f3fc597-aa14-45a2-8a82-ec2f706bb7e2" />**
> — CyberArk PVWA dashboard showing component health, PAM administration modules, and connected infrastructure services

---

## Key Lessons Learned

### 1. PAM infrastructure requires strict server role separation

Separating Vault, CPM, PSM, and PVWA into dedicated systems improves security and follows enterprise PAM design standards.

### 2. The Vault is the most security-critical component

The CyberArk Digital Vault acts as the core trust boundary for the entire PAM environment and requires additional hardening and isolation.

### 3. Static networking is critical for enterprise infrastructure

Consistent static IP addressing simplifies component communication, DNS resolution, and PAM configuration.

### 4. Active Directory integration is foundational

Reliable domain authentication and DNS configuration are essential for CyberArk component communication.

### 5. Service validation is critical during deployment

Verifying Windows services directly with `sc query` helped confirm successful installations and identify issues quickly.

### 6. PAM environments involve multiple interconnected systems

CyberArk deployments require careful coordination between networking, authentication, server configuration, and security hardening.

---

## Tools Used

| Tool                   | Purpose                               |
| ---------------------- | ------------------------------------- |
| VMware Workstation     | Virtual machine management            |
| Windows Server         | Active Directory and CyberArk hosting |
| CyberArk Digital Vault | Secure credential storage             |
| CPM                    | Password lifecycle management         |
| PSM                    | Privileged session isolation          |
| PVWA                   | Web administration portal             |
| PrivateArk             | Vault administration                  |
| PowerShell             | Administration and configuration      |
| Command Prompt         | Service validation                    |
| Active Directory       | Domain authentication and DNS         |

---

## What I'd Improve in Production

### High Availability Architecture

A production deployment would include:

* Redundant Vault servers

* Load-balanced PVWA servers

* Failover CPM infrastructure

* Backup PSM systems

### SSL/TLS Hardening

I would implement:

* Trusted enterprise certificates

* Full HTTPS enforcement

* Secure encrypted communication

* Certificate lifecycle management

### SIEM Integration

Production environments should forward:

* PAM audit logs

* Session recordings

* Administrative actions

* Vault events

into a SIEM platform such as Splunk or Microsoft Sentinel.

### MFA Integration

Administrative access should require:

* Multi-factor authentication

* Conditional access policies

* Privileged access approval workflows

### Automated Monitoring

Additional monitoring could include:

* Service health alerts

* Session monitoring

* Vault performance metrics

* Credential rotation failures

---

*CyberArk Privileged Access Management Lab — Built as a hands-on enterprise PAM and privileged infrastructure project focused on secure credential management, vault architecture, and CyberArk administration.*

**David Saulsberry**

[LinkedIn](https://www.linkedin.com/in/david-saulsberry/?utm_source=chatgpt.com) · [GitHub](https://github.com/DavidLSaulsberry?utm_source=chatgpt.com)
