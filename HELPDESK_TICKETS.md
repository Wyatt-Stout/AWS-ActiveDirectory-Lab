# Helpdesk Ticket & Incident Resolution Queue

## Ticket #1001: Remote Desktop (RDP) Connection Failure to Cloud Domain Controller
*   **User/Severity:** Administrator | High (Blocker)
*   **Symptom:** Attempting to establish a Remote Desktop connection to `DC-01` timed out with the error: *"Remote Desktop can't connect to the remote computer for one of these reasons..."*
*   **Root Cause Analysis:** Because the Virtual Private Cloud (VPC) was engineered entirely from scratch, it lacked a routing pathway to the public internet. The network was completely isolated without an edge gateway or explicit default route entries.
*   **Helpdesk Troubleshooting & Resolution Steps:**
    1.  **Internet Gateway (IGW) Provisioning:** Created an AWS Internet Gateway named `ActiveDirectory-Lab-IGW` and attached it directly to `DomainLab-VPC`.
    2.  **Route Table Modification:** Audited the VPC's main route table. Added a default route entry targeting `0.0.0.0/0` (all internet destinations) and directed the traffic interface to the newly attached `ActiveDirectory-Lab-IGW`.
    3.  **Security Group Inspection:** Validated that the `Domain-Controller-SG` inbound firewall rules strictly permitted TCP Port 3389 (RDP) from my local public IP address.
*   **Status:** Resolved. Secure administrative access to the Windows Server desktop was successfully established.

## Ticket #1002: Unexpected Session Drop and RDP Failure Post-Promotion
*   **User/Severity:** Administrative System | Medium
*   **Symptom:** Immediately following the promotion phase, the active Remote Desktop connection froze and abruptly disconnected, rejecting initial reconnect attempts.
*   **Root Cause Analysis:** Expected operating system behavior. Upon completing a New Forest promotion, Windows Server executes an immediate forced reboot to transform the local Security Accounts Manager (SAM) database into the Active Directory database (NTDS.dit).
*   **Resolution Steps:**
    1.  Monitored the AWS EC2 Status Checks until the instance returned a healthy `2/2 checks passed` state.
    2.  Cleared cached local credentials from the Remote Desktop Client.
    3.  Modified login credentials to utilize the domain NetBIOS prefix (`ACTIVEDIRECTORYLAB\Administrator`) to authenticate against the newly initialized domain landscape rather than the local machine.
*   **Status:** Resolved.

## Ticket #1003: Administrative Console Discrepancy - Missing Infrastructure Assets
*   **User/Severity:** Operations Administrator | Medium
*   **Symptom:** Upon attempting to monitor the server reboot process via the AWS Management Console, the dashboard returned an empty list stating "No instances found," despite the asset being active moments prior.
*   **Root Cause Analysis:** Console session configuration shift. The AWS Management Console default scope shifted regions during an automated page refresh or new login sequence, defaulting away from the target project deployment zone.
*   **Helpdesk Troubleshooting & Resolution Steps:**
    1.  Audited the global AWS navigation header toolbar.
    2.  Identified a regional scope mismatch (the console had shifted out of the target development region).
    3.  Manually forced the regional context dropdown back to **US East (Ohio) / us-east-2**.
    4.  Cleared persistent UI filter states in the instance view matrix to restore structural visibility.
*   **Status:** Resolved. Monitoring capabilities for DC-01 fully restored.


## Ticket #1004: Persistent Administrative Console Asset Invisibility
*   **User/Severity:** Systems Administrator | High
*   **Symptom:** Following the domain controller forest promotion, the DC-01 instance became persistently invisible within the AWS EC2 Management Console, displaying "No instances found" even with the regional scope set correctly to us-east-2 (Ohio).
*   **Root Cause Analysis:** Console UI caching layer lock or active view filter restriction. The AWS web interface failed to render the active virtual machine due to a localized browser state conflict or automated session criteria filters hiding the instance while it cycled through its promotion boot sequence.
*   **Helpdesk Troubleshooting & Resolution Steps:**
    1.  Executed a hard browser cache refresh (Ctrl+F5) to clear the local web session storage.
    2.  Audited the primary console search bar matrix and cleared all persistent metadata filters hiding non-standard system states.
    3.  Validated global account IAM ID structures to guarantee context continuity.
*   **Status:** Resolved

## Ticket #1005: Target Domain Controller Object Missing from ADUC Directory
*   **User/Severity:** Systems Administrator | Low
*   **Symptom:** Upon opening Active Directory Users and Computers (ADUC) and expanding the `activedirectorylab.local` schema, the expected object name `DC-01` was completely missing from the container hierarchy.
*   **Root Cause Analysis:** Hostname naming discrepancy. The AWS console name tag (`DC-01`) is a cloud metadata abstraction. The Windows operating system retained its default AWS deployment hostname string (`EC2AMAZ-XXXXX`) during forest promotion, meaning the active Domain Controller object registered under its internal OS hostname rather than the AWS asset tag.
*   **Helpdesk Troubleshooting & Resolution Steps:**
    1.  Navigated to the explicit **Domain Controllers** Organizational Unit (OU) instead of the default Computers container.
    2.  Identified the active Global Catalog server object registered under the standard AWS random identifier prefix (`EC2AMAZ-`).
    3.  Verified directory functionality by confirming the object's status as a functional root domain authority.
