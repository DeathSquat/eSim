# eSim Semester Long Internship Spring 2026

**Title** : eSim Upgradation
(CSE and Related Fields (Task - 4))
**Author** : Nishchay Chaurasia
**College Name** : Dronacharya Group of Institutions, Greater Noida, Uttar Pradesh.

**Description/Theory** : The task involves to figure out the dependency issues while installing eSim 2.5 on Ubuntu 25. and fix atleast one of the Issues.
 
# eSim 2.5 Installation Report on Ubuntu 25.04

## 1. Introduction
This report documents the issues encountered while installing **eSim 2.5** on **Ubuntu 25.04** and the fixes applied.
Since eSim officially supports older Ubuntu versions, several dependency and compatibility issues were observed on Ubuntu 25.04.
The objective of this task was to identify these issues, fix at least one major issue, and document the complete process.

---

## 2. System Details
- Operating System: Ubuntu 25.04 (Plucky Puffin)
- Architecture: 64-bit
- eSim Version: 2.5
- Installation Method: install-eSim.sh script
- Platform: Virtual Machine (VirtualBox)

---

## 3. Repository Details
- Base Repository: https://github.com/FOSSEE/eSim
- Forked Repository: https://github.com/DeathSquat/eSim
- Branch Used: installer

---

## 4. Issues Encountered and Fixes

---

## Issue 1: Unsupported Ubuntu Version (Critical Issue)

### Error Message
- Detected Ubuntu Version:
- Unsupported Ubuntu version: 25.04

### Cause
The `install-eSim.sh` script explicitly checks for supported Ubuntu versions and does not allow Ubuntu 25.04.
Ubuntu 25.04 is not included in the allowed list, causing the installer to terminate immediately.

### Fix Applied
The Ubuntu version validation block in `install-eSim.sh` was edited to allow installation on Ubuntu 25.04.

### Code Change
#    case $VERSION_ID in
#        "22.04")
#            if [[ "$FULL_VERSION" == "22.04.4" ]]; then
#                SCRIPT="$SCRIPT_DIR/install-eSim-22.04.sh"
#            else
#                SCRIPT="$SCRIPT_DIR/install-eSim-23.04.sh"
#            fi
#            ;;
#        "23.04")
#            SCRIPT="$SCRIPT_DIR/install-eSim-23.04.sh"
#            ;;
#        "24.04" | "25.04")
#            SCRIPT="$SCRIPT_DIR/install-eSim-24.04.sh"
#            ;;
#        *)
#            echo "Unsupported Ubuntu version: $VERSION_ID ($FULL_VERSION)"
#            exit 1
#            ;;
#    esac

### Result
- After applying this fix, the installer proceeded to the dependency installation stage successfully.

## Issue 2: APT Package Fetch Failure (Network / Mirror Issue)

### Error Message
- Err:6 http://in.archive.ubuntu.com/ubuntu plucky/universe amd64 python3-virtualenv all 20.29.1+ds-1
-  Undetermined Error [IP: 2620:2d:4000:1::102 80]
- Get:3 http://in.archive.ubuntu.com/ubuntu plucky-updates/universe amd64 python3-pip-whl all 25.0+dfsg-1ubuntu0.2 [1,446 kB]
- Get:4 http://in.archive.ubuntu.com/ubuntu plucky-updates/universe amd64 python3-setuptools-whl all 75.8.0-1ubuntu1 [1,153 kB]
- Fetched 2,599 kB in 9s (276 kB/s)
- Error: Failed to fetch http://in.archive.ubuntu.com/admin/login.asp  Undetermined Error [IP: 2620:2d:4000:1::102 80]
- Error: Failed to fetch http://in.archive.ubuntu.com/admin/login.asp  Undetermined Error [IP: 2620:2d:4000:1::102 80]
- Error: Failed to fetch http://in.archive.ubuntu.com/admin/login.asp  Undetermined Error [IP: 2620:2d:4000:1::102 80]
- Error: Failed to fetch http://in.archive.ubuntu.com/admin/login.asp  Undetermined Error [IP: 2620:2d:4000:1::102 80]
- Error: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?


- Error! Kindly resolve above error(s) and try again.

- Aborting Installation...

### Cause

Ubuntu 25.04 uses newer package repositories.
The default Indian mirror (in.archive.ubuntu.com) failed over IPv6, causing repeated package download failures during dependency installation.

### Fix Applied

APT was forced to use IPv4 by creating a configuration file.

### Steps Taken
# sudo nano /etc/apt/apt.conf.d/99force-ipv4


### File Changes:

# Acquire::ForceIPv4 "true";


### Then:

# sudo apt clean
# sudo apt update

### Result

- Package downloads worked correctly after forcing IPv4, and the installer progressed further.
- Python distutils and PyQt5 could have been the Potential Errors, but after the IPv4 Fix. None of the errors occured.

## Conclusion

- During the installation of eSim 2.5 on Ubuntu 25.04, multiple compatibility and dependency issues were identified.
- A critical installer blocker related to unsupported Ubuntu version was fixed, along with a network-related APT issue.
- These fixes improve eSim’s compatibility with newer Ubuntu releases and allow the installer to proceed beyond the initial failure stages.

## Issue 3: KiCad PPA Not Available for Ubuntu 25.

### Error Observed

- During KiCad installation, the installer failed with the following error:
- 404 Not Found
- https://ppa.launchpadcontent.net/kicad/kicad-6.0-releases/ubuntu plucky Release

### Root Cause

- The KiCad 6.0 PPA does not provide packages for Ubuntu 25.04 (plucky).
- The installer assumes that the PPA is available for all Ubuntu versions.

### Impact

- Due to the missing PPA, the installer aborted during the KiCad installation step.

### Status

- The issue was identified and documented. No fix was applied since the PPA is not maintained for Ubuntu 25.04.

## Git Workflow Observation

- The forked repository initially contained only the master branch.
- The installer Branch was locally created from the GUI (GitHub Website) and Modifications were Committed too.
