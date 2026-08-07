# Lab Documentation: Bypassing Layer 1 Hardware Limits via Cloud Identity Pivot

## The Roadblock & Triage
During a local Windows Server Domain Controller deployment, I encountered a hypervisor resource allocation failure ("Not enough memory to initialize"). My host machine had roughly 1.1GB of available RAM, but the enterprise home lab parameters require a minimum of 4GB of RAM to initialize a Windows Server VM properly. 

To triage the issue, I checked Task Manager for background processes consuming Standby Memory and attempted to adjust the hypervisor's Dynamic RAM settings to the absolute minimum floor (1024 MB). Despite these software-level adjustments, the host machine mathematically lacked the physical memory to carve out a contiguous block for the boot sequence. I formally identified this as a hard Layer 1 (Physical) hardware limitation.

## The Strategic Pivot
Rather than halting the lab or risking host system instability by forcing a type-2 hypervisor deployment, I pivoted from a localized server build to a cloud-native architecture. I provisioned a base Microsoft Entra ID tenant and executed a Microsoft 365 Business Premium trial to secure a fully licensed sandbox environment. This completely bypassed the local hardware constraints and provided a dedicated environment to practice configuring cloud-native users, Multi-Factor Authentication (MFA), and Conditional Access policies.

## Identity Routing Fix
While attempting to activate the enterprise licensing in the Microsoft 365 Admin Center, I encountered a "Not Authorized" access error. I diagnosed this as an application-layer identity mismatch: my web browser was automatically passing my personal email token instead of the required enterprise token. 

To bypass this routing block, I located my native cloud User Principal Name (`admin@spsweetapplegmail.onmicrosoft.com`) in the Entra ID dashboard. I then utilized an InPrivate browser session to isolate the token, successfully authenticating as the native Global Administrator and successfully securing the licenses.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e120a02a-fb30-404f-b7b8-7e3f0dba65f7" />