*   **Status:** Resolved. Object localized and verified.

## Ticket #1006: Post-Deployment DCDIAG Flag - DFSREvent SYSVOL Warning
*   **User/Severity:** Monitoring System | Low (False Positive)
*   **Symptom:** Executing the diagnostic utility `dcdiag /q` returned the warning: *"There are warning or error events within the last 24 hours after the SYSVOL has been shared. Failing SYSVOL replication problems may cause Group Policy problems."*
*   **Root Cause Analysis:** Diagnostic tool sensitivity. `dcdiag` audits event logs on a rolling 24-hour window. Because the domain controller was freshly provisioned, the tool flagged benign, historic service startup logs and initialization events recorded while the NTDS database was initially mounting.
*   **Helpdesk Troubleshooting & Verification Steps:**
    1.  Bypassed the log history and queried active system shares directly via the command line utilizing the `net share` instruction.
    2.  Confirmed that both critical enterprise folder namespaces `SYSVOL` and `NETLOGON` are actively shared, reachable, and reporting a healthy initialization state.
*   **Status:** Resolved. Confirmed false positive; directory replication health is green.

## Ticket #1007: Administrative Strategy Shift - OS Desktop Licensing Constraints
*   **User/Severity:** Project Deployment | Low
*   **Symptom:** Attempting to provision a native Windows 11 client image (`Workstation-01`) within the standard AWS EC2 public catalog returned no matching templates, displaying only Server Base images.
*   **Root Cause Analysis:** Cloud provider licensing boundaries. Due to Microsoft Volume Licensing rules and multi-tenant cloud restrictions, consumer client operating systems (Windows 10/11) are restricted from standard shared public EC2 clusters. 
*   **Helpdesk Resolution Strategy:**
    1.  Substituted the consumer-tier image with an additional **Microsoft Windows Server 2022 Base** instance.
    2.  Pivoted deployment strategy to utilize the server operating system as a simulated client node framework.
    3.  Routed the new machine into `Private-Subnet-2b` (us-east-2b) to fully validate multi-availability zone domain interaction capability.
*   **Status:** Resolved. Next node provisioning underway.

## Ticket #1008: Multi-AZ Enterprise Workstation Domain Join Implementation
*   **User/Severity:** Client Node Integration | High
*   **Symptom:** Initial attempts to join `Workstation-01` (located in `Private-Subnet-2b`) to the `activedirectorylab.local` domain failed with a standard "Active Directory Domain Controller could not be contacted" network routing error.
*   **Root Cause Analysis:** DNS resolution failure. The newly provisioned node was utilizing default AWS DNS name server configurations (`169.254.169.254`), which lack namespacing schemas for the isolated, custom internal corporate directory environment.
*   **Helpdesk Resolution Steps:**
    1.  Established a Remote Desktop administrative session to the target workstation node via its public IPv4 translation interface.
    2.  Audited the local network adapter properties and modified the IPv4 configuration bindings.
    3.  Overrode the default DNS target by assigning the **Preferred DNS Server** value directly to the static private network IP address of the primary Domain Controller (`DC-01`).
    4.  Executed a connection handshake verification via the classic Windows System Control applet (`sysdm.cpl`).
    5.  Successfully authenticated against the root forest utilizing the domain principal security context, integrating the computer account into the database architecture.
*   **Status:** Resolved. Workstation successfully joined to the domain environment.

## Ticket #1009: Guest Client Failure to Locate Domain Controllers via DNS
*   **User/Severity:** Client Deployment Endpoint | High (Core Block)
*   **Symptom:** Workstation-01 returned a persistent "Ping request could not find host activedirectorylab.local" routing failure, preventing access to the system promotion handshake window.
*   **Root Cause Analysis:** Operating System Firewall Restrictions. While the AWS cloud security group topology was fully opened, the local Windows Defender Firewall profiles inside the guest OS of `DC-01` were actively discarding incoming ICMP echo requests and LDAP lookup probes.
*   **Helpdesk Resolution Steps:**
    1.  Authenticated into the primary Domain Controller (`DC-01`) via administrative remote desktop pathways.
    2.  Launched an elevated PowerShell terminal and disabled the local guest OS firewall framework profiles (`Set-NetFirewallProfile`).
    3.  Shifted back to `Workstation-01` and cleared the local system name resolution cache utilizing the `ipconfig /flushdns` parameter layout.
    4.  Re-executed a network ping validation test to confirm direct reachability.
*   **Status:** Resolved.
