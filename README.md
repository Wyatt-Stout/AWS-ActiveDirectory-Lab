# AWS-ActiveDirectory-Lab
Self Managed Active Directory AWS Lab

## Project Intent
This lab was designed to develop a practical understanding of enterprise IT infrastructure and network administration. Instead of utilizing a managed directory service, I manually architected and configured a **Self-Managed Active Directory** infrastructure from the ground up. 

The deployment uses Windows Server 2022 EC2 instances promoted to Domain Controllers. These are distributed across separate physical Availability Zones within the AWS US-East-2 (Ohio) region to simulate a resilient, fault-tolerant enterprise environment capable of surviving a localized data center outage.

## Core Technologies & Tools
* **Cloud Platform:** Amazon Web Services (VPC, EC2)
* **Operating Systems:** Windows Server 2022, Windows 11 (Client Workstations)
* **Directory Services & Networking:** Active Directory Domain Services (AD DS), DNS Server Management, AWS Security Groups, VPC DHCP Option Sets

## Repository Structure
* **NETWORK_LAYOUT.md:** Technical documentation covering subnet allocation, IP addressing schemas, and security group firewall rules. *(In progress)*
* **BUILD_LOG.md:** Step-by-step technical log tracking server provisioning, OS configuration, and forest promotion. *(Next step)*
* **HELPDESK_TICKETS.md:** Documented troubleshooting logs detailing network connectivity errors, DNS resolution failures, and domain trust issues resolved during deployment. *(Final step)*
