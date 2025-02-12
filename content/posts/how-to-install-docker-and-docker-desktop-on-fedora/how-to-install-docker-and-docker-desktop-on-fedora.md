---
title: "How to Install Docker and Docker Desktop on Fedora"
date: "2025-02-07"
author: "fernand3z"
description: "Step-by-step guide to installing Docker and Docker Desktop on Fedora."
tags:
  [
    "docker",
    "fedora",
    "linux",
    "containers",
    "installation",
    "docker-desktop",
    "devops",
  ]
categories: ["Development Tools", "Tutorials", "DevOps"]
cover:
  image: "/images/posts/docker-install/docker-image.jpg"
  alt: "Docker Logo"
  caption: "Docker - Container Platform"
  relative: false
  hidden: false
  responsiveImages: true
  linkFullImages: true
---

Docker is a popular containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers. In this guide, we'll walk through the installation of both Docker and Docker Desktop on Fedora.

## Prerequisites

- Fedora 35 or later
- `sudo` privileges
- Internet connectivity

## Step 1: Update Your System

Before installing Docker, ensure your system is up to date:

```sh
sudo dnf update -y
```

## Step 2: Add the Docker Repository

Fedora does not include Docker in its default repositories, so you need to add the official Docker repository:

```sh
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

## Step 3: Install Docker

Once the repository is added, install Docker with:

```sh
sudo dnf install -y docker-ce docker-ce-cli containerd.io
```

## Step 4: Enable and Start Docker

To start Docker and enable it to launch at boot:

```sh
sudo systemctl enable --now docker
```

## Step 5: Verify Installation

Check if Docker is running correctly:

```sh
sudo systemctl status docker
```

You can also verify by running a test container:

```sh
sudo docker run hello-world
```

If you see a message confirming that Docker is working, the installation was successful.

## Step 6: Add Your User to the Docker Group (Optional)

To run Docker without `sudo`, add your user to the `docker` group:

```sh
sudo usermod -aG docker $USER
```

Log out and log back in, or run:

```sh
newgrp docker
```

Test if it works without `sudo`:

```sh
docker run hello-world
```

## Step 7: Enable Docker on Boot (Optional)

If you want Docker to start automatically on system boot:

```sh
sudo systemctl enable docker
```

# How to Install Docker Desktop on Fedora

Docker Desktop is a GUI tool for managing containers and images. Since Fedora does not officially support Docker Desktop, we will use an alternative approach.

## Step 1: Install Dependencies

Ensure `curl`, `dnf-plugins-core`, and `wget` are installed:

```sh
sudo dnf install -y curl dnf-plugins-core wget
```

## Step 2: Download Docker Desktop RPM Package

Download the latest Docker Desktop package from the official website:

```sh
wget https://desktop.docker.com/linux/main/amd64/docker-desktop-latest.rpm
```

## Step 3: Install Docker Desktop

Use `dnf` to install the downloaded package:

```sh
sudo dnf install -y ./docker-desktop-latest.rpm
```

## Step 4: Start Docker Desktop

To start Docker Desktop, run:

```sh
docker-desktop
```

If you encounter issues with Wayland, try launching it with:

```sh
QT_QPA_PLATFORM=xcb docker-desktop
```

## Step 5: Enable Docker Desktop on Startup (Optional)

To automatically start Docker Desktop on login:

```sh
mkdir -p ~/.config/autostart
cp /usr/share/applications/docker-desktop.desktop ~/.config/autostart/
```

## Step 6: Verify Docker Desktop Installation

Ensure Docker Desktop is running:

```sh
docker info
```

If you see the Docker version and system information, the installation was successful.

## Uninstall Docker and Docker Desktop (If Needed)

If you ever need to remove Docker and Docker Desktop, use:

```sh
sudo dnf remove -y docker-ce docker-ce-cli containerd.io docker-desktop
sudo rm -rf /var/lib/docker /var/lib/containerd
```

## Conclusion

You have successfully installed both Docker and Docker Desktop on Fedora! You can now start using containers for development, testing, and deployment.

For more details, visit the [official Docker documentation](https://docs.docker.com/engine/install/fedora/).
