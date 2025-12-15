# Configure-ProCom

A comprehensive PowerShell automation script for configuring Windows PCs for ProCom deployments. This script provides both interactive and quick-mode configuration options to streamline system setup.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
  - [Interactive Mode](#interactive-mode)
  - [Quick Mode](#quick-mode)
  - [Debug Mode](#debug-mode)
- [Configuration Files](#configuration-files)
  - [Software Installations](#software-installations)
  - [Network Drive Mapping](#network-drive-mapping)
  - [User Management](#user-management)
- [Features Overview](#features-overview)
- [Known Issues](#known-issues)
- [Credits](#credits)

## Features

- **Software Installation**: Install popular applications via Winget
  - Google Chrome
  - VLC Player
  - E-ID Middleware & Viewer (Belgian Government)
  - Adobe Acrobat Reader

- **Microsoft Office 365**: Automated installation with Dutch (nl-nl) configuration

- **Windows Updates**: Enable and apply the latest Windows updates

- **System Configuration**: 
  - User performance profile optimization
  - Device name management
  - Local user account creation and management
  - Password policy management

- **Extra Installations**: Support for custom software installations via CSV configuration

- **Network Drive Mapping**: Automated network drive configuration from CSV files

- **User Management**: Create local users and admin accounts with configurable settings

## Requirements

- **Windows Operating System** (Windows 10/11 recommended)
- **PowerShell 5.0+** or **Windows PowerShell ISE**
- **Administrator privileges** required
- **Execution Policy**: RemoteSigned, Bypass, or Unrestricted
  
  To set execution policy:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

- **Winget** (Windows Package Manager) installed and functional

## Installation

1. Clone or download this repository:
   ```bash
   git clone https://github.com/yourusername/PcConfig.git
   cd PcConfig
   ```

2. Ensure PowerShell is running as Administrator

3. (Optional) Adjust the execution policy:
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

## Usage

### Running the Script

Navigate to the script directory in PowerShell (as Administrator) and run:

```powershell
.\Configure-ProCom.ps1
```

### Interactive Mode

The default mode presents a menu with the following options:

```
1. Install basic software
2. Install Microsoft Office 365 NL
3. Update all software via Winget
4. Enable Windows updates and reboot
5. Set up basic machine configuration
6. Adjust user performance profile settings
7. Disable password change on next login of current user
8. Create local admin user
9. Change device name
10. Rename local user account name & rename user folder
11. Extra installs: Install folder executables, map network drives, and create local users
0. Exit
```

Select the desired option by entering the corresponding number.

### Quick Mode

For automated configuration without user interaction, use:

```powershell
.\Configure-ProCom.ps1 -Quick
```

**Quick mode applies the following sequence automatically:**
1. Basic machine configuration
2. User performance profile optimization
3. Basic software installation
4. Software updates
5. Extra installations from config files
6. Microsoft Office 365 installation
7. Windows updates and system reboot

**⚠️ Known Issues with Quick Mode:**
- Software installation may be incomplete when run via batch shortcut
- Some minor information displays may not show correctly on certain Windows versions
- Windows update failures may prevent automatic system reboot

### Debug Mode

For troubleshooting Winget issues, use:

```powershell
.\Configure-ProCom.ps1 -Debug
```

If Winget is not working properly, opening the Windows Store and updating through the debug menu (option 3) often resolves the issue.

## Configuration Files

The script supports external configuration files in CSV format for customized deployments.

### Software Installations

Create an `Install` folder in the script directory with the following structure:

**File: `.\Install\installations.csv`**

```csv
software;file;arguments
Google Drive;GoogleDrive.exe;
Custom App;CustomApp.exe;/silent /norestart
Installer Name;setup.exe;--quiet --accept-license
```

**Fields:**
- `software`: Display name for the installation
- `file`: Executable filename (must be in the Install folder)
- `arguments`: Command-line arguments (leave empty if none required)

### Network Drive Mapping

Create a `config` folder in the script directory with the following structure:

**File: `.\config\netdrive.csv`**

```csv
Drive;Path
N;\\server\data
B;\\test\Folder1
Z;\\fileserver\shared
```

**Fields:**
- `Drive`: Drive letter to map to (single letter)
- `Path`: UNC path to the network share

### User Management

**File: `.\config\users.csv`**

```csv
user;fullname;password
jdoe;John Doe;Password123
asmith;Amy Smith;SecurePass456
```

**Fields:**
- `user`: Login username (no spaces)
- `fullname`: Display name for the user account
- `password`: Password for the account (leave empty for no password)

## Features Overview

### 1. Basic Software Installation
Installs common productivity and security applications via Winget:
- Google Chrome
- VLC Media Player
- Belgian eID Middleware
- Belgian eID Viewer
- Adobe Acrobat Reader

### 2. Microsoft Office 365 Installation
- Automated installation with Dutch language configuration
- Creates desktop and Start Menu shortcuts for:
  - Word
  - Excel
  - PowerPoint
  - Outlook (classic)
- Removes Office Deployment Tool after installation

### 3. System Configuration
- **Performance Profile**: Optimizes system for best performance
- **Windows Updates**: Enables and applies system updates with automatic reboot
- **Device Naming**: Change computer name (requires reboot)

### 4. User Management
- Create local administrator accounts
- Set password policies
- Disable password change requirement on next login
- Support for bulk user creation via CSV

### 5. Extra Installations
- Install software from executable files in the Install folder
- Map network drives from configuration file
- Create local users from configuration file
- All operations integrated into a single menu option

## Known Issues

### Current Limitations

- **Taskbar Widgets**: Disabling taskbar Widgets via registry is currently blocked by Windows security. A workaround is being investigated.

- **Quick Mode Installation**: Sometimes software may not fully install when running quick mode through the batch shortcut. Running the script directly in PowerShell is recommended.

- **Windows Display**: Some minor information displays may not render correctly on certain Windows versions.

- **Windows Update Failures**: If a Windows update fails, the system may not reboot automatically. This is a Microsoft Windows issue, not a script issue.

## Troubleshooting

### Winget Not Working

1. Open Windows Store and check for updates
2. Run the script with Debug mode: `.\Configure-ProCom.ps1 -Debug`
3. Update Winget manually through the Windows Store
4. Check Windows Settings → Apps → Apps & features → App installer version

### Script Won't Run

- Ensure you're running PowerShell as Administrator
- Check execution policy: `Get-ExecutionPolicy`
- Set policy if needed: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
- Verify you're in the correct script directory

### Network Drives Not Mapping

- Verify network connectivity: `Test-NetConnection -ComputerName servername`
- Confirm UNC paths in CSV file are correct
- Check network share credentials and permissions
- Ensure CSV file uses correct delimiter (semicolon)

## Advanced Options

### Custom Execution

The script supports the following command-line switches:

```powershell
.\Configure-ProCom.ps1 -Quick      # Automated configuration
.\Configure-ProCom.ps1 -Advanced   # Advanced options (reserved)
.\Configure-ProCom.ps1 -Debug      # Debug mode for Winget
```

### Running from Batch File

Create a batch file to launch the script:

**File: `RunConfiguration.bat`**
```batch
@echo off
powershell.exe -ExecutionPolicy Bypass -Command "& '%~dp0Configure-ProCom.ps1' -Quick"
pause
```

## Credits

- **Author**: Michel Michaux
- **Version**: 1.0
- **Creation Date**: October 23, 2025
- **Last Updated**: December 2025

## Support

For issues or questions:
1. Check the Known Issues section above
2. Run in Debug mode to identify Winget problems
3. Review configuration files for proper CSV formatting
4. Ensure running with Administrator privileges

**Note**: This script is designed for ProCom deployments and may require customization for other environments. Always test in a non-production environment first.