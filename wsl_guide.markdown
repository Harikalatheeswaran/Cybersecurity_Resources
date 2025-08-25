# Windows Subsystem for Linux (WSL) Guide

## What is WSL and What Does It Do?
The Windows Subsystem for Linux (WSL) is a compatibility layer in Windows that allows you to run Linux distributions and tools natively without a traditional virtual machine (VM) or dual-boot setup. Key features:
- Run Linux commands, scripts, and tools (e.g., `bash`, `grep`, `sed`) within Windows.
- Develop, test, and deploy Linux-compatible software on a Windows machine.
- Access Windows files from Linux and vice versa for seamless integration.
- Supports multiple Linux distributions (e.g., Ubuntu, Kali, Debian) side by side.

**WSL1** translates Linux system calls to Windows kernel calls, while **WSL2** uses a lightweight VM with a Microsoft-provided Linux kernel for better performance and full system call compatibility.

## Does WSL Install a Linux Distro as a VM?
- **WSL1**: Not a VM; it translates Linux system calls to Windows in real-time.
- **WSL2**: Uses a lightweight VM with a managed Linux kernel, not a traditional VM like VirtualBox, but a container-like environment with a small footprint.
- **Difference**: WSL2 offers better performance for I/O-heavy tasks and full kernel support, while WSL1 is lighter but less compatible.

## Does WSL Allow Linux Commands Inside Windows CMD?
- Linux commands run in a WSL terminal (e.g., bash shell) for a specific distro, not directly in CMD.
- From CMD, PowerShell, or Windows Terminal, use `wsl <command>` (e.g., `wsl ls -la`) to run Linux commands.
- **Windows Terminal** (recommended) integrates WSL distros alongside CMD/PowerShell.

## Steps to Install Kali Linux and Ubuntu Using WSL
### Prerequisites
- Windows 10 (version 1903 or higher) or Windows 11.
- WSL2 enabled (preferred for performance).
- Admin privileges for setup.
- Microsoft Store access or manual download for distros.

### Step 1: Enable WSL and Set Up WSL2
1. Open PowerShell as Administrator.
2. Enable WSL and install default Ubuntu:
   ```powershell
   wsl --install
   ```
   This enables WSL, the Virtual Machine Platform, and sets WSL2 as default.
3. If WSL is installed, set WSL2 as default:
   ```powershell
   wsl --set-default-version 2
   ```
4. Restart your computer if prompted.

### Step 2: Install Ubuntu
1. **Microsoft Store**:
   - Open Microsoft Store, search for “Ubuntu” (e.g., Ubuntu 22.04 LTS), and click “Install.”
   - Launch Ubuntu from the Start menu.
2. **Manual Download**:
   - Download the Ubuntu `.appx` package from Microsoft’s GitHub or Canonical’s site.
   - Run in PowerShell:
     ```powershell
     Add-AppxPackage .\Ubuntu_2204.1.7.0_x64.appx
     ```
3. On first launch, Ubuntu sets up (~1 minute). Set a username and password.
4. Verify WSL2:
   ```powershell
   wsl --list --verbose
   ```
   Ensure Ubuntu shows “2” under VERSION.

### Step 3: Install Kali Linux
1. **Microsoft Store**:
   - Search for “Kali Linux” in Microsoft Store and install.
   - Launch Kali from the Start menu.
2. **Manual Download**:
   - Download the Kali WSL `.appx` package from Kali’s site or Microsoft.
   - Install via PowerShell:
     ```powershell
     Add-AppxPackage .\KaliLinux_<version>.appx
     ```
3. On first launch, set up a username and password.
4. Verify WSL2:
   ```powershell
   wsl --list --verbose
   ```
   Ensure Kali shows “2” under VERSION.

### Step 4: Access Distros
- In Windows Terminal or CMD:
  - Ubuntu: `wsl -d Ubuntu`
  - Kali: `wsl -d kali-linux`
- Or launch from the Start menu.

