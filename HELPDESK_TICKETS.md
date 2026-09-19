# Windows OS Administration & Directory Services Log

## 1. Initial Host Access Protocol
* **Default Administrative Account:** Administrator
* **Authentication Method:** Decrypted via AWS Key Management using `activedirectory-lab-key.pem`.
* **Public IPv4 Address:** 3.16.139.18


# Helpdesk Ticket & Incident Resolution Queue

##  Ticket #1001: Remote Desktop (RDP) Connection Failure to Cloud Domain Controller
*   **User/Severity:** Administrator | High (Blocker)
*   **Symptom:** Attempting to establish a Remote Desktop connection to `DC-01` timed out with the error: *"Remote Desktop can't connect to the remote computer for one of these reasons..."*
*   **Root Cause Analysis:** Because the Virtual Private Cloud (VPC) was engineered entirely from scratch, it lacked a routing pathway to the public internet. The network was completely isolated without an edge gateway or explicit default route entries.
*   **Helpdesk Troubleshooting & Resolution Steps:**
    1.  **Internet Gateway (IGW) Provisioning:** Created an AWS Internet Gateway named `ActiveDirectory-Lab-IGW` and attached it directly to `ActiveDirectory-Lab-VPC`.
    2.  **Route Table Modification:** Audited the VPC's main route table. Added a default route entry targeting `0.0.0.0/0` (all internet destinations) and directed the traffic interface to the newly attached `ActiveDirectory-Lab-IGW`.
    3.  **Security Group Inspection:** Validated that the `Domain-Controller-SG` inbound firewall rules strictly permitted TCP Port 3389 (RDP) from my local public IP address.
*   **Status:** Resolved. Secure administrative access to the Windows Server desktop was successfully established.

