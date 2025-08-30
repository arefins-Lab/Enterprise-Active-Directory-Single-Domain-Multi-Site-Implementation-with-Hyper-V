# Enterprise-Active-Directory-Single-Domain-Multi-Site-Implementation-with-Hyper-V
Project Overview
This project details the implementation of a robust, on-premise Active Directory infrastructure. The design focuses on providing a secure and reliable network with fast authentication services for a medium-sized enterprise with three regional offices: Dhaka, Chittagong, and Sylhet.

The architecture uses a single-domain, multi-site structure to ensure high availability and efficient replication across geographically separated locations.

Key Features
Multi-Site Active Directory: A three-site topology was designed to represent the Dhaka, Chittagong, and Sylhet offices. The project includes the proper configuration of site links to control replication between the three offices, ensuring efficient use of network bandwidth.

Single-Domain Structure: A single domain, systest.lab, was established with all sites belonging to the same domain. This design streamlines administration and establishes a unified security boundary for the entire organisation.

High Availability and Redundancy:

The main Dhaka office hosts the primary Domain Controller (DC), SRV-DHK-DC01, and an Additional Domain Controller (ADC), SRV-DHK-ADC01, for redundancy.

The Chittagong branch office features its own ADC, SRV-CTG-ADC02, to ensure high availability and efficient authentication within the regional site.

Secure Remote Access: The Sylhet office is configured with a Read-Only Domain Controller (RODC), SRV-SYL-RODC01, which provides secure local authentication while minimising the security risk associated with a full domain controller in a less secure location.

DNS & Replication: The design includes a fully configured DNS infrastructure with a backup DNS server, SRV-DHK-DNS02, to support domain resolution and reliable AD replication across all sites.

Technologies Used
Active Directory Domain Services (AD DS)

Windows Server 2022

Hyper-V

Virtual Router with Remote Access (LAN Routing)

Personal Reflections
One of the main challenges I faced during this project was figuring out how to connect the three isolated network segments. Initially, I considered using pfSense as a virtual router. However, since my home lab has only a single ISP connection, I decided it was crucial to keep the lab environment completely isolated from my home network by using internal virtual switches. This led me to switch my plan and use a Windows Server 2019 VM with the Remote Access role to provide LAN routing, which proved to be a more straightforward solution for my specific lab setup.

Another key difficulty I repeatedly faced was troubleshooting NTP synchronization and replication problems, which are critical for a functional multi-site Active Directory environment. The frustration from these issues was a key motivator in ensuring a robust and stable final configuration.

The most important lesson I learned is that when building a new lab for any design or project, it is essential to solve the NTP server synchronization problem immediately after the initial domain controller promotion, as this will prevent numerous frustrating replication issues later on.

Getting Started
This guide will walk you through the process of setting up the virtual lab environment in Hyper-V, replicating the Active Directory site design from this project.

Create the Virtual Machines
Begin by creating five virtual machines (VMs) using Windows Server 2022 and give them the following names and roles:

SRV-DHK-DC01 (Dhaka Domain Controller)

SRV-DHK-ADC01 (Dhaka Additional Domain Controller)

SRV-CTG-ADC02 (Chittagong Additional Domain Controller)

SRV-SYL-RODC01 (Sylhet Read-Only Domain Controller)

SRV-DHK-DNS02 (Dhaka DNS Backup Server)

Configure Virtual Networking
Create three Internal Virtual Switches in Hyper-V to isolate the lab environment:

DHK-Network

CTG-Network

SYL-Network

Set IP Addressing
Configure a static IP address on each VM, assigning them to the following subnets:

Dhaka: 192.168.10.0/24 (Connect this subnet to the DHK-Network switch)

Chittagong: 192.168.20.0/24 (Connect this subnet to the CTG-Network switch)

Sylhet: 192.168.30.0/24 (Connect this subnet to the SYL-Network switch)

Install a Virtual Router
Use a separate VM running Windows Server 2019 and install the Remote Access role with LAN Routing enabled. This virtual router will provide connectivity between the three isolated subnets.

Deploy Active Directory
Begin by promoting the SRV-DHK-DC01 VM to a forest root domain controller to create the parent domain, systest.lab. Continue deploying the other domain controllers in their respective sites.
