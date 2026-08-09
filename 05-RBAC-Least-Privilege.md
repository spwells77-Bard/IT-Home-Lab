# Lab Documentation: Role-Based Access Control and The Principle of Least Privilege

## The Objective
To establish a secure daily operations account by configuring Role-Based Access Control (RBAC) within Microsoft Entra ID. Operating daily as a Global Administrator violates security frameworks, requiring the strict implementation of the Principle of Least Privilege.

## Execution and Role Assignment
I provisioned a dedicated `helpdesk` user account within the Microsoft 365 Admin Center. Rather than assigning unrestricted global access, I assigned the built-in **Helpdesk Administrator** role. This specific configuration grants the precise permissions necessary to manage standard user lifecycles, force password resets, and revoke sign-in sessions for Tier 1 support requests.

## Boundary Testing and Validation
To validate the security configuration, I executed a boundary failure test. After authenticating into an isolated session as the restricted `helpdesk` user and clearing the Multi-Factor Authentication (MFA) gate, I attempted to force a password reset on the primary Global Administrator account. 

As expected, the system actively blocked the action, throwing a localized reset failure error. This confirmed the RBAC policies were successfully restricting my L1/L2 account from altering tenant infrastructure or elevating privileges.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1f16d74-0c26-4563-a39c-fac29a0959c3" />
