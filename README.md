# ROS 2 Jazzy Installation Guide for Ubuntu 24.04 LTS (deb packages)

This repository provides step-by-step instructions to **install ROS 2 Jazzy Jalisco from source** on Ubuntu 24.04.

---

## Table of Contents
- [Overview](#overview)
- [System Requirements](#system-requirements)
- [System Setup](#System-setup)
  - [Set Locale](#set-locale)
  - [Enable Required Repositories](#enable-required-repositories)
  - [Install Development Tools (optional)](#install-development-tools-(optional))
- [Install ROS 2](#Install-ROS-2)
- [Setup environment](#Setup-environment)
- [Try some examples](#Try-some-examples)
- [Uninstall](#uninstall)

---

## Overview
ROS 2 Jazzy is the officially supported ROS 2 distribution for Ubuntu 24.04 LTS.\
This guide provides step-by-step instructions to build and install it from source on Ubuntu 24.04 (Noble).

![ROS_2_Jazzy_Jalisco](Images/ROS_2_Jazzy_Jalisco.png)

---

## System Requirements
- Ubuntu 24.04 (Noble, Tier 1) 64-bit  
- Minimum 8 GB RAM recommended  
- Internet connection  
- `sudo` privileges  

---
## System Setup

### Set Locale
Make sure you have a locale which supports `UTF-8`. If you are in a minimal environment (such as a docker container), the locale may be something minimal like 'POSIX'. We test with the following settings. However, it should be fine if you’re using a different UTF-8 supported locale.

```bash
locale  # check for UTF-8 support

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```
---
### Enable Required Repositories

You will need to add the ROS 2 apt repository to your system.\
First ensure that the `Ubuntu Universe repository` is enabled.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```
The `ros-apt-source` packages provide keys and apt source configuration for the various ROS repositories.

Installing the ros2-apt-source package will configure ROS 2 repositories for your system. Updates to repository configuration will occur automatically when new versions of this package are released to the ROS repositories.

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```
---
### Install Development Tools (optional)

If you are going to build ROS packages or otherwise do development, you can also install the development tools:

```bash
sudo apt update && sudo apt install ros-dev-tools
```
---

## Install ROS 2

Update your apt repository caches after setting up the repositories.

```bash
sudo apt update 
```

ROS 2 packages are built on frequently updated Ubuntu systems. It is always recommended that you ensure your system is up to date before installing new packages.

```bash
sudo apt upgrade
```

Desktop Install (Recommended): ROS, RViz, demos, tutorials.

```bash
sudo apt install ros-jazzy-desktop
```

ROS-Base Install (Bare Bones): Communication libraries, message packages, command line tools. No GUI tools.

```bash
sudo apt install ros-jazzy-ros-base
```

## Setup environment

Set up your environment by sourcing the following file.

```bash
source /opt/ros/jazzy/setup.bash
```

## Try some examples

If you installed `ros-jazzy-desktop` above you can try some examples.

In one terminal, source the setup file and then run a C++ `talker`:

```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```
In another terminal source the setup file and then run a Python `listener`:

```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```

You should see the `talker` saying that it’s `Publishing` messages and the `listener` saying `I heard` those messages. This verifies both the C++ and Python APIs are working properly. Hooray!

## Uninstall

If you need to uninstall ROS 2 or switch to a source-based install once you have already installed from binaries, run the following command:

```bash
sudo apt remove ~nros-jazzy-* && sudo apt autoremove
```

You may also want to remove the repository:

```bash
sudo apt remove ros2-apt-source
sudo apt update
sudo apt autoremove
sudo apt upgrade # Consider upgrading for packages previously shadowed.
```
