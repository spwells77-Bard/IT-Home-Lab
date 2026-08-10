# Lab Documentation: Enterprise Win32 Application Packaging & Troubleshooting Masterclass

## 1. Objective
To engineer, package, deploy, and troubleshoot a standard executable application utilizing the Microsoft Intune Management Extension (IME). This lab chronicles the complete end-to-end process—from raw installer acquisition to secure cloud deployment—while documenting real-world troubleshooting roadblocks and architectural resolutions.

## 2. Architectural Framework & The "Shipping Box" Concept
Microsoft Intune cannot evaluate or execute a loose, unmanaged `.exe` or `.msi` file directly in cloud deployments without proper wrapping. To manage complex software lifecycles, prerequisite checks, and silent installation parameters, Intune relies on the **Win32 Content Prep Tool** (`IntuneWinAppUtil.exe`). 

* **The Analogy:** Think of standard software as a fragile item. You cannot mail it bare; you must package it inside a secure, encrypted shipping box (the `.intunewin` file) so the courier (Intune) can safely deliver and unpack it on the destination endpoint.
* **The Enforcer:** Once deployed, the local **Microsoft Intune Management Extension (IME)** service executes the installation commands in the background without user interruption.

## 3. Step-by-Step Execution Workflow

### Phase 1: Establishing the Staging Factory (Local Directory Structure)
To prevent file corruption and tool confusion, clean workspace separation is required:
1. Created a root directory on the local drive: `C:\IntunePackaging`.
2. Created two subdirectories inside the root: `Source` (housing raw installation media) and `Output` (designated for the final encrypted payload).
3. Placed the packaging utility `IntuneWinAppUtil.exe` directly into the root `C:\IntunePackaging` folder.

### Phase 2: Acquiring and Preparing the Source Media
1. Downloaded the official 64-bit installer for **Notepad++ v8.9.7** (`npp.8.9.7.Installer.x64.exe`).
2. Placed the raw executable securely inside the `C:\IntunePackaging\Source` directory.

### Phase 3: Encapsulating the Payload via Command Line
1. Launched the Windows **Command Prompt** with administrative privileges.
2. Executed directory navigation to point to the staging tool:
   ```cmd
   cd C:\IntunePackaging  
<img width="1920" height="1080" alt="Screenshot 2026-08-10 110441" src="https://github.com/user-attachments/assets/8d0a96a5-74be-4e2f-8f5d-afadac14c54a" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 110604" src="https://github.com/user-attachments/assets/1d214f78-8e9e-4af3-bb76-86cb579621ff" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 110801" src="https://github.com/user-attachments/assets/a5a99647-23ef-4bf0-b9e5-023478d6da68" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 111205" src="https://github.com/user-attachments/assets/3f71d40d-4430-4f9f-830d-971da4848cd5" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 111225" src="https://github.com/user-attachments/assets/38c5c57f-8780-4a2d-8b37-7a9611b043e9" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 111544" src="https://github.com/user-attachments/assets/5e0c6c65-b20e-42f4-9656-4fe82cda7b37" />
<img width="1920" height="1080" alt="Screenshot 2026-08-10 111650" src="https://github.com/user-attachments/assets/783ad534-7a06-4b3d-bfda-8684db8c2286" />
