
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

![Windows stop errors](images/figure-01.png)

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

![Initial BIOS storage detection](images/figure-02.png)

*Figure 2. Initial BIOS storage screen: M2_1 was not detected.*

![NVMe self-test](images/figure-03.png)

*Figure 3. NVMe self-test could not find a supported device.*

---

## 3. Create Recovery Media and Check the Disk

Using another computer, I downloaded Microsoft's official Windows 11 installation media tool and prepared a bootable USB drive.

I booted the affected desktop from the USB using the UEFI boot menu.

**Official Microsoft download page:**

https://www.microsoft.com/software-download/windows11

![Windows 11 installation media](images/figure-04.png)

*Figure 4. Official Windows 11 download page used to prepare installation media.*

![USB boot menu](images/figure-05.png)

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

![DiskPart results](images/figure-06.png)

*Figure 6. DiskPart initially showed the USB installer but not the internal SSD.*

---

## 4. Inspect and Reseat the NVMe SSD

Since the SSD was not consistently detected in BIOS, I decided to check its physical connection.

I completely shut down the computer, disconnected the power cable, and opened the PC case.

I located the **Verbatim Vi3000 1TB M.2 NVMe SSD** on the motherboard, carefully removed it, and securely reinstalled it into the M.2 slot.

After reconnecting the power and turning the computer back on, I entered BIOS and checked whether the SSD was detected.

![NVMe SSD inspection](images/figure-07.png)

*Figure 7. Verbatim Vi3000 M.2 NVMe SSD during the physical inspection.*

### SSD Detection After Reseating

After reseating the NVMe SSD, I checked the boot menu again and confirmed that **Windows Boot Manager (Verbatim Vi300)** was now detected.

This entry had been missing before, suggesting that reseating the SSD may have resolved a connection issue.

![Windows Boot Manager detected](images/figure-08.png)

*Figure 8. Windows Boot Manager appeared after reseating the NVMe SSD.*

The BIOS subsequently detected the Verbatim Vi3000 in the `M2_1` slot.

![SSD detected in BIOS](images/figure-09.png)

*Figure 9. BIOS subsequently detected the Verbatim NVMe drive in M2_1.*

The SSD was now detected as a Verbatim Vi3000 (approximately 1 TB).

Reseating may have restored a connection, but I could not confirm the precise reason for the earlier detection failure.

---

## 5. Attempt Windows Recovery

With the SSD visible again, I attempted to start the existing Windows installation and explored the Windows Recovery Environment (WinRE).

The PC could boot into Windows briefly, but freezing and stop errors continued.

![Windows stop error](images/figure-10.png)

*Figure 10. A further stop error occurred after the SSD became detectable again.*

### Windows Recovery Environment (WinRE) – System Freeze

I attempted to access Windows Recovery Environment (WinRE) to troubleshoot the issue, but the system became completely unresponsive.

Neither the mouse nor the Tab key worked, preventing me from selecting any recovery options.

Despite restarting the computer more than ten times, the same freezing issue continued to occur.

This made the situation more serious, as I could no longer access Windows or use its built-in recovery tools to troubleshoot the problem.

![Windows Recovery Environment](images/figure-11.png)

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

![Windows installation partitions](images/figure-12.png)

*Figure 12. Identifying the internal Windows partitions separately from the ESD-USB installer.*

![Unallocated SSD space](images/figure-13.png)

*Figure 13. Selecting unallocated space on the internal SSD for the clean installation.*

### Installing Windows 11

I selected the unallocated space and continued with Windows 11 installation.

![Windows 11 installation](images/figure-14.png)

*Figure 14. Windows 11 installation in progress.*

Once installation finished, I completed the initial Windows setup and confirmed the desktop could load.

![Windows 11 setup completed](images/figure-15.png)

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

![PowerShell SSD health check](images/figure-16.png)

*Figure 16. PowerShell storage health check (the corrected command succeeded).*

### 7.2 Check RAM with Windows Memory Diagnostic

I also started Windows Memory Diagnostic to check for possible memory issues.

I opened the Run dialog using `Windows + R` and entered:

```cmd
mdsched.exe
```

I selected the option to restart the computer and check for problems.

Windows restarted and launched the memory diagnostic tool.

![Windows Memory Diagnostic](images/figure-17.png)

*Figure 17. Windows Memory Diagnostic running.*

The diagnostic process was captured, but I did not capture a verified final result. Therefore, I have not claimed that the RAM passed the test.

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

![OneDrive synchronisation](images/figure-18.png)

*Figure 18. OneDrive and application configuration during post-installation setup.*

### 8.3 Check Windows Updates

As the final configuration step, I opened:

**Settings → Windows Update**

I checked for available Windows system and security updates.

![Windows Update](images/figure-19.png)

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
