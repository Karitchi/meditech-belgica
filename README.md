# meditech-belgica

## Introduction

This project, developed as part of the **Sécurité des Réseaux** course, focuses on designing and implementing a secure and efficient network infrastructure for **MediTech Belgica**, a healthcare organization. MediTech Belgica offers various services such as personalized medical consultations, secure sharing of medical records, and WiFi access for patients. The goal is to build a network that ensures performance, scalability, and, most importantly, security.

By utilizing concepts covered in class and using network simulators like GNS3, this project demonstrates the integration of security mechanisms to safeguard sensitive data, ensure secure communications, and support the organization’s operations. The infrastructure includes both internal services (Active Directory, medical record management) and external services (a public website).

---

## Table of Contents

1. [Introduction](#introduction)
2. [Device Plan](#device-plan)
   - Network Devices
   - Services Devices
   - End Devices
3. [Network Plan](#network-plan)
   - VLANs and Subnets
4. [Configuration and Security Measures](#configuration-and-security-measures)
5. [Implementation](#implementation)
6. [Conclusion](#conclusion)

---

## Device Plan

### Network Devices

| **Role**                  | **Device/Server**                                | **Purpose**                                                                                                          |
| ------------------------- | ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **Core Router (pfSense)** | pfSense or similar (handles routing/firewall)    | Manages routing, security, and traffic flow between internal, external, and DMZ networks.                            |
| **Layer 3 Switches**      | Managed Layer 3 Switches (Routing between VLANs) | Performs inter-VLAN routing and network segmentation, ensuring communication between departments or network zones.   |
| **Layer 2 Switches**      | Managed Layer 2 Switches                         | Connects end devices (computers, printers, etc.) within each department and supports VLANs for network segmentation. |
| **WiFi Access Points**    | Wireless Access Points                           | Provides wireless network access for internal employees and guest users (patients), with segmentation as needed.     |

### Services Devices

| **Role**                        | **Device/Server**                             | **Purpose**                                                                                                    |
| ------------------------------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Active Directory**            | Windows Server (with AD DS)                   | Centralized authentication, user management, group policies, and DNS/DHCP services for internal users.         |
| **Intranet Server**             | Linux Server                                  | Hosts internal services (e.g., file sharing, internal web apps, collaboration tools) for staff use.            |
| **Medical Records Server**      | Windows/Linux Server (PACS system)            | Secure storage and sharing of medical records between doctors while ensuring patient confidentiality.          |
| **Extranet Server (Web)**       | Web Server (Linux/Apache or Windows IIS)      | Public-facing website for appointment booking, patient interaction, and access to MediTech Belgica’s services. |
| **Backup Server**               | Dedicated Backup Server                       | Manages regular backup of critical data (medical records, user data, server configurations).                   |
| **Cloud Services (Office 365)** | Microsoft Cloud (External email/Office Suite) | Manages external email, calendar, and document collaboration tools for the organization.                       |

### End Devices

| **Role**                         | **Device**                             | **Purpose**                                                                                                            |
| -------------------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Doctors' Workstations**        | Desktop PCs or Laptops (Windows/Linux) | Access medical records, internal services, email, and manage patient consultations.                                    |
| **Secretaries' Workstations**    | Desktop PCs or Laptops (Windows/Linux) | Manage patient appointments, scheduling, billing, and perform administrative tasks.                                    |
| **Medical Devices**              | Network-connected Medical Equipment    | Devices used for patient diagnosis and treatment that require network access for data sharing (e.g., imaging systems). |
| **WiFi-enabled Patient Devices** | Smartphones/Tablets (iOS/Android)      | Patients can access the secure guest WiFi to browse the internet and access MediTech Belgica’s services.               |
| **Administrator Workstations**   | Desktop PCs or Laptops (Windows/Linux) | IT staff or system administrators use these for managing servers, network devices, and infrastructure.                 |
| **Bastion Host Access (Admin)**  | Admin PC or Terminal (Windows/Linux)   | Provides secure access for IT administrators to manage infrastructure and perform troubleshooting tasks.               |

---

## Network Plan

### VLANs and Subnets

| **VLAN**    | **Name**       | **Devices/Servers**                                                             | **Purpose**                                                                                                                       | **Subnet** | **CIDR/Mask**   |
| ----------- | -------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- |
| **VLAN 10** | **doctor**     | Doctors' Workstations, Medical Devices, Intranet Server, Medical Records Server | Internal devices related to doctors' consultations, medical equipment, and medical records.                                       | 10.10.0.0  | 255.255.255.0   |
| **VLAN 20** | **secretary**  | Secretaries' Workstations, Active Directory, Backup Server, Layer 2 Switches    | Internal administrative devices for managing patient scheduling, billing, authentication, and backups.                            | 10.20.0.0  | 255.255.255.0   |
| **VLAN 30** | **guest**      | WiFi-enabled Patient Devices                                                    | Guest WiFi for patients with secure internet access and possibly limited access to specific services.                             | 10.30.0.0  | 255.255.255.0   |
| **VLAN 40** | **server**     | Intranet Server, Medical Records Server, Backup Server                          | Servers dedicated to internal operations, file sharing, records, backups, and other important services.                           | 10.40.0.0  | 255.255.255.0   |
| **VLAN 50** | **dmz**        | Extranet Server (Web)                                                           | Devices for external-facing website.                                                                                              | 10.50.0.0  | 255.255.255.0   |
| **VLAN 60** | **core**       | Layer 3 Switch 1, Layer 3 Switch 2, pfSense LAN Interface                       | Core network devices for routing between VLANs. Dedicated subnet for interconnection between Layer 3 switches and pfSense router. | 10.60.0.0  | 255.255.255.252 |
| **VLAN 99** | **management** | Core Router (pfSense), Layer 3 Switches, Layer 2 Switches, Bastion Host (Admin) | Routing, security devices, and management of communication between different VLANs and external networks.                         | 10.99.0.0  | 255.255.255.0   |

---

## Configuration and Security Measures

This section will detail the specific configurations implemented to ensure the network is secure, including:

- **VLAN Configuration**: Detailed setup for each VLAN and their respective purposes.
- **Routing and Firewall Rules**: Configuration for pfSense and Layer 3 switches to ensure inter-VLAN routing and security.
- **Access Control**: How devices are segmented and secured to ensure proper access control.
- **Encryption**: Measures taken to protect sensitive data (e.g., medical records) in transit and at rest.

---

## Implementation

This section will explain how the infrastructure is implemented, detailing the following:

- **Software Used**: Description of the tools and simulators used to create the network.
- **Virtualization**: The setup of virtual machines and the environment used for testing and implementation (e.g., GNS3 or Proxmox).
- **Challenges and Solutions**: A brief overview of any challenges faced during the implementation and how they were resolved.

---

## Conclusion

In this section, the project’s outcomes will be summarized, including:

- The security features implemented to ensure the integrity of the MediTech Belgica network.
- The roles of each group member in completing the project.
- Lessons learned and potential improvements for future iterations of the project.
