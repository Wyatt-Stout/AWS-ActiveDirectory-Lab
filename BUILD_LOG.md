# Server Provisioning & Build Log

## 1. Cryptographic Key Management
* **Key Pair Name:** ActiveDirectory-Lab-Key
* **Encryption Type:** RSA (.pem format)
* **Security Control:** Generated a local private key pair to securely decrypt the Windows Server built-in Administrator password. The private key is stored locally and excluded from public source control repository uploads for security compliance.

## 2. Primary Domain Controller Provisioning (DC-01)
* **Server Hostname:** DC-01
* **Operating System:** Microsoft Windows Server 2022 Base
* **Compute Tier:** t3.medium (2 vCPUs, 4 GiB RAM)
* **Network Placement:** Target VPC: `DomainLab-VPC` | Target Subnet: `Private-Subnet-2a` (us-east-2a)
* **Instance ID:** i-0457de0e6be1b5564
