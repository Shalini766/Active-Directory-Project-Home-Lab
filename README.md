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
* Enable the network adapter in NAT network

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

### Step 5: Configuring the network

***Creating a NAT Network***
* Go to tools -> Network -> Nat Network
* Name the network -> AD Project
* IPv4Prefix-> 192.168.10.0/24

Now change settings for all the VMs Kali Linux, Windows, Windows Server, and Splunk Server to the NAT Network that has just been created, i.e, AD Project

***Setting up the Static IP for Splunk Server***

* Open the Splunk server and enter

          sudo nano/etc/netplan/50-cloud-init.yaml
* Edit the following points
            PIC
*Enter   *sudo netplan apply*
* Check with *ip a*, this will reflect the new IP address 192.168.10.10/24

### Step 6: Install Splunk on our HOST machine

* Go to www.splunk.com -> Products-> Splunk Enterprise Linux version -> .deb file

### Step 7: Adding guest add-on on Splunk Server (VM)

* sudo apt-get install virtualbox-guest-additions-iso
* Click y for yes, now the package should be installed
* sudo apt-get install virtualbox-guest-utils

* Now go to Devices-> Shared Folder-> Settings-> Add folder
* Select the Path of the Splunk installer and select all three options below
* enter ok
* Now reboot the Splunk Server *sudo reboot*

  ***Adding user to the vboxsf group***

    * sudo adduser mytest vboxsf
    * The user is added to the vboxsf group

  ***Create a New Directory called share***

  * mkdir share
  * sudo mount -t vboxsf -o uid=1000,gid=1000 Active-Directory-Project share/
  * ls -la
  * cd share
  * ls -la (gives all the files listed in that directory)
  * to install Splunk
    
                sudo dpkg -i splunk-9.4.3-237ebbd22314-linux-amd64.deb
  * cd /opt/splunk
  * Change the user to Splunk
    
                sudo -u splunk bash
  * cd bin
  * to run the installer
    
                ./splunk start
  * enter username and password to complete the installation
  *  to exit the Splunk user
    
                exit

   To make sure every time Virtual Machine reboots, splunk runs with the user splunk enter the following command 
  * cd bin
  * sudo ./splunk enable boot-start -user splunk 











