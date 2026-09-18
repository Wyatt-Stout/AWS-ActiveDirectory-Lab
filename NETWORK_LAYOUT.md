# Network Infrastructure Log

## 1. Virtual Private Cloud (VPC)
* **VPC Name:** DomainLab
* **IPv4 CIDR Block:** 10.0.0.0/16 (Allows up to 65,536 internal IP addresses)
* **VPC ID:** vpc-039f3fbc45215e826

## 2. Subnet Configurations
To support high availability and isolate infrastructure, two subnets were created across separate physical data centers:

* **Subnet Name:** Private-Subnet-2b
  * **Zone:** us-east-2a
  * **CIDR:** 10.0.1.0/24
  * **Subnet ID:** subnet-0c6efeeba3e24c42a

* **Subnet Name:** Private-Subnet-2b
  * **Zone:** us-east-2b
  * **CIDR:** 10.0.2.0/24
  * **Subnet ID:** subnet-09030fc84c634d4a8

## 3. Firewall Primitives (Security Groups)
* **Security Group Name:** Domain-Controller-SG
* **Security Group ID:** sg-0bf0a06ebaa6d7af5
* **Security Strategy:** Applied least-privilege access rules. RDP inbound access (Port 3389) is strictly locked to my home public IP address to prevent brute-force network attacks.
