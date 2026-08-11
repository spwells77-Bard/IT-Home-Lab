# Lab Documentation: Windows Autopilot Hardware Hashing & Modern Device Provisioning

## 1. Objective
To configure, harvest, and troubleshoot modern enterprise endpoint provisioning using Windows Autopilot and Microsoft Intune. This lab documents the transition from legacy manual imaging to zero-touch, cloud-native hardware deployment while establishing robust troubleshooting workflows for common enrollment roadblocks.

---

## 2. Architectural Framework & The "Factory-Direct Smart Car" Analogy
Traditional IT imaging requires an administrator to manually unpack physical hardware, connect media, and step through interactive installation screens. **Windows Autopilot** modernizes this lifecycle by allowing corporate devices to ship straight from the vendor directly to the end-user.

* **The Analogy:** Traditional deployment is like a mechanic manually tuning a car in a parking lot before handing over the keys. Autopilot is like a car delivered straight from the factory floor in a sealed box; the user opens it, powers it on, and it automatically configures its features, security parameters, and corporate identity in the background.
* **The Enforcer:** During Out-of-Box Experience (OOBE), the endpoint communicates directly with Microsoft cloud services using its unique hardware fingerprint (hardware hash) to pull down assigned device profiles and compliance baselines silently.

---

## 3. Step-by-Step Execution Workflow

### Phase 1: Harvesting the Hardware Hash via PowerShell
To tie a physical device to your Intune cloud tenant, its unique hardware signature must be extracted:
1. Launched **Windows PowerShell** with elevated administrative privileges.
2. Executed script execution policy parameters and invoked the official Microsoft hardware harvesting script:
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   Install-Script -Name Get-WindowsAutoPilotInfo -Force
   Get-WindowsAutoPilotInfo -OutputFile C:\IntunePackaging\AutopilotHash.csv
   ```
3. Verified the generation of the standardized CSV cryptographic hash file inside the `C:\IntunePackaging` staging directory.

### Phase 2: Tenant Ingestion and Registration
1. Navigated to the **Microsoft Intune admin center** (`intune.microsoft.com`).
2. Selected **Devices** > **Windows enrollment** > **Devices** (under the Windows Autopilot Deployment Program section).
3. Clicked **Import**, selected the harvested `AutopilotHash.csv` file, and uploaded the hardware fingerprint into the cloud tenant.

### Phase 3: Engineering the Autopilot Deployment Profile
To dictate how devices behave upon unboxing, a custom profile was configured:
1. Navigated to **Devices** > **Windows enrollment** > **Deployment profiles** and created a new **Windows PC** profile.
2. Configured core OOBE parameters:
   * **Deployment mode:** `User-driven`
   * **Join type:** `Microsoft Entra joined`
   * **Privacy settings & EULA:** `Hide` (streamlining the user onboarding experience)
   * **User account type:** `Standard` (enforcing the Principle of Least Privilege)
3. Assigned the profile to an Entra ID group containing the target hardware hashes.

---

## 4. Roadblocks Encountered & Troubleshooting Methodologies
Real-world endpoint management involves diagnosing provisioning anomalies. Documenting these resolutions ensures operational resilience:

### Roadblock 1: Orphaned Device Records Post-Wipe
* **The Symptom:** A device wiped via self-deploying mode or reset fails to automatically re-register and provision during subsequent OOBE loops.
* **The Root Cause:** Stale, orphaned hardware records remain lodged in the Microsoft Intune admin center database, blocking duplicate registration tokens.
* **The Resolution:** Manually purged the stale device record from the Intune console before re-importing the hardware hash and triggering a fresh sync.

### Roadblock 2: The `ERROR_NOT_SUPPORTED` Reset Trap
* **The Symptom:** Initiating a remote or local Windows Autopilot Reset aborts prematurely, returning a specific `ERROR_NOT_SUPPORTED` error code.
* **The Root Cause:** The underlying Windows Recovery Environment (WinRE) partition was disabled or missing on the target endpoint, preventing the local reset engine from executing.
* **The Resolution:** Opened an administrative command prompt on the client machine and manually re-enabled WinRE utilizing the command `reagentc.exe /enable`.

### Roadblock 3: Hardware Hash Mismatch Failures
* **The Symptom:** Initial out-of-the-box enrollment requests fail authentication during the device preparation phase.
* **The Root Cause:** Firmware updates or motherboard component swaps altered the device's cryptographic signature, invalidating the previously imported CSV hash.
* **The Resolution:** Re-ran the `Get-WindowsAutoPilotInfo` PowerShell utility in an elevated session to harvest a fresh hardware fingerprint CSV file and updated the tenant record.

---

## 5. Conclusion
Mastering Windows Autopilot shifts organizational deployment from reactive, desk-side imaging to scalable, automated cloud provisioning. By understanding hardware hash harvesting, deployment profiles, and OOBE troubleshooting frameworks, enterprise IT environments can securely onboard endpoints with absolute reliability.
<img width="592" height="61" alt="Screenshot 2026-08-11 150059" src="https://github.com/user-attachments/assets/a13c5f8e-2535-491b-be6a-8f21279c8599" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 150324" src="https://github.com/user-attachments/assets/6a8cc4b8-332d-4f9d-b1c3-08fd5f9d9ab8" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 150425" src="https://github.com/user-attachments/assets/548cb41f-f71f-44b8-911a-35dcb64055a9" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 150434" src="https://github.com/user-attachments/assets/3091e9ea-23cc-4401-b814-0f83df92f622" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 150555" src="https://github.com/user-attachments/assets/80fb3205-a1a6-4eef-89c8-b21819b65c50" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 150638" src="https://github.com/user-attachments/assets/50002f56-7153-4217-bd6d-99c166953310" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 152034" src="https://github.com/user-attachments/assets/8cfa0732-00c1-4309-8d98-150e1d20071b" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 152402" src="https://github.com/user-attachments/assets/79cf39d0-ff65-42f7-b3da-ee314890b748" />
<img width="1920" height="1080" alt="Screenshot 2026-08-11 152402" src="https://github.com/user-attachments/assets/91972cc3-46cb-45ee-bf6e-f93159302f08" />
