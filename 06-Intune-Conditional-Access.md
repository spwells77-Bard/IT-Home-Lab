# Lab Write-Up: Enforcing Modern Endpoint Security via Microsoft Intune & Entra ID

## **Objective**

To engineer, deploy, and validate a closed-loop modern endpoint management architecture using Microsoft Intune and Microsoft Entra ID. This lab demonstrates how to establish device security baselines and enforce identity-based access controls to isolate noncompliant endpoints from corporate resources.

---

## **Architectural Framework: Lawgiver vs. Enforcer**

In modern enterprise cloud environments, endpoint security relies on a two-part governance model:

1. **Microsoft Intune (The "Legislative Branch"):** Serves as the rule maker. Through **Device Compliance Policies**, Intune defines baseline technical requirements (e.g., encryption, OS versions) and evaluates whether an endpoint complies. Intune alone does not control access tokens—it flags compliance state.
2. **Microsoft Entra ID Conditional Access (The "Executive Branch"):** Serves as the gatekeeper. Through **Conditional Access Policies**, Entra ID evaluates identity authentication requests. By referencing Intune's compliance flag, Entra ID enforces access rules and actively blocks noncompliant endpoints from reaching organization data.

---

## **Detailed Execution Steps**

### **Phase 1: Defining the Security Baseline in Microsoft Intune**

1. **Access Intune Admin Center:**
* Navigated to the Microsoft Intune admin center at `intune.microsoft.com`.


2. **Initiate Compliance Policy Creation:**
* Navigated to **Devices** > **Compliance policies**.
* Selected **+ Create Policy**.


3. **Configure Platform and Basics:**
* **Platform:** Selected `Windows 10 and later`.
* **Name:** Set policy name to `Baseline Security - Windows Workstations`.


4. **Configure Compliance Settings:**
* **Device Health:** Expanded settings and set **Require BitLocker** to `Require`.
* **System Security:** Set **Require a password to unlock mobile devices** to `Require`.


5. **Set Noncompliance Actions:**
* **Actions for Noncompliance:** Retained default setting to mark the device as `Noncompliant Immediately`.


6. **Review and Deploy:**
* Reviewed all settings and clicked **Create** to deploy the baseline compliance policy across the tenant.



---

### **Phase 2: Configuring Access Control in Microsoft Entra ID**

1. **Access Microsoft Entra Admin Center:**
* Opened a new tab and navigated to the Microsoft Entra admin center at `entra.microsoft.com`.


2. **Locate Conditional Access:**
* Navigated through the left menu to **Protection** > **Conditional Access** (or utilized the top search bar for `Conditional Access`).


3. **Create New Policy:**
* Clicked **+ New policy** (or **+ Create new policy**).
* **Name:** Entitled the policy `Require Compliant Device for M365 Access`.


4. **Define Policy Assignments:**
* **Users:** Selected **Assignments** > **Users** and targeted `All users`.
* **Target Resources:** Selected **Cloud apps or actions** and targeted `All cloud apps`.


5. **Configure Access Controls (Grant):**
* Under **Access controls**, selected **Grant**.
* Checked the box for **Require device to be marked as compliant**.
* Clicked **Select** to bind the control.


6. **Enable and Enforce:**
* Toggled **Enable policy** from *Report-only* to **On**.
* Clicked **Create**.



---

### **Phase 3: Validation, Warnings, and Boundary Analysis**

1. **UI Warning Review:**
* **Self-Lockout Alert:** Entra ID issued a warning regarding tenant lockout risks caused by applying global enforcement across *All users* without an excluded break-glass administrative account.
* **Report-Only Recommendation:** Acknowledged Microsoft's recommendation to run new policies in *Report-only* mode prior to full enforcement in production environments.


2. **Enforcement Verification:**
* Verified that the closed-loop security posture is active:
* If an endpoint attempts to access cloud apps (e.g., SharePoint, Exchange Online), Entra ID queries Intune for compliance telemetry.
* If BitLocker or security requirements are missing, Intune flags the device as `Noncompliant`.
* Entra ID evaluates the `Require device to be marked as compliant` rule and revokes access at the identity layer.






---

## **Summary of Key Configuration Parameters**

| Service | Component | Setting / Parameter | Target Configuration |
| --- | --- | --- | --- |
| **Microsoft Intune** | Compliance Policy | Platform | Windows 10 and later |
| **Microsoft Intune** | Device Health | BitLocker | Required |
| **Microsoft Intune** | System Security | Device Unlock Password | Required |
| **Microsoft Entra ID** | Conditional Access | User Scope | All users |
| **Microsoft Entra ID** | Conditional Access | Target Apps | All cloud apps |
| **Microsoft Entra ID** | Conditional Access | Grant Requirement | Require device to be marked as compliant |
| **Microsoft Entra ID** | Conditional Access | State | On (Active Enforcement) |
<img width="1920" height="1080" alt="Screenshot 2026-08-09 123525" src="https://github.com/user-attachments/assets/ad28e25f-8624-4a38-b7d0-0f30f439bb4e" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 123601" src="https://github.com/user-attachments/assets/45a6ea13-f448-4173-a8d6-fd307bab4fab" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 123847" src="https://github.com/user-attachments/assets/a8d9146d-1306-48b0-ba32-ae4f2d2bf32b" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 124114" src="https://github.com/user-attachments/assets/b3f5cad7-8689-42cb-9173-009547a051da" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 125416" src="https://github.com/user-attachments/assets/da276125-50bb-4eb4-9524-715700d1c845" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 130738" src="https://github.com/user-attachments/assets/4f1ba8ae-d27d-43d2-99e1-fa6cf103e2f0" />
<img width="1920" height="1080" alt="Screenshot 2026-08-09 130954" src="https://github.com/user-attachments/assets/08d3e5ac-50a1-4607-b938-f5035965e42d" />
