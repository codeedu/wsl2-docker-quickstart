<p align="center">
  <a href="https://fullcycle.com.br/" target="blank"><img src="https://fullcycle.com.br/wp-content/themes/fullcycle/assets/images/fullcycle-logo.svg"/></a>
</p>

# WSL2 + Docker Quick Guide

*Read this in other language: [Portuguese](README.md)

## Summary

<details open>
  <summary>
    <strong>WSL</strong>
  </summary>

  * [What is WSL2](#what-is-wsl2)
  * [Minimum Requirements](#minimum-requirements)
  * [Installation of WSL 2](#installation-of-wsl-2)
    * [Windows Update](#windows-update)
    * [Update WSL](#update-wsl)
    * [Set the default version of WSL to version 2](#set-the-default-version-of-wsl-to-version-2)
    * [Install Ubuntu](#install-ubuntu)
    * [(Optional) Change the version of a Linux distribution from WSL 1 to WSL 2](#optional-change-the-version-of-a-linux-distribution-from-wsl-1-to-wsl-2)
    * [Installation of WSL 2 via Windows Store](#installation-of-wsl-2-via-windows-store)
    * [Integration with VSCode](#integration-with-vscode)
  * [Windows Terminal as the default development terminal for Windows](#windows-terminal-as-the-default-development-terminal-for-windows)
  * [What WSL 2 Can Use from Your Machine's Resources](#what-wsl-2-can-use-from-your-machines-resources)
  * [Systemd](#systemd)
</details>

<details open>
  <summary>
    <strong>WSLg</strong>
  </summary>

  * [What is WSLg](#what-is-wslg)
  * [WSLg Architecture](#wslg-architecture)
  * [How to Activate WSLg](#how-to-activate-wslg)
</details>

<details open>
  <summary>
    <strong>Docker</strong>
  </summary>

  * [What is Docker](#what-is-docker)
  * [Why Use WSL 2 + Docker for Development](#why-use-wsl-2--docker-for-development)
  * [Ways to Use Docker on Windows](#ways-to-use-docker-on-windows)
    * [1. (Obsolete) Docker Toolbox](#1-obsolete-docker-toolbox)
    * [2. (Obsolete) Docker Desktop with Hyper-V](#2-obsolete-docker-desktop-with-hyper-v)
    * [3. Docker Desktop with WSL2](#3-docker-desktop-with-wsl2)
      * [Advantages](#docker-desktop-advantages)
      * [Disadvantages](#docker-desktop-disadvantages)
    * [4. Docker Engine (Native Docker) Directly Installed on WSL2](#4-docker-engine-native-docker-directly-installed-on-wsl2)
      * [Advantages](#docker-engine-advantages)
      * [Disadvantages](#docker-engine-disvantages)
  * [Which Docker Usage Mode to Choose on Windows?](#which-docker-usage-mode-to-choose-on-windows)
  * [Installing Docker](#installing-docker)
    * [1 - Install Docker with Docker Desktop](#1---install-docker-with-docker-desktop)
      * [Activate Docker in the Linux Distribution](#activate-docker-in-the-linux-distribution)
      * [Optimize Docker Desktop Resources](#optimize-docker-desktop-resources)
      * [Apply autoMemoryReclaim in WSL 2](#apply-automemoryreclaim-in-wsl-2)
    * [2 - Install Docker with Docker Engine (Docker Native)](#install-docker-using-docker-engine)
      * [Error Starting Docker on Ubuntu 22.04](#error-starting-docker-on-ubuntu-2204)
      * [Automatically Start Docker in WSL](#automatically-start-docker-in-wsl)
      * [Docker with Systemd](#docker-with-systemd)  
  
</details>

<details open>
  <summary>
    <strong>Basic Tips and Tricks with WSL 2</strong>
  </summary>

  * [Basic Tips and Tricks with WSL 2](#basic-tips-and-tricks-with-wsl-2)
    * [Performance When Using WSL 2](#performance-when-using-wsl-2)
    * [Windows Terminal](#windows-terminal)
    * [Access Windows Disks and Other Devices](#access-windows-disks-and-other-devices)
    * [Access WSL 2 from Windows](#access-wsl-2-from-windows)
    * [Integration with VSCode](#integration-with-vscode-1)
    * [Running Windows Executables in WSL 2](#running-windows-executables-in-wsl-2)
    * [Listing Installed Linux Distributions in WSL 2](#listing-installed-linux-distributions-in-wsl-2)
    * [Stopping All Linux Distributions in WSL 2](#stopping-all-linux-distributions-in-wsl-2)
    * [Backup and Restore Linux Distributions in WSL 2](#backup-and-restore-linux-distributions-in-wsl-2)
    * [Compressing the WSL 2 Virtual Disk](#compressing-the-wsl-2-virtual-disk)
    * [LAN and VPN Network Mode](#lan-and-vpn-network-mode)
    * [Enabling BuildKit in Docker](#enabling-buildkit-in-docker)
    * [Automatically Start Docker in WSL 2](#automatically-start-docker-in-wsl-2)
    * [Releasing RAM Memory from WSL 2](#releasing-ram-memory-from-wsl-2)
    * [.wslconfig and wsl.conf Files](#wslconfig-and-wslconf-files)

  
</details>

<details open>
  <summary>FAQ</summary>

  * [FAQs](#faqs)
    * [Do I need a Windows 10/11 Pro license to use WSL 2?](#do-i-need-a-windows-1011-pro-license-to-use-wsl-2)
    * [Can I continue developing on Windows without using WSL 2?](#can-i-continue-developing-on-windows-without-using-wsl-2)
    * [Does WSL 2 work alongside other virtual machines like VirtualBox or VMWare?](#does-wsl-2-work-alongside-other-virtual-machines-like-virtualbox-or-vmware)
    * [Can I access applications running in WSL 2 from Windows?](#can-i-access-applications-running-in-wsl-2-from-windows)
    * [Can I run graphical applications in WSL 2?](#can-i-run-graphical-applications-in-wsl-2)
    * [Can I use WSL in production scenarios?](#can-i-use-wsl-in-production-scenarios)
    * [Can I run Docker Engine alongside Docker Desktop?](#can-i-run-docker-engine-alongside-docker-desktop)

    
</details>

## What is WSL2

In 2016, Microsoft announced the possibility of running Linux inside Windows 10 as a subsystem, and this was named **WSL** or **Windows Subsystem for Linux**.

Accessing the file system in Windows 10 from Linux was simple and fast, but we did not have a complete execution of the Linux kernel, among other native artifacts, which made it impossible to perform several tasks on Linux, one of them being Docker.

In 2019, Microsoft announced **WSL 2**, with an improved dynamic compared to the 1st version:

* Execution of the complete Linux kernel.
* Better performance for accessing files within Linux.
* Full system call compatibility.

WSL 2 was officially released on **May 28, 2020**.

With WSL 2, it is possible to run Docker and other tools that depend on the Linux Kernel using Windows 10/11.

Compare WSL versions: [https://docs.microsoft.com/en-us/windows/wsl/compare-versions](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)

## Minimum Requirements

* **Windows 10 Home or Professional**
  - Version 2004 or higher (Build 19041 or higher).
  - Older versions require manual installation of WSL 2. See tutorial [https://learn.microsoft.com/en-us/windows/wsl/install-manual](https://learn.microsoft.com/en-us/windows/wsl/install-manual).

* **Windows 11 Home or Professional**
  - Version 22000 or higher (any Windows 11).

* A machine compatible with virtualization (check availability according to your processor brand. If your machine is older, it may be necessary to enable it in the BIOS).

* At least 4GB of RAM (8GB recommended).

Your Windows is probably already on the supported version, but check this by going to `All Settings > System > About`. If it is not, use the Windows Update Assistant to update your Windows version.

> **It is essential to keep Windows updated, as WSL 2 depends on an updated version of Hyper-V. Check Windows Update.**

## Installation of WSL 2

All instructions below are for Windows 10/11.

### Windows Update

Make sure your Windows is updated, as WSL 2 depends on an updated version of Hyper-V. Check Windows Update.

### Update WSL

With Windows 10 version 2004 or Windows 11, WSL will already be present on your machine. Run the command to get the latest version of WSL:

```bash
wsl --update
```

### Set the default version of WSL to version 2

Version 2 is usually the default, but version 1 of WSL might be the default. Run the command below to set version 2 as the default:

```bash
wsl --set-default-version 2
```

### Install Ubuntu

Run the command:

```bash
wsl --install
```

This command will install `Ubuntu` as the default Linux distribution.

If you want to install a different version of Ubuntu, run the command `wsl -l -o`. It will list all available Linux distributions. Install the chosen version with the command `wsl --install -d distribution-name`.

We suggest Ubuntu (without a version) because it is a popular distribution that comes with many useful development tools installed by default.

After the command completes, you will need to create a **username** (use a username without spaces or special characters) and a **password** (set a strong password). This password will be used to install packages and perform superuser operations.

To open a new Ubuntu window, just type `Ubuntu` in the start menu and click on the Ubuntu icon.

We recommend using the [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started) as the default terminal for development on Windows. It will aggregate the Ubuntu shell, as well as PowerShell and CMD, into a single window, and allow for color and theme customization.

### (Optional) Change the version of a Linux distribution from WSL 1 to WSL 2

If the Linux distribution you installed is on version 1, you can change it to version 2 with the following command:

```bash
wsl --set-version <distribution name> 2
```

Congratulations, your WSL2 is now up and running!

![Example of WSL2 running](img/wsl2-working.png)

### Installation of WSL 2 via Windows Store

It is also possible to install Linux distributions from the Windows Store. Just access the Windows Store, search for the desired Linux distribution name, and click install.

We suggest Ubuntu (without a version) because it is a popular distribution that comes with many useful development tools installed by default.

![Linux distributions in the Windows Store](img/linux-distros.png)

### Integration with VSCode

Visual Studio Code has an extension called **Remote - WSL** that allows accessing WSL 2 directly from VSCode. With this extension, you can edit your files directly in WSL 2, run commands, install extensions, and much more.

Learn more about this extension at [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl).

When opening a project located inside Linux, make sure that the WSL mode in VSCode is activated. In the bottom left corner of VSCode, click on the `><` button and select `Connect to WSL`. This will connect VSCode to WSL 2, allowing you to open the project inside Linux. You will see that the `><` button will change to `WSL: Ubuntu`.

![Connect to WSL 2 in VSCode](img/vscode-remote-wsl.png)

If VSCode is not set this way when opening projects inside Linux, WSL mode will not be activated, and you will lose performance when editing files inside Linux, as well as support for some extensions.

Another way to open the project in WSL mode is by accessing the project folder in the WSL terminal and typing `code .`. This will open VSCode in WSL mode.

## Windows Terminal as the default development terminal for Windows

A deficiency that Windows has always had was providing an adequate terminal for development. Now we have the **Windows Terminal** built by Microsoft itself, which allows running terminals in tabs, changing colors and themes, configuring shortcuts, and much more.

Install it from the Windows Store.

See more configuration options at [More about Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started).

By default, Windows Terminal will identify the main shells installed on Windows, such as PowerShell, CMD, and WSL.

## What WSL 2 Can Use from Your Machine's Resources

We can say that WSL 2 has almost complete access to your machine's resources. By default, it has access to:

* The entire hard drive.
* Full use of processing resources.
* 50% of the available RAM.
* 25% of the available SWAP (virtual memory).

If you want to customize these limits, create a file named `.wslconfig` at the root of your user folder `(C:\Users\<your_username>)` and define these settings:

```conf
[wsl2]
memory=8GB
processors=4
```

These are example limits and the most basic configurations to be used. Adjust them according to your available resources.

For more details, see Microsoft's documentation: [Configuration Settings for WSL](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configuration-setting-for-wslconfig). There are other configurations that can be made, such as network settings, VPN, memory release, etc.

To apply these settings, you need to restart the Linux distributions. Run the command: `wsl --shutdown` (This command will shut down all active WSL 2 instances, just open the terminal again to use them with the new settings).

This `.wslconfig` file is a global configuration file, meaning it will affect all Linux distributions you have installed in WSL 2, as you can have more than one Linux distribution installed in WSL 2, such as Ubuntu, Debian, Fedora, etc.

## Systemd

WSL is compatible with `systemd`. `Systemd` is an initialization and service management system widely used in modern Linux distributions. It allows you to use more complex tools on Linux like snapd, LXD, etc.

It is not mandatory to activate it, and it can be deactivated and reactivated at any time. However, we recommend keeping it activated because it will improve compatibility with Linux distributions, allowing you to use more tools and services like Kubernetes, etc. (It is not necessary for running Docker).

To activate it, edit the `/etc/wsl.conf` file:

Run the command to edit:

```bash
sudo vim /etc/wsl.conf
```

Press the `i` key (to enter insert mode) and paste the following content:

```conf
[boot]
systemd = true
```

When you finish editing, press `Esc`, then type `:` to enter the command `wq` (write and quit) and press `enter`.

Every time this change is made, it is necessary to restart WSL with the command `wsl --shutdown` in DOS or PowerShell.

## What is WSLg

WSLg is an extension of WSL 2 that allows running Linux graphical applications on Windows. It is an extension of WSL 2, and no additional installation is required, just have WSL 2 installed and updated.

With it, you can run applications such as Chrome, Firefox, Gedit, IDEs (VSCode, JetBrains), and even graphical applications made in Java, Python, etc.

### WSLg Architecture

WSLg is composed of the following components: Wayland, Weston, PulseAudio, and CBL-Mariner.

Basically, Wayland serves as the display server, Weston as the compositor, PulseAudio for audio, and CBL-Mariner as the Linux distribution to run graphical applications.

![WSLg Architecture](img/WSLg_ArchitectureOverview.png)

### How to Activate WSLg

To activate WSLg, just have WSL 2 installed and updated. No additional installation is necessary.

When you install an application that depends on a graphical interface, WSLg will be activated automatically. Let's look at an example:

```bash
sudo apt-get update

sudo apt-get install gedit
```

Open Gedit in the WSL 2 terminal by typing `gedit`, and it will open on Windows.

Therefore, just install the application and launch it in the WSL 2 terminal to open it on Windows.

## What is Docker

Docker is an open-source platform that enables the packaging of an application within a container. An application can adapt and run on any machine that has this technology installed.

### Why Use WSL 2 + Docker for Development

Setting up development environments on Windows has always been bureaucratic and complex, in addition to the performance of some tools not being entirely satisfactory.

With the advent of Docker, this scenario has greatly improved, as we can set up our development environment based on Unix, independently and quickly, while still being unified with other operating systems.

Check out our **live session on WSL 2 + Docker on the Full Cycle channel**: [https://www.youtube.com/watch?v=O33trWxqVC4](https://www.youtube.com/watch?v=O33trWxqVC4).

## Ways to Use Docker on Windows

* 1. *Obsolete* [Docker Toolbox](#obsoleto-docker-toolbox)
* 2. *Obsolete* [Docker Desktop with Hyper-V](#obsoleto-docker-desktop-com-hyper-v)
* 3. [Docker Desktop with WSL2](#docker-desktop-com-wsl2)
* 4. [Docker Engine (Native Docker) Directly Installed on WSL2](#docker-engine-docker-nativo-diretamente-instalado-no-wsl2)

### 1. (Obsolete) Docker Toolbox

Runs on Oracle's virtualization program, called **VirtualBox**.
The performance of Docker Toolbox can be very poor, making its use unfeasible.

It can still be used on older Windows versions, such as XP, Vista, 7, 8, and 8.1.

### 2. (Obsolete) Docker Desktop with Hyper-V

Runs on Microsoft's **Hyper-V** instead of using VirtualBox used by Docker Toolbox. Docker Desktop with Hyper-V requires the **PRO** version of Windows 10/11, so it is necessary to purchase it if you do not have it.

Hyper-V tends to require many machine resources and, although the performance is better than Docker Toolbox, the machine may become slow for other tasks on Windows.

### 3. Docker Desktop with WSL2

Runs on **Virtual Machine Platform**, a component of Hyper-V, and integrates with WSL2, allowing Docker to run within the Linux environment.

No need to purchase a PRO license for Windows 10/11.

It has excellent performance and consumes fewer resources compared to Docker Toolbox or Docker Desktop with Hyper-V.

#### <a id="docker-desktop-advantages"></a> Advantages

* Has a visual tool to manage Docker.
* Allows installing extensions to use direct tools in Docker.
* Enables running a local Kubernetes cluster easily.
* Simplifies Docker configuration both on Windows and WSL 2.
* Allows running Docker outside of WSL 2, making it possible to use any shell like PowerShell or DOS.
* Supports Windows mode containers (Images that contain Windows under the hood instead of Linux).
* By running another separate Linux instance, it provides a more isolated development environment and increased security.
* Creates a centralized environment for storing images, volumes, and other Docker configurations. Multiple WSL 2 distributions can run the same Docker instance.

#### <a id="docker-desktop-disadvantages"></a> Disadvantages

* Initial memory usage without running any Docker container is 1GB or more.
* It may be necessary to restart Docker Desktop to function correctly in some situations.
* Tends to consume more machine resources than Docker Engine directly installed on WSL 2.

### 4. Docker Engine (Native Docker) Directly Installed on WSL2

Docker Engine is the native Docker (as it was created) that runs in the Linux environment and is fully supported for WSL 2. Its installation is identical to that described for the Linux distributions available on the [Docker site](https://docs.docker.com/engine/install/ubuntu/).

It is the purest way to use Docker on Linux.

#### <a id="docker-engine-advantages"></a> Advantages

* Consumes less memory to run the Docker Daemon (Docker server) compared to Docker Desktop.
* Provides the purest experience of using Docker on Linux.

#### <a id="docker-engine-disvantages"></a> Disadvantages

* If you need to run Docker in another WSL 2 instance, you need to install Docker again in that instance or configure access to the desired Docker socket to share Docker between instances.
* Does not support Windows mode containers.
* Currently, the size of the VHDX (virtual disk where Linux is mounted) does not automatically shrink using the sparse mode of WSL (automatic compression mode) when Docker artifacts (containers, images, volumes, layers) are deleted. Depending on the size of the virtual disk, it may be necessary to compact it manually from time to time.
* It may pose more security risks to Linux because Docker Engine is software that runs directly on Linux, and running third-party images may pose security risks.

## Which Docker Usage Mode to Choose on Windows?

The first two options are obsolete and not recommended. The last two options are the most recommended.

**Docker Desktop**, as explained in the [Docker Desktop with WSL2](#3-docker-desktop-com-wsl2) section, is recommended for most users because it is easier to use and has a graphical interface for managing Docker. However, it consumes more machine resources, particularly RAM. With 8GB of RAM, Docker Desktop may not be the best option. Having 16GB of RAM might be ideal for running Docker Desktop on your machine.

You can try Docker Desktop, and if you find it consuming too many resources, you can uninstall it and install Docker Engine directly on WSL 2 instead. Docker Desktop has a RAM and CPU usage indicator, so monitor it over a few days to see if it consumes too many resources. Below, we will show how to optimize the resources consumed by Docker Desktop using Resource Save Mode.

## Installing Docker

### 1 - Install Docker with Docker Desktop

Download it from this link: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) and install Docker Desktop.

Immediately after installation, you'll be prompted to log in with your Docker account. Log in (create an account if you don't have one) and follow the instructions.

Once the installation is complete, Docker Desktop will be installed and running. You can see the Docker icon near the Windows clock. It will run in the background. The Docker Desktop interface should look like this:

![Docker Desktop Installed](img/installing-docker-desktop.png)

You can now see that there are two Linux distributions running on WSL 2: one is the default Ubuntu distribution (or the one you installed), and the other is the Docker Desktop distribution. Run the command `wsl -l -v` to view the installed Linux distributions and their current status.

![Linux Distributions Running on WSL 2](img/wsl-docker-desktop-running.png)

#### Activate Docker in the Linux Distribution

To make Docker work in your Linux distribution, you need to enable it in the Docker Desktop panel. Open the Docker Desktop interface, click on the gear icon in the upper right corner, go to `Resources -> WSL Integration`, enable the Linux distribution you want to use Docker with, and click `Apply & Restart`, as shown in the image below:

![Activate Docker in the Linux Distribution](img/docker-desktop-wsl-integration.png)

#### Optimize Docker Desktop Resources

Docker Desktop has a feature called **Resource Save Mode** that optimizes machine resource usage. It reduces RAM and CPU usage when Docker Desktop is not in use.

Occasionally, Docker Desktop will check for running containers, and if none are found, it will reduce resource usage.

Enable it by clicking on the gear icon in the upper right corner, going to `Resources -> Advanced`, and enabling the `Resource Save Mode` option, as shown in the image below:

![Enable Resource Save Mode in Docker Desktop](img/resource-saver.png)

You can choose how often Docker Desktop will check for running containers and reduce resource usage. The default is 5 minutes.

#### Apply autoMemoryReclaim in WSL 2

Over time, WSL may consume RAM and not release it; memory caching is done to improve performance, but this memory can be freed after a while. This option is called `autoMemoryReclaim`, which frees up unused RAM through one of two options:

* **gradual:** Frees RAM gradually every 5 minutes.
* **dropcache:** Frees RAM immediately.

To activate `autoMemoryReclaim`, edit the `.wslconfig` file:

```conf
[experimental]
autoMemoryReclaim=gradual
```

This option will only work after restarting WSL with the command `wsl --shutdown`.

> Do not use this option if you are using Docker Engine, as it will cause issues with Docker Engine. Use it only if you are using Docker Desktop or if Docker is not installed.

### <a id="install-docker-using-docker-engine"></a>2 - Install Docker with Docker Engine (Docker Native)

Installing Docker on WSL 2 is similar to installing Docker on any Linux distribution. For instance, if you have Ubuntu, the process is the same as for Ubuntu; if it's Fedora, it's the same as for Fedora. The Docker installation documentation for different Linux distributions is [here](https://docs.docker.com/engine/install/), but let’s see how to install it on Ubuntu.

> **For those migrating from Docker Desktop to Docker Engine, there are two options:**
> 1. Uninstall Docker Desktop.
> 2. Disable Docker Desktop Service in Windows services. This option allows you to use Docker Desktop if necessary, but for most users, uninstalling Docker Desktop is recommended.
> If you chose the 2nd option, you will need to delete the `~/.docker/config.json` file and authenticate with Docker again using the `docker login` command.

> **If you need to integrate Docker with other IDEs besides VSCode:**
> 
> VSCode integrates with Docker on WSL through the Remote WSL or Remote Container extension.
> 
> To enable TCP socket connection to the Docker server, follow these steps:
> 1. Create the file `/etc/docker/daemon.json` with the command:
>    ```bash
>    sudo echo '{"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}' > /etc/docker/daemon.json
>    ```
> 2. Restart Docker:
>    ```bash
>    sudo service docker restart
>    ```
> 
> After this, go to your IDE and select TCP Socket to connect to Docker, entering the URL `http://IP-OF-WSL:2375`. You can find your WSL IP with the command `cat /etc/resolv.conf`.
> 
> If it does not work, restart WSL with the command `wsl --shutdown` and start Docker service again.

Execute the following commands to install Docker:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Add Docker’s repository to Ubuntu’s sources list:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

> **NOTE:** If you are using a distribution other than Ubuntu, check the Docker documentation for installation commands: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

Grant permissions to run Docker with your current user:

```bash
sudo usermod -aG docker $USER
```

Restart WSL from the Windows command line to avoid needing root permissions to run Docker commands:

```bash
wsl --shutdown
```

Re-access Ubuntu and start the Docker service:

```bash
sudo service docker start
```

This command will need to be executed every time Linux is restarted. If Docker is not running, you will see the following error when running Docker commands:

```bash
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

#### Error Starting Docker on Ubuntu 22.04

> If you encounter the following error or a similar one even after starting the Docker service:
>
> `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`
> Run the command `sudo update-alternatives --config iptables` and select option 1 `iptables-legacy`
>
> Then run `sudo service docker start` again. Run a Docker command like `docker ps` to check if it’s working properly. If the error no longer appears, Docker is running correctly.

#### Automatically Start Docker in WSL

> **For Windows 11 or Windows 10 with update KB5020030 only**

You can specify a default command to run each time WSL starts, which allows Docker to start automatically. Edit the file `/etc/wsl.conf`:

Run the command to edit:

```bash
sudo vim /etc/wsl.conf
```

Press `i` to enter insert mode and paste the following content:

```conf
[boot]
command = service docker start
```

#### Docker with Systemd

If you have enabled systemd, Docker will usually start automatically. Therefore, if you have the line `command = service docker start` in `/etc/wsl.conf`, comment it out with `#` and restart WSL with the command `wsl --shutdown`.

Otherwise, you can start Docker automatically using these commands:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

You need to restart WSL with `wsl --shutdown` for the changes to take effect.

Once you’ve done this, restart WSL with `wsl --shutdown` from DOS or PowerShell to test. After reopening WSL, type `docker ps` to check if the command no longer returns the message: `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`

## Basic Tips and Tricks with WSL 2

### Performance When Using WSL 2

The performance of WSL 2 is optimal when running everything inside Linux, so avoid running your projects with or without Docker from the `/mnt/c` path, as it will result in reduced performance. Ideally, run everything within Linux, in the `/home/your_user` path.

The idea is to move all your projects from `C:` to Linux, specifically `/home/your_user`. This way, everything is within Linux and performance will be better.

It might seem strange to do everything within Linux at first, but it’s the best way to achieve optimal performance with WSL 2.

### Windows Terminal

Windows Terminal is a terminal application that combines multiple terminals into one window, such as PowerShell, CMD, and WSL. It offers customization for colors, themes, shortcuts, and more.

Install it from the Windows Store.

Using Windows Terminal is a much better experience than the default Windows terminal. Use it for development on Windows and to access WSL 2.

### Access Windows Disks and Other Devices

WSL 2 has access to the entire Windows hard drive. You can access the C: drive from `/mnt/c`. If you have other drives, they will be available at `/mnt/d`, `/mnt/e`, etc.

The `/mnt` directory is a mount point in Linux where Windows devices are mounted.

![Mount no WSL2](img/mount-in-wsl2.png)

### Access WSL 2 from Windows

You can access WSL 2 through Windows Explorer by typing `\\wsl.localhost$` in the address bar to view the installed Linux distributions.

You can also access it through the `Linux` icon on the left side of Windows Explorer. If your Windows version doesn’t have this icon, use the path `\\wsl.localhost$`.

![Accessing WSL 2 from Windows Explorer](img/acessing-wsl-win-explorer.png)

You can also open a folder in Windows Explorer from within Linux by typing `explorer.exe .` in the WSL terminal. This will open the current folder in Windows Explorer.

### Integration with VSCode

Visual Studio Code has an extension called **Remote - WSL** that allows you to access WSL 2 directly from VSCode. With this extension, you can edit your files directly in WSL 2, run commands, install extensions, and more.

Learn more about this extension at [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl).

When opening a project that is inside Linux, make sure the WSL mode in VSCode is enabled. In the lower-left corner of VSCode, click the `><` button and select `Connect to WSL`. This will connect VSCode to WSL 2, allowing you to open projects inside Linux. You’ll see the button `><` change to `WSL: Ubuntu`.

![Connecting WSL 2 in VSCode](img/vscode-remote-wsl.png)

If VSCode doesn’t open projects in WSL mode, the WSL mode isn’t activated, and you will lose performance while editing files in Linux and may miss out on some extensions.

Another way to open a project in WSL mode is by accessing the project folder in the WSL terminal and typing `code .`. This will open VSCode in WSL mode.

### Running Windows Executables in WSL 2

You can run Windows executables within WSL 2 by typing the full path of the Windows executable in the WSL terminal. For example, to run `notepad.exe`, just type `notepad.exe` in the WSL terminal.

![Running Windows executables in WSL 2](img/executables-win-in-wsl2.png)

### Listing Installed Linux Distributions in WSL 2

To list the Linux distributions installed in WSL 2, execute the command `wsl -l -v` in the Windows terminal.

### Stopping All Linux Distributions in WSL 2

To stop all running Linux distributions in WSL 2, execute the command `wsl --shutdown` in the Windows terminal.

To stop a specific Linux distribution, use the command `wsl --t <distribution name>`.

![Checking installed distros in WSL 2](img/check-installed-distros.png)

> If you are using Docker Desktop, executing `wsl --shutdown` will also stop Docker Desktop, and you will need to restart it.

* You can run some graphical applications in Linux with WSL 2. Check this tutorial: [https://medium.com/@dianaarnos/aplica%C3%A7%C3%B5es-gr%C3%A1ficas-no-wsl2-e0a481e9768c](https://medium.com/@dianaarnos/aplica%C3%A7%C3%B5es-gr%C3%A1ficas-no-wsl2-e0a481e9768c).
* Run the command ```wsl -l -v``` with PowerShell to see the Linux versions installed and their current status (running or stopped).

### Backup and Restore Linux Distributions in WSL 2

You can back up and restore Linux distributions in WSL 2. This is useful as it saves everything you’ve done in Linux, such as package installations and configurations, and allows you to restore it wherever you want.

To back up a Linux distribution, execute:

```bash
wsl --export Ubuntu C:\path\backup.vhdx --vhd
```

Where `Ubuntu` is the name of the Linux distribution you want to back up and `C:\path\backup.vhdx` is the path where the backup will be saved. The `--vhd` option saves the backup in VHD format, which facilitates restoration and can be mounted as a hard disk in Windows if desired.

Depending on the size of the virtual disk, the backup process may take some time.

To restore a Linux distribution, execute:

```bash
wsl --import-in-place Ubuntu C:\path\backup.vhdx
```

Where `Ubuntu` is the name of the Linux distribution (or any name you prefer) you want to restore and `C:\path\backup.vhdx` is the path where the backup was saved.

Typically, VHDX files are saved in `C:\Users\<your_user>\AppData\Local\Packages\CanonicalGroupLimited...\LocalState\`, but once you have restored the Linux distribution, you can move the VHDX file wherever you like. It will be the path you provide in the `wsl --import-in-place` command.

### Compressing the WSL 2 Virtual Disk

The WSL 2 virtual disk is a VHDX file that grows as you use Linux. If you delete files, the virtual disk does not automatically shrink; it will remain the same size.

To reduce its size, enable the `sparse` mode in WSL 2, which provides automatic disk compression. To enable it, edit the `.wslconfig` file:

```conf
[wsl2]
sparseVhd=true
```

This option will only apply to new virtual disks.

For existing virtual disks, use the following commands:

```bash
wsl --manage "Ubuntu" --set-sparse true
wsl --manage "docker-desktop" --set-sparse true # If using Docker Desktop
```

This will convert the virtual disk to sparse mode, which is automatic disk compression.

Before running the command, make sure the Linux distribution is stopped by using `wsl --shutdown` to stop all distributions.

If you have an old distribution and after applying sparse mode the virtual disk hasn't reduced in size, you can compress it manually.

Use the [wslcompact](https://github.com/okibcn/wslcompact) tool for this process. This tool is an executable for `PowerShell` that exports and imports the virtual disk, forcing WSL to compress it.

Run the command:

```bash
wslcompact -c Ubuntu
```

This will generate a new virtual disk named `ext4.vhdx`, which will be your compressed distribution. When the executable asks to register the disk, press `Y` and then `Enter`. This won't automatically register it; you'll need to do it manually as shown in the backup and restoration section of this tutorial.

Remove the old disk with:

```bash
wsl --unregister Ubuntu
```

And register the new disk with:

```bash
wsl --import Ubuntu C:\path\to\ext4.vhdx
```

> **Warning**: Backup your Linux distribution before compressing it to avoid losing data in case of errors.

### LAN and VPN Network Mode

Since WSL is virtualized, there is a separate network interface, so if you use LAN (wired), you may not be able to access applications running in WSL through a browser, for instance, without port binding, which can be inefficient.

You might also encounter issues with VPNs as WSL 2 has its own network interface, which might not work correctly with some VPNs.

To resolve this, enable the `mirrored` network mode in WSL 2, which makes the WSL 2 network interface the same as Windows. Edit the `.wslconfig` file:

```conf
[wsl2]
network=mirrored
```

This option will only work after restarting WSL with the command `wsl --shutdown`.

This option helps improve the experience when using VPNs.

### Enabling BuildKit in Docker

> **For Docker Engine only**

Add `export DOCKER_BUILDKIT=1` at the end of your Linux user’s `.profile` file or in your custom terminal file to enhance performance when building with Docker. Run `source ~/.profile` to load this environment variable into your WSL 2 environment.

### Automatically Start Docker in WSL 2

> **For Windows 11 or Windows 10 with update KB5020030 only**
> **For Docker Engine only**
> **For those who haven’t enabled systemd**

You can specify a default command to run every time WSL starts, allowing Docker to start automatically. Edit the `/etc/wsl.conf` file:

Run the command to edit:

```conf
sudo vim /etc/wsl.conf
```

Press `i` to enter insert mode and paste the following content:

```conf
[boot]
command = service docker start
```

### Releasing RAM Memory from WSL 2

If you notice that WSL 2 is consuming too many resources, execute the following commands within the WSL 2 terminal to release RAM memory:

```bash
echo 1 | sudo tee /proc/sys/vm/drop_caches
```

This method is aggressive as it releases RAM instantly but may be necessary in some cases.

> Do not use this option if you

 are using Docker Engine. It can be used with Docker Desktop or if Docker is not installed.

WSL has a feature called `autoMemoryReclaim` that automatically releases unused RAM. 

There are two options for `autoMemoryReclaim`:

* `gradual`: Releases RAM gradually every 5 minutes.
* `dropcache`: Releases RAM immediately.

Edit the `.wslconfig` file:

```conf
[experimental]
autoMemoryReclaim=gradual
```

### .wslconfig and wsl.conf Files

The `.wslconfig` file is a global configuration file, meaning it affects all Linux distributions installed in WSL 2, as you might have multiple Linux distributions like Ubuntu, Debian, Fedora, etc.

The `wsl.conf` file is a distribution-specific configuration file, affecting only the Linux distribution specified in `wsl.conf`.

Some options you can use in `.wslconfig` and `wsl.conf` are detailed in the official documentation [https://docs.microsoft.com/pt-br/windows/wsl/wsl-config](https://docs.microsoft.com/pt-br/windows/wsl/wsl-config).

> These options can help manage WSL 2 behavior, such as allocating more RAM, CPUs, etc. Basic configurations were already covered in the WSL installation section, Docker installation, and this Tips and Tricks section.

These options will only take effect after restarting WSL with the command `wsl --shutdown`.

## FAQs

### Do I need a Windows 10/11 Pro license to use WSL 2?

No, WSL 2 is supported on all versions of Windows 10/11, as long as they are updated.

### Can I continue developing on Windows without using WSL 2?

Yes, you can continue developing on Windows without using WSL 2. However, WSL 2 provides a development experience closer to Linux, with better performance and more features.

Unless you have a specific need to develop directly on Windows, such as developing .NET applications, your application will likely run on Linux. WSL 2 is a better option if you want to use Windows while leveraging Linux capabilities without dual booting or using a virtual machine.

### Does WSL 2 work alongside other virtual machines like VirtualBox or VMWare?

Yes, WSL 2 can work alongside other virtual machines like **VirtualBox** or **VMWare**. For more details, check the [official reference](https://learn.microsoft.com/pt-br/windows/wsl/faq#poderei-executar-o-wsl-2-e-outras-ferramentas-de-virtualiza--o-de-terceiros--como-vmware-ou-virtualbox-).

### Can I access applications running in WSL 2 from Windows?

Yes, you can access applications running in WSL 2 from Windows by navigating to `localhost` in your Windows browser. WSL 2 has its own network interface, and Windows can access applications running on WSL 2.

### Can I run graphical applications in WSL 2?

Yes, WSL 2 supports graphical applications through the WSLg (Windows Subsystem for Linux GUI) project. For more information, check the top of this tutorial.

### Can I use WSL in production scenarios?

WSL is designed as a development tool and is not recommended for production use.

### Can I run Docker Engine alongside Docker Desktop?

No, you can only run one at a time. It is possible to have both installed, but only one can be running at any given time.