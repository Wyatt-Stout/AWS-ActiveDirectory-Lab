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

## 3. Directory Services Role Installation
* **Feature Set:** Active Directory Domain Services (AD DS) & Remote Server Administration Tools (RSAT)
* **Execution Method:** Windows Server Manager Deployment Wizard
* **Status:** Installed binary files and administrative modules onto core operating system storage.

## 4. Active Directory Forest Promotion
* **Deployment Schema:** Add a New Forest Root
* **Root Domain Name (FQDN):** activedirectorylab.local
* **Functional Level Compatibility:** Windows Server 2016
* **Backdoor Recovery:** Set and archived a unique Directory Services Restore Mode (DSRM) credential block.

## 5. Domain Identity & Validation
* **Primary Domain Name (FQDN):** activedirectorylab.local
* **NetBIOS Domain Name:** ACTIVEDIRECTORYLAB
* **Verification Protocol:** Executed PowerShell ActiveDirectory Domain classes to confirm the schema namespace matches the intended design specifications.

## 6. Post-Promotion Infrastructure Verification
*   **Operating Status:** Successful Integration
*   **System Identity:** The server successfully transitioned from a standalone workgroup asset to the primary root Domain Controller for `activedirectorylab.local`.
*   **Access Validation:** Administrative access verified via RDP utilizing the explicit enterprise security principal: `ACTIVEDIRECTORYLAB\Administrator`.
*   **Active Directory Database Integrity:** Confirmed database generation (NTDS.dit) and system access pathways are online and responding locally.

## 7. Directory Administration & User Onboarding
* **Administration Tooling:** Active Directory Users and Computers (ADUC)
* **Provisioning Operation:** Manual creation of a standard domain security principal.
* **Onboarding Criteria:** Created account user object `testuser` with explicit operational lifecycle constraints ("User must change password at next logon") to adhere to enterprise security identity compliance standards.
