---
title: "Multi-Platform Development Tools Installer"
date: "2025-02-07"
author: "fernand3z"
description: "A streamlined installer to set up a development environment across Linux, macOS, and Windows."
tags: ["development-tools", "automation", "linux", "macos", "windows", "cross-platform", "installer", "zsh", "docker", "python", "nodejs", "git", "vscode", "terminal"]
categories: ["Development Tools", "Automation", "DevOps"]
---

Setting up a development environment across multiple operating systems can be tedious. To simplify this, the [Multi-Platform Development Tools Installer](https://github.com/fernand3z/dev-toolbox) helps developers install essential tools in a structured and customizable way on Linux, macOS, and Windows.

## Features

### Terminal Enhancements
- **zsh**
- **oh-my-zsh** (with syntax highlighting & autosuggestions)
- **Starship Prompt** (Nerd Font preset)
- **fzf** (fuzzy finder)
- **zoxide** (smart directory jumping, initialized with `--cmd cd`)

### Development Environment Tools
- **nvm** (Node.js version manager) & Node.js
- **Python** (with pip)
- **Docker**
- **Git**
- **Visual Studio Code**
- **PyCharm Community Edition**

## Customizable Installation
Choose to install all tools or select specific categories:
- Terminal Enhancements
- Development Environment Tools

## Platform-Specific Installers
- **Linux**: Uses `apt` (Debian/Ubuntu-based), requires `sudo` privileges upfront.
- **macOS**: Uses `Homebrew` for package management.
- **Windows**: Uses `winget` and a batch file (`run_install_windows.bat`) for admin privilege elevation.

## File Structure

```plaintext
project/
├── install.sh
├── linux/
│   └── install_linux.sh
├── macos/
│   └── install_macos.sh
└── windows/
    ├── install_windows.sh
    └── run_install_windows.bat
```

### File Descriptions
- `install.sh`: Master script that sets permissions and launches the OS-specific installer.
- `linux/install_linux.sh`: Installs tools on Linux, requesting `sudo` at the beginning.
- `macos/install_macos.sh`: Uses `Homebrew` to install necessary tools on macOS.
- `windows/install_windows.sh`: Installs tools on Windows using `winget`, intended for a Bash environment (e.g., Git Bash).
- `windows/run_install_windows.bat`: A batch wrapper that requests admin privileges via UAC before launching the Windows installer.

## Prerequisites

### Linux
- Debian/Ubuntu-based system
- `sudo` privileges
- Internet connectivity

### macOS
- Homebrew installed
- Internet connectivity

### Windows
- Bash environment (e.g., Git Bash)
- `winget` installed
- To run the installer with admin privileges, double-click `windows/run_install_windows.bat`

## Installation Guide

### Clone the Repository
```sh
git clone https://github.com/fernand3z/dev-toolbox.git
cd dev-toolbox
```

### Run the Master Installer
Make sure the script has execute permissions (for macOS/Linux):
```sh
chmod +x install.sh
./install.sh
```

#### Installation Options
You will be prompted to select your operating system and choose an installation option:
- **Install All** - Installs both terminal enhancements and development environment tools.
- **Customize Installation** - Select which category of tools to install.

### Windows Users
Instead of running the Bash script directly, double-click the batch file:
```plaintext
windows/run_install_windows.bat
```
This requests administrative privileges and launches the Windows installer.

## Customization
The installer scripts are structured into categories. You can modify the scripts to:
- Add new tools
- Remove unwanted tools
- Adjust configurations based on your needs

## Contributing
Contributions are welcome! If you have suggestions, improvements, or find bugs, feel free to open an issue or submit a pull request.

## License
This project is licensed under the **MIT License**.

## Disclaimer
These scripts are provided as examples. Depending on your system configuration and tool versions, you may need to adjust them for optimal performance.

For more details, visit the repository: [GitHub - fernand3z/dev-toolbox](https://github.com/fernand3z/dev-toolbox). 