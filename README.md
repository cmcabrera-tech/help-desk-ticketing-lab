# Help Desk Ticketing Lab

![Platform](https://img.shields.io/badge/Platform-Hyper--V-0078D4)
![Server](https://img.shields.io/badge/Server-Ubuntu%2022.04-E95420)
![Ticketing](https://img.shields.io/badge/Ticketing-osTicket-orange)
![Web](https://img.shields.io/badge/Web-Apache-D22128)
![Database](https://img.shields.io/badge/Database-MariaDB-003545)
![PHP](https://img.shields.io/badge/PHP-8.2-777BB4)

## Project Overview

This project documents the design and implementation of a self-hosted Help Desk ticketing environment using osTicket on an Ubuntu Server virtual machine. The lab simulates a small enterprise support operation with departments, role-based access, service-level agreements, ticket routing, Tier 1 troubleshooting, Tier 2 escalation, user communication, and incident resolution.

The environment was created as a hands-on portfolio project for entry-level Help Desk, IT Support Specialist, and Desktop Support roles.

> This is an isolated home-lab environment. All users, agents, email addresses, incidents, and company information shown in this repository are fictional.

## Objectives

- Deploy a Linux-based ticketing server in Hyper-V.
- Install and secure the Apache, MariaDB, and PHP application stack.
- Deploy and configure osTicket.
- Implement departments, technical roles, agents, users, and SLA plans.
- Route requests through appropriate Help Topics.
- Document complete incident lifecycles.
- Demonstrate Tier 1 diagnosis and Tier 2 escalation.
- Connect ticket resolutions to Active Directory concepts.
- Practice clear technical documentation and user communication.

## Lab Architecture

```mermaid
flowchart TD
    USER["End Users"] --> PORTAL["osTicket Help Desk"]
    PORTAL --> T1["Tier 1 Support"]
    T1 -->|Escalation required| T2["Tier 2 Support"]
    T1 -.->|Account and permission checks| AD["Active Directory Lab"]
    T2 -.->|Advanced troubleshooting| AD
    PORTAL --> DB["MariaDB Database"]
    T1 --> RES["Resolution and User Communication"]
    T2 --> RES
```

| Component | Configuration |
|---|---|
| Hypervisor | Microsoft Hyper-V |
| Virtual machine | TICKET01 |
| Operating system | Ubuntu Server 22.04.5 LTS |
| Ticketing platform | osTicket v1.18.4 |
| Web server | Apache HTTP Server |
| Database | MariaDB |
| Application runtime | PHP 8.2 |
| Host system | Windows 11 Pro |

## Implementation

### 1. Server deployment

I created a Generation 2 Ubuntu Server virtual machine in Hyper-V with dynamic memory, two virtual processors, a virtual hard disk, Secure Boot configured for Linux, and network connectivity through a Hyper-V virtual switch.

After installing Ubuntu Server, I verified network connectivity and DNS resolution, updated the operating system, and enabled OpenSSH for remote administration.

### 2. Web application stack

I installed and configured:

- Apache
- MariaDB
- PHP 8.2
- PHP extensions required by osTicket
- GDlib and APCu

I verified the Apache and MariaDB services, tested the web server, created a dedicated database and database user, and confirmed that the required PHP extensions were enabled.

### 3. osTicket deployment and security

I downloaded osTicket from its official release repository, verified the package checksum, deployed the application files, and configured an Apache virtual host.

After completing the web installer, I:

- Removed the installation directory.
- Restricted access to the osTicket configuration file.
- Used a dedicated database account instead of the MariaDB root account.
- Avoided publishing passwords or other credentials.
- Verified access to the Staff Control Panel.

## Help Desk Configuration

### Departments

- Support
- IT Support
- Network Support
- Account Administration
- Maintenance
- Sales

![Configured Help Desk departments](screenshots/11-helpdesk-departments-created.png)

### Support roles

| Role | Responsibility |
|---|---|
| Help Desk Technician - Tier 1 | Initial triage, common troubleshooting, documentation, user communication, and escalation |
| Senior Support Specialist - Tier 2 | Advanced troubleshooting, escalated incidents, and technical resolution |
| Administrator | System configuration, global access, and account administration |

### Agents

The lab includes an administrator, a Tier 1 technician, and a Tier 2 specialist. Department access and permissions were assigned according to each agent's responsibilities.

### SLA plans

| SLA | Grace period | Intended use |
|---|---:|---|
| Critical | 4 hours | Business-impacting incidents |
| Standard | 24 hours | Normal support requests |
| Low Priority | 72 hours | Low-impact requests and planned assistance |

### Help Topics

- Password Reset / Account Access
- Network Connectivity Issue
- Shared Folder / File Access

Each Help Topic was associated with the appropriate department, priority, and SLA plan to support consistent ticket routing.

## Incident Simulations

### Incident 1: Expired password

**Problem:** A user could not sign in after the account password expired.

**Tier 1 actions:**

- Verified the user's identity.
- Confirmed that the account was active.
- Identified an expired password.
- Issued a temporary password through an approved secure channel.
- Required a password change at the next sign-in.
- Confirmed successful access and closed the ticket.

### Incident 2: Network connectivity failure

**Problem:** A workstation displayed an unidentified network and could not access corporate resources.

**Tier 1 diagnosis:**

- Verified physical connectivity and airplane mode.
- Restarted the network adapter.
- Identified an APIPA address in the `169.254.0.0/16` range.
- Attempted to release and renew the DHCP lease.
- Escalated the incident after DHCP renewal failed.

**Tier 2 resolution:**

- Verified that the DHCP service was operational.
- Identified an incorrect switch-port VLAN assignment.
- Corrected the VLAN configuration.
- Renewed the workstation's DHCP lease.
- Verified gateway, DNS, and internet connectivity.

![Network ticket resolved after Tier 2 escalation](screenshots/21-network-ticket-resolved-by-tier2.png)

### Incident 3: Shared-folder access denied

**Problem:** An HR employee received an Access Denied message when opening the departmental shared folder.

**Resolution:**

- Confirmed network and server availability.
- Verified that the shared path was accessible.
- Reviewed the user's Active Directory group memberships.
- Identified missing membership in `GG_HR_Users`.
- Added the user to the authorized security group.
- Refreshed the user's security token.
- Confirmed successful access to the HR shared folder.

![Shared-folder access ticket resolved](screenshots/23-shared-folder-ticket-resolved.png)

## Ticket Workflow Demonstrated

```mermaid
flowchart LR
    A["Ticket Created"] --> B["Categorized"]
    B --> C["Assigned"]
    C --> D["Diagnosed"]
    D --> E{"Tier 1 resolves?"}
    E -->|Yes| F["User Notified"]
    E -->|No| G["Escalated to Tier 2"]
    G --> F
    F --> H["Ticket Closed"]
```

The documented workflow includes:

1. User identification and ticket creation.
2. Categorization through Help Topics.
3. Department, priority, and SLA assignment.
4. Initial user acknowledgment.
5. Internal troubleshooting notes.
6. Tier 1 to Tier 2 escalation when required.
7. Root-cause documentation.
8. User-facing resolution.
9. Ticket closure.

## Skills Demonstrated

- Help Desk ticket lifecycle management
- Incident triage and prioritization
- SLA configuration and monitoring
- Tier 1 and Tier 2 escalation
- Technical and customer-facing documentation
- Role-based access control
- Linux server administration
- Apache virtual-host configuration
- MariaDB database administration
- PHP module management
- DHCP, APIPA, DNS, VLAN, and gateway troubleshooting
- Active Directory security groups
- Shared-folder and NTFS permission concepts
- Hyper-V virtualization
- Security-conscious credential handling

## Evidence

Selected evidence from the deployment, configuration, and operation of the Help Desk environment. The complete set of screenshots is available in the [`screenshots`](screenshots/) directory.

### osTicket Administration

The osTicket Staff Control Panel was successfully deployed and configured for Help Desk administration.

![osTicket Admin Dashboard](screenshots/10-osticket-admin-dashboard.png)

### Help Desk Roles and Agents

Role-based access was configured for Tier 1, Tier 2, and administrative responsibilities.

![Help Desk Roles](screenshots/12-helpdesk-roles-created.png)

![Help Desk Agents](screenshots/13-helpdesk-agents-created.png)

### SLA and Ticket Routing

SLA plans and Help Topics were configured to control response expectations and route incoming requests appropriately.

![SLA Plans](screenshots/14-sla-plans-configured.png)

![Help Topics](screenshots/15-help-topics-configured.png)

### Password Reset Incident

A password-related support request was documented through the complete ticket lifecycle, from creation to resolution.

![Password Reset Ticket Opened](screenshots/17-password-reset-ticket-opened.png)

![Password Reset Ticket Resolved](screenshots/18-password-reset-ticket-resolved.png)

### Tier 1 Diagnosis and Tier 2 Escalation

A network connectivity incident was diagnosed at Tier 1 and escalated to Tier 2 when additional troubleshooting was required.

![Tier 1 Network Diagnosis](screenshots/19-network-ticket-tier1-diagnosis.png.png)

![Ticket Escalated to Tier 2](screenshots/20-network-ticket-escalated-to-tier2.png)

![Tier 2 Resolution](screenshots/21-network-ticket-resolved-by-tier2.png)

### Help Desk Dashboard

The final dashboard demonstrates the configured and operational ticketing environment.

![Help Desk Dashboard](screenshots/24-helpdesk-ticket-dashboard.png)

## Key Takeaways

This lab reinforced the importance of structured troubleshooting, accurate internal notes, clear communication with users, access based on job responsibilities, and timely escalation. It also demonstrated how a ticketing platform connects day-to-day support work with networking, server administration, security groups, and Active Directory.

## Author

**Carlos Cabrera**  
CompTIA A+ Certified | IT Support | Systems Administration
