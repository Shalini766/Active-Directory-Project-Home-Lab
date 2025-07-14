# Active-Directory-Project-Home-Lab
This project demonstrates setting up an Active Directory home lab on VirtualBox that includes Windows Server, Splunk, and Kali Linux. Here, we can create a fully functional Domain Controller on Windows Server and explore its environment and learn how to ingest events into Splunk SIEM, and generate telemetry using Sysmon related to attacks and help them detect in the future. 

***Active Directory***(AD) is a database containing users, groups, computers, and many more. To use AD, the server should be installed with Active Directory Domain Services (AD DS), then the server must be promoted to a Domain Controller (DC). This grants the permission to perform authentication and authorization of our server. 

AD DS contains objects and attributes such as user and the user's first and last name in its database.

## Hardware Requirement
* Minimum 16GB of RAM
* Disk space of 250GB
## Software Requirement
* Oracle VirtualBox – A Virtualization platform to host multiple virtual machines.

* Kali Linux – Attacker machine for penetration testing and threat simulation.

* Windows 10 VM – Target machine to simulate user activity and attacks.

* Sysmon (System Monitor) – Windows system monitoring tool to log key events.

* Ubuntu Server – This is for Splunk, to collect logs from Active Directory and Windows machine.

* Windows Server - To set up a fully functional Active Directory Domain Controller.

## Steps:
### Step 1: Install VirtualBox
* Download from https://www.virtualbox.org

* Install on your host OS (Windows/Linux/macOS)

Here we are using Windows OS as our host machine.

### Step 2: Download the ISO images and tools.

#### Windows 10 VM:
* Install Windows 10 ISO (https://www.microsoft.com/en-ca/software-download/windows10)
* Click on Create Windows 10 installation media and download the ISO image
* Add the ISO image to the virtual machine (VM)
* Enable the network adapter in NAT/Host-only mode for isolation

#### Kali Linux VM:
* Download ISO from https://www.kali.org
* To unzip the Kali Linux file, download 7-Zip (https://7-zip.org).
* Basic setup with networking enabled

### Step 3: Installing Windows Server
* Search Windows Server 2022 ISO
* Go to the Microsoft website and click on Windows Server ISO (https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022)
* Select the 64-bit edition in ISO downloads
* Add the ISO image to the virtual machine (VM)
* Name the VM as ADDC01
* Click on Start and Select *Microsoft Windows Server Desktop Experience*
* Install now

### Step 4: Installing Splunk Server
* Go to ubuntu.com, select Ubuntu Server
* Download the Ubuntu Server 22.04.4LTS
* Add the ISO image and name the VM *Splunk*
* Install Splunk Server
* Name your Splunk Server and set the Username and Password
* Log in with your credentials and enter

       *sudo apt-get update && sudo apt-get upgrade*   to update the repositories
  








