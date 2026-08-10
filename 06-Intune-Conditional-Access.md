Lab Write-Up: Enforcing Modern Endpoint Security via Microsoft Intune & Entra ID
Objective
To engineer, deploy, and validate a closed-loop modern endpoint management architecture using Microsoft Intune and Microsoft Entra ID. This lab demonstrates how to establish device security baselines and enforce identity-based access controls to isolate noncompliant endpoints from corporate resources.

Architectural Framework: Lawgiver vs. Enforcer
In modern enterprise cloud environments, endpoint security relies on a two-part governance model:

Microsoft Intune (The "Legislative Branch"): Serves as the rule maker. Through Device Compliance Policies, Intune defines baseline technical requirements (e.g., encryption, OS versions) and evaluates whether an endpoint complies. Intune alone does not control access tokens—it flags compliance state.

Microsoft Entra ID Conditional Access (The "Executive Branch"): Serves as the gatekeeper. Through Conditional Access Policies, Entra ID evaluates identity authentication requests. By referencing Intune's compliance flag, Entra ID enforces access rules and actively blocks noncompliant endpoints from reaching organization data.

Detailed Execution Steps
Phase 1: Defining the Security Baseline in Microsoft Intune
Access Intune Admin Center:

Navigated to the Microsoft Intune admin center at intune.microsoft.com.

Initiate Compliance Policy Creation:

Navigated to Devices > Compliance policies.

Selected + Create Policy.

Configure Platform and Basics:

Platform: Selected Windows 10 and later.

Name: Set policy name to Baseline Security - Windows Workstations.

Configure Compliance Settings:

Device Health: Expanded settings and set Require BitLocker to Require.

System Security: Set Require a password to unlock mobile devices to Require.

Set Noncompliance Actions:

Actions for Noncompliance: Retained default setting to mark the device as Noncompliant Immediately.

Review and Deploy:

Reviewed all settings and clicked Create to deploy the baseline compliance policy across the tenant.

Phase 2: Configuring Access Control in Microsoft Entra ID
Access Microsoft Entra Admin Center:

Opened a new tab and navigated to the Microsoft Entra admin center at entra.microsoft.com.

Locate Conditional Access:

Navigated through the left menu to Protection > Conditional Access (or utilized the top search bar for Conditional Access).

Create New Policy:

Clicked + New policy (or + Create new policy).

Name: Entitled the policy Require Compliant Device for M365 Access.

Define Policy Assignments:

Users: Selected Assignments > Users and targeted All users.

Target Resources: Selected Cloud apps or actions and targeted All cloud apps.

Configure Access Controls (Grant):

Under Access controls, selected Grant.

Checked the box for Require device to be marked as compliant.

Clicked Select to bind the control.

Enable and Enforce:

Toggled Enable policy from Report-only to On.

Clicked Create.

Phase 3: Validation, Warnings, and Boundary Analysis
UI Warning Review:

Self-Lockout Alert: Entra ID issued a warning regarding tenant lockout risks caused by applying global enforcement across All users without an excluded break-glass administrative account.

Report-Only Recommendation: Acknowledged Microsoft's recommendation to run new policies in Report-only mode prior to full enforcement in production environments.

Enforcement Verification:

Verified that the closed-loop security posture is active:

If an endpoint attempts to access cloud apps (e.g., SharePoint, Exchange Online), Entra ID queries Intune for compliance telemetry.

If BitLocker or security requirements are missing, Intune flags the device as Noncompliant.

Entra ID evaluates the Require device to be marked as compliant rule and revokes access at the identity layer.

Summary of Key Configuration Parameters
Service	Component	Setting / Parameter	Target Configuration
Microsoft Intune	Compliance Policy	Platform	Windows 10 and later
Microsoft Intune	Device Health	BitLocker	Required
Microsoft Intune	System Security	Device Unlock Password	Required
Microsoft Entra ID	Conditional Access	User Scope	All users
Microsoft Entra ID	Conditional Access	Target Apps	All cloud apps
Microsoft Entra ID	Conditional Access	Grant Requirement	Require device to be marked as compliant
Microsoft Entra ID	Conditional Access	State	On (Active Enforcement)
