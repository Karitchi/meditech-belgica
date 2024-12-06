# meditech-belgica

## Introduction

This project, developed as part of the **Sécurité des Réseaux** course, focuses on designing and implementing a secure and efficient network infrastructure for **MediTech Belgica**, a healthcare organization. MediTech Belgica offers various services such as personalized medical consultations, secure sharing of medical records, and WiFi access for patients. The goal is to build a network that ensures performance, scalability, and, most importantly, security.

By utilizing concepts covered in class and using network simulators like GNS3, this project demonstrates the integration of security mechanisms to safeguard sensitive data, ensure secure communications, and support the organization’s operations. The infrastructure includes both internal services (Active Directory, medical record management) and external services (a public website).

Shemas :

![image](https://github.com/user-attachments/assets/e321601d-10ce-4728-afc5-15caa954d46d)


---

## Table of Contents

1. [Introduction](#introduction)
2. [Specification] (#Spécification)
   2.1. [Project Objectives](###Project Objectives)
   2.2. [Functional Requirements](###Functional Requirements)
   2.3. [Technical constraints](###Technical constraints)
   2.4. [Deliverables](###Deliverables)
   2.5. [MVP (Minimum Viable Product)](###MVP (Minimum Viable Product))
2. [Architecture and Network Infrastructure](#Architecture and Network Infrastructure)
   - Network Devices
   - Services Devices
   - End Devices
3. [Network Plan](#network-plan)
   - VLANs and Subnets
4. [Configuration and Security Measures](#configuration-and-security-measures)
5. [Implementation](#implementation)
6. [Conclusion](#conclusion)

---

### Project Objectives

The primary objective is to implement a secure and efficient IT infrastructure that enables critical healthcare operations. This includes secure medical records management, online appointment scheduling, patient data protection, and secure WiFi connectivity for both staff and patients.

Beyond security, the infrastructure must be reliable and scalable to support daily medical operations. Using GNS3 simulation, we will demonstrate how proper network segmentation, access controls, and security protocols can protect sensitive medical information while maintaining system performance and accessibility.

### Functional Requirements

#### Network Infrastructure and Security

Active Directory setup for authentication, DNS, and DHCP services
Secure network segmentation (VLAN) for medical staff and patients
Firewall configuration and access control lists
VPN setup for remote access

#### Essential Services

Static website hosting
WiFi network with separate access for staff and patients
File sharing system for medical records
Office 365 email service integration

#### Technical constraints

Use of a simulator (GNS3 or equivalent)
Justified choice of operating systems
Complete documentation of configurations
Integration of security concepts learned in class

### Deliverables

* GNS3 network simulation
* Technical report including:
  * Configuration files and documentation
  * Logical topology with security mechanisms
  * Team member contributions
* Visual presentation support

### MVP (Minimum Viable Product)

The initial prototype will demonstrate:
* Basic network topology
* Core Active Directory configuration
* Minimal static website
* Essential security mechanisms (firewalls, VLANs)



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
| **Extranet Server (Web)**       | Web Server (Apache)      | Public-facing website for appointment booking, patient interaction, and access to MediTech Belgica’s services. |
| **Backup Server**               | Dedicated Backup Server                       | Manages regular backup of critical data (medical records, user data, server configurations).                   |
| **Cloud Services (Office 365)** | Microsoft Cloud (External email/Office Suite) | Manages external email, calendar, and document collaboration tools for the organization.                       |

### End Devices

| **Role**                         | **Device**                             | **Purpose**                                                                                                            |
| -------------------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Doctors' Workstations**        | Desktop PCs or Laptops (Windows) | Access medical records, internal services, email, and manage patient consultations.                                    |
| **Secretaries' Workstations**    | Desktop PCs or Laptops (Windows) | Manage patient appointments, scheduling, billing, and perform administrative tasks.                                    |
| **Medical Devices**              | Network-connected Medical Equipment    | Devices used for patient diagnosis and treatment that require network access for data sharing (e.g., imaging systems). |
| **WiFi-enabled Patient Devices** | Smartphones/Tablets (iOS/Android)      | Patients can access the secure guest WiFi to browse the internet and access MediTech Belgica’s services.               |
| **Administrator Workstations**   | Desktop PCs or Laptops (Windows) | IT staff or system administrators use these for managing servers, network devices, and infrastructure.                 |
| **Bastion Host Access (Admin)**  | Admin PC or Terminal (Windows)   | Provides secure access for IT administrators to manage infrastructure and perform troubleshooting tasks.               |

---

### Proposed Architecture and Network Infrastructure

| VLAN ID | Name | Devices/Servers | Purpose | Subnet | CIDR/Mask | Security Focus |
|---------|------|-----------------|----------|---------|------------|----------------|
| 10 | doctor | Doctors' Workstations, Medical Records Server | Secure medical data access | 10.10.0.0 | 255.255.255.0 | Data confidentiality, access control |
| 20 | secretary | Workstations, AD, Backup Server | Patient management, scheduling | 10.20.0.0 | 255.255.255.0 | Identity management, data backup |
| 30 | guest | WiFi Access Points | Patient internet access | 10.30.0.0 | 255.255.255.0 | Network isolation, bandwidth control |
| 40 | server | Intranet, Records, Backup Servers | Internal services | 10.40.0.0 | 255.255.255.0 | Service availability, data integrity |
| 50 | dmz | Web Server | Public website hosting | 10.50.0.0 | 255.255.255.0 | Perimeter security, access limitation |
| 60 | core | L3 Switches, pfSense LAN | Network infrastructure | 10.60.0.0 | 255.255.255.252 | Infrastructure security, routing |
| 99 | management | pfSense, Switches, Admin Host | Network administration | 10.99.0.0 | 255.255.255.0 | Privileged access, monitoring |

#### Design Principles
- Clear network segmentation
- Simple and maintainable structure
- Defense in depth strategy
- Principle of least privilege

#### Best Practices
- Systematic VLAN numbering
- Consistent subnet allocation
- Separated medical and public traffic
- Isolated management access
- DMZ for external services
- Security zone isolation
- Access control strategies
- Infrastructure monitoring

### Configuration and Security Measures

This section will detail the specific configurations implemented to ensure the network is secure.

#### Network Core Layer at MediTech Belgica

The core layer is where our medical center's network begins. Using a single pfSense firewall, we manage critical security and routing for our entire infrastructure. For our school project, we chose this simplified yet efficient design to thoroughly understand core networking concepts.

Our pfSense connects to three key elements. First, there's our bastion host - a dedicated management computer on the 10.99.0.0/24 network. This host is essential for securely configuring pfSense through its web interface:

```
# Management Access Configuration via Web Interface
Interface: Management (em3)
Network: 10.99.0.0/24

Rule Set:
- Allow HTTPS access from bastion (10.99.0.10)
- Log and deny all other management attempts
```

Second, we connect the appointment system in our DMZ (10.50.0.0/24). This public-facing service needs careful security rules:

```
# DMZ Configuration via Web Interface
Interface: DMZ (em2)
Network: 10.50.0.0/24

External Access Rules:
- Allow internet users to access appointment portal 
- Permit DMZ server to receive updates
- Block all DMZ access to internal networks
- Log all connection attempts
```

Third, we handle patient WiFi directly through pfSense (10.30.0.0/24). We keep it separate from our internal networks for enhanced security:

```
# Patient WiFi Configuration via Web Interface
Interface: WiFi (em4)
Network: 10.30.0.0/24

Control Rules:
- Enable internet access for patients
- Block access to all medical systems
- Set 20Mbps bandwidth limit
- Enable guest portal authentication
```

The internal medical records web interface, despite being web-based, lives inside our secure server VLAN (10.40.0.0/24) behind our distribution layer. Only authorized internal staff can access it through proper authentication.

This core configuration ensures our medical facility maintains proper network segmentation, with each service placed exactly where it needs to be for optimal security and performance. Every rule and configuration serves a specific purpose in protecting our medical operations while providing necessary services to patients.

From here, our core pfSense connects to our distribution layer switches, where we'll manage internal traffic between different medical departments.

#### Distribution Layer at MediTech Belgica

After our core pfSense firewall, the distribution layer acts like a smart traffic controller for our medical center. Here, we use two Cisco Layer 3 switches (let's call them DIST-SW1 and DIST-SW2) that connect to both our core pfSense and our access layer switches where all our medical staff's devices connect.

Think of these distribution switches like two highly coordinated traffic officers - if one gets sick, the other can handle all the traffic alone. This is crucial because doctors and nurses can't afford network downtime when accessing patient records. Here's how we make this happen:

Hot Standby Router Protocol (HSRP)

HSRP makes our two distribution switches work together. Imagine two receptionists at a desk - even if one takes a break, patients are still greeted seamlessly. Here's how we configure this on our switches:

```
! DIST-SW1 Configuration
interface Vlan10
 description "Medical Staff Network"
 ip address 10.10.0.2 255.255.255.0
 hsrp 10
  priority 110
  preempt
  authentication md5 key-chain hsrp-med
  ip 10.10.0.1

! DIST-SW2 Configuration (Backup)
interface Vlan10
 description "Medical Staff Network"
 ip address 10.10.0.3 255.255.255.0
 hsrp 10
  priority 90
  authentication md5 key-chain hsrp-med
  ip 10.10.0.1
```

EtherChannel Implementation

When our radiologists send large medical images across the network, we need lots of bandwidth. EtherChannel is like having multiple lanes on a highway instead of just one. We connect our distribution switches using multiple links that act as one big pipe:

```
! DIST-SW1 Configuration
interface range GigabitEthernet1/0/1-4
 description "EtherChannel to DIST-SW2"
 channel-protocol lacp
 channel-group 1 mode active
 no shutdown

interface Port-channel1
 description "Distribution Switch Interconnect"
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40
```

OSPF Routing

For network traffic to flow smoothly between different departments, we need smart routing. OSPF helps our switches learn the best paths automatically, like a GPS system constantly updating its routes:

```
! Both Distribution Switches
router ospf 1
 router-id 10.60.0.2
 network 10.60.0.0 0.0.0.255 area 0
 network 10.10.0.0 0.0.0.255 area 0
 passive-interface default
 no passive-interface GigabitEthernet1/0/5

interface GigabitEthernet1/0/5
 description "Link to Core pfSense"
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 secure_ospf_key
```

Inter-VLAN Routing

Different departments need different levels of security. Inter-VLAN routing lets us keep departments separate while still allowing necessary communication:

```
! DIST-SW1 Configuration
ip access-list extended MEDICAL_TO_ADMIN
 permit ip 10.10.0.0 0.0.0.255 10.20.0.0 0.0.0.255
 deny ip any any log

interface Vlan20
 description "Administrative Staff"
 ip address 10.20.0.2 255.255.255.0
 ip access-group MEDICAL_TO_ADMIN in
```

These distribution switches connect upward to our pfSense core and downward to our access layer switches. They ensure that:
- Doctors can always reach patient records with the help of HSRP
- Medical images transfer quickly through EtherChannel
- Network traffic finds the best path using OSPF
- Different departments stay securely separated yet connected through smart VLAN routing


#### Access Layer at MediTech Belgica

The access layer connects all our internal medical staff devices to the network. We use four Cisco Layer 2 switches (ACC-SW1 through ACC-SW4). Unlike patient WiFi which connects directly through pfSense for security isolation, these access switches serve our medical and administrative staff.

Internal Medical Records Web Access

Our medical records system needs to be both secure and easily accessible to authorized staff. Each access switch port connecting to a medical workstation is configured like this:

```
! ACC-SW1 Configuration
interface GigabitEthernet0/1
 description "Doctor Office - Medical Records Access"
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 spanning-tree portfast
 spanning-tree bpduguard enable
```

The internal medical records web server resides in VLAN 40 (Server VLAN), accessible only through secured staff VLANs. This ensures that doctors and authorized staff can access patient records while keeping them completely separate from the public-facing appointment system in the DMZ.

Department Segregation

Each department gets its own VLAN to maintain security:

```
! ACC-SW2 Configuration
vtp domain meditechbelgica
vtp mode client
vtp password secure_vtp_key

! Medical Staff Port
interface GigabitEthernet0/2
 description "Nurse Station"
 switchport mode access
 switchport access vlan 10
 switchport port-security
 storm-control broadcast level 20

! Administrative Staff Port
interface GigabitEthernet0/3
 description "Reception Desk"
 switchport mode access
 switchport access vlan 20
 switchport port-security
```

Uplink Redundancy

Our medical staff needs reliable access to records and systems, so we configure redundant uplinks to our distribution switches:

```
! ACC-SW3 Configuration
interface range GigabitEthernet0/23-24
 description "Uplink to Distribution Layer"
 switchport mode trunk
 switchport trunk allowed vlan 10,20,40
 switchport trunk native vlan 99
 spanning-tree guard root
```

These access switches focus purely on internal medical operations. By keeping patient WiFi separate on pfSense and the medical records system in a secure VLAN, we create clear security boundaries. Medical staff can efficiently access patient records while external patient access remains isolated through pfSense's DMZ and separate WiFi network.

This design ensures that sensitive internal medical systems stay completely separate from patient-accessible networks, much like how a hospital keeps treatment areas separate from public waiting rooms.



## Network Infrastructure Implementation at MediTech Belgica

For our school project, we designed and implemented a secure medical center network using three distinct layers. This approach helps us understand network architecture while ensuring both security and efficiency.

### Core Layer Implementation

At the heart of our network sits a pfSense firewall. We chose pfSense because it provides enterprise-level security features while remaining accessible for our learning environment. Through our bastion host, we can safely configure pfSense using its web interface. This setup is particularly important because misconfiguration of core security could expose sensitive medical data.

We connected three critical elements to our pfSense. First, the bastion host lives in the management VLAN (10.99.0.0/24). Think of this as our secure control room - only network administrators can access it, and only through strict security protocols. From this host, we configure all pfSense settings and monitor network health.

Second, we placed our appointment booking system in a DMZ (10.50.0.0/24). This separation is crucial - patients need to book appointments online, but this public-facing service must never provide a path into our internal medical systems. The DMZ acts like a secure reception area, separate from where we keep sensitive patient records.

Third, we handle patient WiFi directly through pfSense (10.30.0.0/24). By connecting guest WiFi at the core rather than through internal switches, we create a complete separation between patient internet access and our medical network. This design prevents any possibility of unauthorized access to medical systems through the guest network.


### Distribution Layer Implementation

In the middle of our network, we used two advanced Cisco switches. These switches needed to be powerful and redundant because they handle all traffic between our medical departments. When a doctor accesses patient records or a secretary schedules appointments, these switches ensure the data reaches its destination reliably and securely.

Our distribution switches connect upward to pfSense and downward to simpler department switches. This arrangement lets us enforce security policies effectively. For example, we can ensure that only authorized medical staff can access patient records, while administrative staff can access scheduling systems but not medical data.

### Access Layer Implementation

At the edge of our network, where doctors, nurses, and staff actually connect their computers, we have standard Cisco switches. Think of these switches as the doorways into our network - each port needs to be configured correctly to maintain our security. 

When a doctor plugs their computer into an office network port, they should only be able to access the resources they need. We achieve this by putting each department into its own virtual network (VLAN). Doctors' workstations go into VLAN 10, while secretaries' computers connect to VLAN 20. This separation isn't just for organization - it's a critical security measure that prevents problems in one department from affecting others.

One of our most important systems, the Windows Server 2022, lives in VLAN 40. This server handles user authentication and controls who can access what resources. By placing it in its own VLAN, we protect it from potential issues in user networks while ensuring it remains accessible to those who need it.

## Implementation Approach

For our project, we decided to use GNS3 with a server-client setup. This was new for us - in class, we usually work with pre-configured virtual machines. Setting up our own GNS3 server taught us a lot about network virtualization. It also let our team work together more efficiently, as we could all connect to the same network simulation.

The biggest challenge was learning how to add new devices to GNS3. Unlike our classroom exercises where everything is pre-installed, we had to figure out how to integrate pfSense, Windows Server, and our Cisco switches. Each device needed specific configuration to work in our virtual environment.

Working as a team made these challenges easier to overcome. Nicolas and Louis became our pfSense experts, spending time understanding its security features and how to configure them properly. Meanwhile, Alexis and Noël focused on making sure our Windows Server worked correctly and that all our devices could communicate properly.

When we hit roadblocks - like when we first tried to get the Windows Server talking to our switches - we would brainstorm together. Someone would suggest an idea, another would test it, and we'd all learn from the results. This approach helped us understand not just how to build a network, but how to troubleshoot problems effectively.

## Conclusion

We learned that building a secure medical network isn't just about following a recipe - it's about understanding why each piece matters and how they work together. Every decision, from where to place the guest WiFi to how to configure switch ports, needed to balance security with usability. A medical center needs to protect patient data while still making it accessible to authorized staff, and our network design reflects this balance.
