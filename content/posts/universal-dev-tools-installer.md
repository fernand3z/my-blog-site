---
title: "Universal Dev Tools Installer for Multiple Platforms"
date: 2025-02-02
description: "Set up your development environment on Linux, macOS, and Windows using a few simple scripts"
tags: ["development", "tools", "automation", "linux", "macos", "windows"]
categories: ["Development Tools", "Tutorials"]
---

Set up your development environment on Linux, macOS, and Windows using a few simple scripts.

## Repository Overview

In the `fernand3z/dev-tools` repository, you'll find a set of scripts designed to automatically install your favorite development tools—such as zsh, oh‑my‑zsh, starship, nvm/node, python3/pip3, docker, git, VS Code, and PyCharm Community Edition—on Linux, macOS, and Windows.

The repository is organized into a few key directories and files:

```bash
fernand3z/dev-tools/
├── install.sh           # The master installer that sets permissions and lets you choose an OS.
├── linux/
│   └── install_linux.sh # A universal Linux installer that auto-detects your package manager.
├── macos/
│   └── install_macos.sh # A macOS installer using Homebrew.
└── windows/
    ├── install_windows.sh   # A Windows installer using winget.
    └── run_install_windows.bat  # A batch file that requests admin privileges.
```

Each platform has its own script, but the master installer (`install.sh`) ties everything together.

## The Master Installer Script

The master script ensures that all sub‑scripts are executable and then prompts you to select your operating system. Here's a brief look:

```bash
#!/bin/bash
# install.sh – Master installer

# Ensure all sub-scripts are executable.
chmod +x linux/install_linux.sh macos/install_macos.sh windows/install_windows.sh

echo "Select your operating system:"
echo "1) Linux"
echo "2) macOS"
echo "3) Windows"

read -rp "Enter choice [1-3]: " choice

case $choice in
  1) bash linux/install_linux.sh ;;
  2) bash macos/install_macos.sh ;;
  3) bash windows/install_windows.sh ;;
  *) echo "Invalid choice. Exiting."; exit 1 ;;
esac
```

What's happening here?

1. The `chmod +x` command makes sure all installer scripts are executable.
2. A simple menu is shown so you can pick the right installer for your OS.

This script is the entry point, making it easy for anyone to get started.

## Linux Installer Script – Key Sections

The Linux installer (`linux/install_linux.sh`) is built to work on nearly any Linux distribution by auto-detecting your package manager. Below are the most important code sections:

### 1. Requesting Sudo Privileges & Keep-Alive

```bash
# Request sudo privileges upfront.
sudo -v

# Keep-alive: update existing sudo timestamp until the script finishes.
while true; do 
    sudo -n true; 
    sleep 60; 
    kill -0 "$$" || exit; 
done 2>/dev/null &
```

Explanation:

- `sudo -v` prompts you for your password at the start so that later commands can run with elevated privileges.
- The keep-alive loop refreshes the sudo session every 60 seconds, ensuring you won't be interrupted during a long installation process.

### 2. Package Manager Detection

```bash
if command -v apt >/dev/null 2>&1; then
    PM="apt"
    UPDATE_CMD="sudo apt update"
    INSTALL_CMD="sudo apt install -y"
    DOCKER_PKG="docker.io"
    PYTHON_PIP_PKG="python3-pip"
elif command -v dnf >/dev/null 2>&1; then
    # Similar settings for dnf...
# Additional detection for yum, pacman, and zypper
else
    echo "No supported package manager found on your system."
    exit 1
fi
```

Explanation:

- This section checks for commonly used package managers (apt, dnf, yum, pacman, or zypper).
- It sets variables like `UPDATE_CMD` and `INSTALL_CMD` so that the same installation functions can be used on any distro.
- If none of the known package managers are found, the script exits with an error message.

### 3. Example: Installing zsh and oh‑my‑zsh

```bash
install_zsh() {
    if ! command -v zsh >/dev/null 2>&1; then
        echo "Installing zsh..."
        $INSTALL_CMD zsh
    else
        echo "zsh is already installed."
    fi
}

install_oh_my_zsh() {
    if [ ! -d "$HOME/.oh-my-zsh" ]; then
        echo "Installing oh-my-zsh (unattended)..."
        sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
        # Install plugins for syntax highlighting and autosuggestions...
    else
        echo "oh-my-zsh is already installed."
    fi
}
```

Explanation:

- `install_zsh()` checks if zsh exists; if not, it uses the appropriate package manager command to install it.
- `install_oh_my_zsh()` installs oh‑my‑zsh in unattended mode and optionally clones plugin repositories for additional features.

These functions are part of a larger set that installs all terminal enhancements and development tools.

### 4. User Menu for Installation Options

```bash
echo "Linux Development Tools Installer"
echo "Choose installation option:"
echo "1) Install All"
echo "2) Customize Installation (Choose Categories)"
read -rp "Enter choice [1-2]: " install_choice

case $install_choice in
    1)
        install_terminal_tools
        install_dev_environment
        ;;
    2)
        echo "Select categories to install (separated by space):"
        echo "1) Terminal Enhancements (zsh, oh-my-zsh, starship, fzf, zoxide)"
        echo "2) Development Environment (nvm, node, python, pip, docker, git, vscode, pycharm)"
        read -rp "Enter choices (e.g. 1 2): " -a categories
        # Loop through user-selected categories...
        ;;
    *)
        echo "Invalid choice. Exiting."
        exit 1
        ;;
esac
```

Explanation:

- This section provides a simple interactive menu so you can either install everything at once or select specific categories.
- The use of arrays and a loop lets the script process multiple category choices in one go.

## macOS and Windows Installers – A Quick Look

### macOS Installer:

- Located in `macos/install_macos.sh`
- Uses Homebrew for installation (e.g., `brew install zsh`, `brew install --cask visual-studio-code`)
- No explicit sudo calls in the script, thanks to Homebrew's user-level installation.

### Windows Installer:

- The Bash script in `windows/install_windows.sh` leverages winget to install applications.
- A wrapper batch file (`windows/run_install_windows.bat`) automatically requests administrative privileges using UAC, ensuring the installer runs with the necessary rights.

## Conclusion

The `fernand3z/dev-tools` repository is a powerful starting point for anyone who wants to automate the setup of a development environment across multiple platforms. By breaking down the script into modular sections—such as sudo management, package manager detection, installation functions, and interactive menus—you can see exactly how each part works and even adapt it for your own needs.

Whether you're looking to use this script directly or build your own version from scratch, these code sections and explanations should help you understand the process and make modifications as needed.

Happy coding, and feel free to contribute or ask questions on GitHub! 