## Enabling GUI Support for Kali Linux
WSL2 supports GUI apps via `wslg` (WSL GUI). For Kali, use **Win-KeX** (Kali’s Windows-Kex) for a seamless desktop experience.

### Steps to Enable GUI for Kali
1. Update Kali:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Win-KeX:
   ```bash
   sudo apt install -y kali-win-kex
   ```
3. Start Win-KeX in windowed mode:
   ```bash
   kex --win
   ```
   - This launches a Kali desktop via RDP in a window.
   - Alternatives: `kex --sl` (seamless mode) or `kex --esm` (enhanced session mode).
4. (Optional) Install additional Kali tools for GUI apps:
   ```bash
   sudo apt install -y kali-linux-default
   ```
5. If GUI fails, ensure WSL2 and `wslg` are active:
   ```powershell
   wsl --update
   ```
6. Access the desktop via Windows RDP client or directly in the Win-KeX window.

### Requirements for GUI
- WSL2 (not WSL1).
- Windows 11 or Windows 10 (version 21H2 or higher) for `wslg`.
- ~2-3 GB additional disk space for Win-KeX and GUI tools.
- RDP client (included in Windows) for Win-KeX’s default mode.

## Disk Space Usage
WSL consumes disk space on the host Windows system:
- **WSL2 VM**: ~1-2 GB for the Linux kernel and virtual disk (VHDX).
- **Ubuntu**:
  - Base install: ~600 MB.
  - With packages (e.g., development tools): ~2-5 GB.
- **Kali Linux**:
  - Base install: ~1-2 GB.
  - With `kali-linux-default`: ~5-10 GB.
  - With `kali-linux-large` or GUI tools (Win-KeX): ~10-20 GB+.
- **Win-KeX (GUI)**: ~2-3 GB for desktop environment and dependencies.
- **VHDX Expansion**: WSL2’s virtual disk grows dynamically (up to 250 GB default) as you add files/packages.
- **Location**: 
  - VHDX: `%userprofile%\AppData\Local\Docker\wsl\data`.
  - Distro files: `%userprofile%\AppData\Local\Packages`.
- **Management**:
  - Check usage: `wsl --list --verbose`.
  - Reclaim space: `wsl --manage <distro> --set-sparse true`.
  - Remove distro: `wsl --unregister <distro>`.

**Total Estimate**:
- Ubuntu (base) + Kali (base) + WSL2: ~3-5 GB.
- With Kali GUI (Win-KeX) and tools: ~10-25 GB, depending on packages.

## Using Linux Commands Without WSL
Alternatives to run Linux commands on Windows without WSL:
1. **Git Bash**: Install Git for Windows; includes bash and commands like `ls`, `grep`. Limited scope.
2. **Cygwin**: POSIX environment with Linux tools. Complex setup, no VM.
3. **MSYS2**: Unix-like environment with `pacman` package manager.
4. **PowerShell**: Native equivalents (e.g., `dir` for `ls`, `findstr` for `grep`). Not full Linux support.
5. **Third-Party Tools**: Install tools like `sed`, `awk` via GnuWin32.
6. **Virtual Machines**: Full Linux VM via VirtualBox/Hyper-V (heavier than WSL).

These are less integrated than WSL for Linux workflows.

## Why Was WSL Created?
Microsoft created WSL to:
- Enable Linux tools on Windows without dual-booting or heavy VMs.
- Simplify cross-platform development (e.g., Linux scripts on Windows).
- Attract developers preferring Linux environments on Windows hardware.
- Support enterprise Linux workloads (e.g., Docker, cloud tools).
- **Evolution**: WSL1 (2016) focused on command-line; WSL2 (2019) added a full Linux kernel via virtualization for better performance.

## Additional Notes
- **Performance**: WSL2 is faster for I/O and CPU tasks but uses ~500 MB memory overhead.
- **Networking**: WSL2’s virtualized network may need configuration for tools like Kali’s penetration testing suite.
- **Updates**: Keep distros updated (`sudo apt update && sudo apt upgrade`) and WSL (`wsl --update`).
- **Recommendation**: Use Windows Terminal for a unified CLI experience.