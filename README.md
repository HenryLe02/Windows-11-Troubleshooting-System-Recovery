
# Windows 11 Troubleshooting & System Recovery

**Project 6 | Real-world Desktop Troubleshooting | IT Support Portfolio**

I troubleshot a Windows 11 desktop that repeatedly froze, restarted unexpectedly, and experienced Blue Screen of Death (BSOD) errors.

I investigated potential hardware and software issues, checked BIOS and SSD detection, and attempted several recovery methods. Despite multiple troubleshooting attempts and restarting the computer more than ten times, the system remained unstable and frequently became unresponsive.

As a last resort, I performed a clean installation of Windows 11 with the user's permission. After reinstalling Windows, I checked the hardware status, reinstalled essential applications, configured OneDrive to restore access to the user's cloud files, and verified Windows Updates to ensure the system was ready for everyday use.

---

## Table of Contents

1. [Issue Overview](#1-issue-overview)
2. [Check the BIOS and Storage Detection](#2-check-the-bios-and-storage-detection)
3. [Create Recovery Media and Check the Disk](#3-create-recovery-media-and-check-the-disk)
4. [Inspect and Reseat the NVMe SSD](#4-inspect-and-reseat-the-nvme-ssd)
5. [Attempt Windows Recovery](#5-attempt-windows-recovery)
6. [Clean Install Windows 11](#6-clean-install-windows-11)
7. [Post-Installation Hardware Checks](#7-post-installation-hardware-checks)
8. [Restore Applications, OneDrive and Updates](#8-restore-applications-onedrive-and-updates)
9. [Outcome and Lessons Learned](#9-outcome-and-lessons-learned)

---

## 1. Issue Overview

The user reported that the desktop had become increasingly unstable. According to the user, a child frequently used the computer without adult supervision, downloaded games from unofficial websites, and clicked on unknown or potentially unsafe links.

This raised concerns about possible malware infections or unwanted software that could have affected the system's stability. However, I treated this as a potential security risk rather than a confirmed cause of the issue.

### Symptoms

- Windows sometimes reached the desktop, but applications became unresponsive shortly afterward.
- The computer froze, restarted unexpectedly, and showed stop errors including `CRITICAL_PROCESS_DIED (0xEF)` and `DPC_WATCHDOG_VIOLATION (0x133)`.
- Recovery attempts were inconsistent, so I checked whether the system drive was being detected.

<img width="733" height="406" alt="image" src="https://github.com/user-attachments/assets/03556bef-e118-46d1-8462-f2da9a3c13f4" />

<img width="900" height="330" alt="image" src="https://github.com/user-attachments/assets/67ca8195-eee0-4c11-ab2d-99cadc8d98d2" />

*Figure 1. Windows stop errors captured during the troubleshooting process.*

---

## 2. Check the BIOS and Storage Detection

I entered MSI Click BIOS 5 and reviewed the basic hardware information.

The system showed the following hardware:

| Component | Details |
|---|---|
| Motherboard | MSI PRO H610M-G WIFI DDR4 |
| Processor | Intel Core i5-12400F |
| Memory | 32 GB RAM |
| Storage | Verbatim Vi3000 1TB M.2 NVMe SSD |
| Operating System | Windows 11 |

The temperatures shown did not suggest an obvious overheating problem at that moment.

More importantly, the `M2_1` slot showed **Not Present** during an initial check.

The BIOS NVMe self-test also reported that no supported NVMe device was found.

This raised a question about SSD detection, although it did not establish that the drive had failed.

<img width="927" height="640" alt="image" src="https://github.com/user-attachments/assets/be65a743-2050-40d9-b6d4-a216f2f99a27" />

<img width="812" height="646" alt="image" src="https://github.com/user-attachments/assets/facff172-3d6f-4945-803f-fb3edd1dded4" />

<img width="582" height="345" alt="image" src="https://github.com/user-attachments/assets/55370f23-2984-4431-9fe4-32998c115c76" />

*Figure 2. Initial BIOS storage screen: M2_1 was not detected.*

<img width="456" height="357" alt="image" src="https://github.com/user-attachments/assets/1c9dd091-58fc-4448-ae1a-3bbc246b10a4" />


*Figure 3. NVMe self-test could not find a supported device.*

---

## 3. Create Recovery Media and Check the Disk

Using another computer, I downloaded Microsoft's official Windows 11 installation media tool and prepared a bootable USB drive.

<img width="317" height="243" alt="image" src="https://github.com/user-attachments/assets/a323914a-4729-457d-b5f5-fc5ee1e793ac" />

I booted the affected desktop from the USB using the UEFI boot menu.

**Official Microsoft download page:**

https://www.microsoft.com/software-download/windows11

<img width="925" height="640" alt="image" src="https://github.com/user-attachments/assets/c4497919-0ea6-4674-b9a3-bdddb4e8eaa4" />

*Figure 4. Official Windows 11 download page used to prepare installation media.*

<img width="618" height="232" alt="image" src="https://github.com/user-attachments/assets/3fcae57c-6c2e-4fae-aa98-56bb1499efbd" />
<img width="768" height="362" alt="image" src="https://github.com/user-attachments/assets/bbe3f679-cf66-46e7-b4eb-0098edf1686b" />


*Figure 5. Selecting the USB drive in the boot menu.*

### Checking the Disk with DiskPart

From Windows Setup, I opened Command Prompt and used DiskPart to inspect available disks and volumes.

Commands used:

```cmd
diskpart
list disk
list volume
exit
```

At that point, the only visible disk was the approximately 28 GB ESD-USB installer.

The internal SSD was not listed, which supported the need for a physical connection check.

<img width="677" height="490" alt="image" src="https://github.com/user-attachments/assets/2d1e8f98-c94f-4fe8-b9fa-bda91d8e4310" />


*Figure 6. DiskPart initially showed the USB installer but not the internal SSD.*

---

## 4. Inspect and Reseat the NVMe SSD

Since the SSD was not consistently detected in BIOS, I decided to check its physical connection.

I completely shut down the computer, disconnected the power cable, and opened the PC case.

I located the Verbatim Vi3000 1TB M.2 NVMe SSD on the motherboard and carefully inspected the SSD and M.2 slot for any visible signs of physical damage. After confirming there was no obvious damage, I removed the SSD and securely reinstalled it into the M.2 slot.

After reconnecting the power and turning the computer back on, I entered BIOS and checked whether the SSD was detected.

<img width="561" height="500" alt="image" src="https://github.com/user-attachments/assets/7cec6b47-cf2f-4365-a3d5-8d701bd7d3ec" />


*Figure 7. Verbatim Vi3000 M.2 NVMe SSD during the physical inspection.*

### SSD Detection After Reseating

After reseating the NVMe SSD, I checked the boot menu again and confirmed that **Windows Boot Manager (Verbatim Vi300)** was now detected.

This entry had been missing before, suggesting that reseating the SSD may have resolved a connection issue.

<img width="942" height="392" alt="image" src="https://github.com/user-attachments/assets/eff6147c-0a89-4f3a-b2f5-332b1bdb9819" />



*Figure 8. Windows Boot Manager appeared after reseating the NVMe SSD.*

The BIOS subsequently detected the Verbatim Vi3000 in the `M2_1` slot.

<img width="928" height="607" alt="image" src="https://github.com/user-attachments/assets/50d3403b-9c60-4d23-bde2-4490ac7eef8b" />




*Figure 9. BIOS subsequently detected the Verbatim NVMe drive in M2_1.*

The SSD was now detected as a Verbatim Vi3000 (approximately 1 TB).

Reseating may have restored a connection, but I could not confirm the precise reason for the earlier detection failure.

---

## 5. Attempt Windows Recovery

With the SSD visible again, I attempted to start the existing Windows installation and explored the Windows Recovery Environment (WinRE).

The PC could boot into Windows briefly, but freezing and stop errors continued.

<img width="576" height="377" alt="image" src="https://github.com/user-attachments/assets/d4728cab-9410-4598-822c-779246a788ea" />


*Figure 10. A further stop error occurred after the SSD became detectable again.*

### Windows Recovery Environment (WinRE) – System Freeze

I attempted to access Windows Recovery Environment (WinRE) to troubleshoot the issue, but the system became completely unresponsive.

Neither the mouse nor the Tab key worked, preventing me from selecting any recovery options.

Despite restarting the computer more than ten times, the same freezing issue continued to occur.

This made the situation more serious, as I could no longer access Windows or use its built-in recovery tools to troubleshoot the problem.

<img width="581" height="423" alt="image" src="https://github.com/user-attachments/assets/822b32d9-3c06-41d1-917e-8648477a3c5c" />

*Figure 11. Windows Recovery Environment was accessed, but recovery did not resolve the instability.*

### Decision to Perform a Clean Installation

Because recovery attempts did not restore a usable system, I discussed the next step with the user.

After checking the data requirements and obtaining permission for a clean installation, I proceeded with Windows 11 setup.

A clean installation was a recovery decision, not evidence that the original root cause had been conclusively identified.

---

## 6. Clean Install Windows 11

In Windows Setup, I confirmed that:

- **Disk 0:** Internal approximately 1 TB SSD
- **Disk 1:** USB installation drive

I removed the old partitions on the internal installation target and selected its unallocated space for Windows 11.

I left the USB installation drive untouched.

**Important:** Deleting partitions removes existing data. I confirmed the correct installation target and obtained the user's permission before proceeding.

<img width="712" height="527" alt="image" src="https://github.com/user-attachments/assets/5a6d229f-5c8a-4702-a507-018a156efa61" />


*Figure 12. Identifying the internal Windows partitions separately from the ESD-USB installer.*

<img width="875" height="513" alt="image" src="https://github.com/user-attachments/assets/6dd189c0-fb93-460e-a402-a80b96b9339f" />


*Figure 13. Selecting unallocated space on the internal SSD for the clean installation.*

### Installing Windows 11

I selected the unallocated space and continued with Windows 11 installation.

<img width="690" height="377" alt="image" src="https://github.com/user-attachments/assets/9dfd37c3-9cf4-41a8-8d71-edfdb7fbc4c6" />


*Figure 14. Windows 11 installation in progress.*

Once installation finished, I completed the initial Windows setup and confirmed the desktop could load.

<img width="865" height="537" alt="image" src="https://github.com/user-attachments/assets/826f3053-e51a-43be-9b96-dc7980a88af9" />


*Figure 15. Windows 11 initial setup completed.*

---

## 7. Post-Installation Hardware Checks

After installing Windows 11, I performed additional checks to assess the hardware status.

### 7.1 Check SSD Health with PowerShell

I opened Windows PowerShell as administrator and ran:

```powershell
Get-PhysicalDisk | Select FriendlyName, HealthStatus, OperationalStatus
```

The result showed:

| FriendlyName | HealthStatus | OperationalStatus |
|---|---|---|
| Verbatim Vi3000 | Healthy | OK |

This confirmed that Windows currently reported the SSD as healthy and operational.

However, this was not a full SSD diagnostic and did not guarantee that an intermittent fault could not recur.

<img width="846" height="426" alt="image" src="https://github.com/user-attachments/assets/8b4b519d-1e0e-4d63-bd39-1b98ef40b81a" />

*Figure 16. PowerShell storage health check (the corrected command succeeded).*

### 7.2 Check RAM with Windows Memory Diagnostic

I also started Windows Memory Diagnostic to check for possible memory issues.

I opened the Run dialog using `Windows + R` and entered:

```cmd
mdsched.exe
```

I selected the option to restart the computer and check for problems.

Windows restarted and launched the memory diagnostic tool.

<img width="718" height="387" alt="image" src="https://github.com/user-attachments/assets/29108d06-75cd-4043-9223-5103e3803150" />

<img width="661" height="482" alt="image" src="https://github.com/user-attachments/assets/33dbf27c-f0c8-4225-8b9e-57ff6173942c" />


*Figure 17. Windows Memory Diagnostic running.*

After the Windows Memory Diagnostic test finished, the computer automatically restarted and booted into Windows without displaying the final test results. No errors were reported during the portion of the test I observed.

---

## 8. Restore Applications, OneDrive and Updates

After reinstalling Windows 11, I completed the final configuration to make the computer usable again.

### 8.1 Reinstall Essential Applications

I reinstalled the essential applications required by the user and checked that they could be opened.

### 8.2 Restore Microsoft Account and OneDrive

I restored the user's Microsoft/OneDrive access and checked that cloud synchronisation resumed.

OneDrive eventually displayed:

**Backed up and synced**

This confirmed that OneDrive was connected and reported successful synchronisation at that time.

<img width="781" height="482" alt="image" src="https://github.com/user-attachments/assets/5fc9003e-22ee-4c43-b10f-fdc8525358ce" />

*Figure 18. OneDrive and application configuration during post-installation setup.*

### 8.3 Check Windows Updates

As the final configuration step, I opened:

**Settings → Windows Update**

I checked for available Windows system and security updates.

<img width="792" height="516" alt="image" src="https://github.com/user-attachments/assets/470d18e3-55f8-4ce4-8419-17fbf0513c54" />


*Figure 19. Windows Update check during final configuration.*

The available screenshot shows that Windows Update was checked, but it does not establish that every update had finished installing.

A final "You're up to date" screenshot and a successful restart test would strengthen this validation.

---

## 9. Outcome and Lessons Learned

### Outcome

- Restored Windows 11 to a usable state through a clean installation.
- Recovered BIOS detection of the Verbatim NVMe SSD after reseating it.
- Verified that Windows reported the SSD as **Healthy / OK** after installation.
- Reconfigured essential applications and OneDrive access.
- Checked Windows Update as part of the final setup.

### What I Learned

This project gave me practical experience troubleshooting a real Windows desktop with both hardware detection issues and operating system instability.

The main lessons I learned were:

1. **Check hardware detection first.** A Windows boot problem is not always caused by the operating system itself.
2. **Use multiple diagnostic tools.** BIOS and DiskPart provided useful evidence when investigating the missing SSD.
3. **Verify the correct disk before installation.** Selecting the wrong drive during a clean installation could result in data loss.
4. **Communicate with the user.** I confirmed the user's data requirements and obtained permission before deleting partitions.
5. **Validate the system after recovery.** Reinstalling Windows was only part of the process. Hardware checks, applications, cloud synchronisation, and Windows Updates were also important.
6. **Avoid assuming the root cause.** Although the system became usable again, I could not conclusively determine whether the original instability was caused by software corruption, SSD connection problems, or another hardware issue.

### Limitations

I did not establish whether unofficial game downloads caused the failure.

The initial SSD detection problem may warrant further monitoring.

A documented final memory-test result and longer stability testing would make the final validation stronger.

---

## Tools and Skills Demonstrated

| Category | Tools / Skills |
|---|---|
| Hardware troubleshooting | NVMe SSD inspection and reseating |
| BIOS / UEFI | MSI Click BIOS 5, boot device selection |
| Storage diagnostics | DiskPart, PowerShell Get-PhysicalDisk |
| Windows recovery | Windows Recovery Environment (WinRE) |
| Operating system deployment | Windows 11 bootable USB, clean installation |
| Memory diagnostics | Windows Memory Diagnostic |
| User configuration | Essential applications, Microsoft account, OneDrive |
| System maintenance | Windows Update |
| IT Support practices | Troubleshooting, documentation, user communication |

---

## Final Reflection

This was a valuable real-world IT Support experience because I had to troubleshoot an unstable computer rather than follow a predefined lab exercise.

I worked through the issue step by step, investigated the storage detection problem, attempted recovery, and ultimately restored the computer to a usable state through a clean Windows 11 installation.

The project strengthened my confidence in troubleshooting Windows, working with BIOS and storage devices, and preparing a computer for an end user.

---

**Project 6 — IT Support Portfolio**
