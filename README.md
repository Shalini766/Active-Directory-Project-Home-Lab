# Active-Directory-Project-Home-Lab
This project demonstrates setting up an Active Directory home lab on VirtualBox that includes Windows Server, Splunk, and Kali Linux. Here, we can create a fully functional Domain Controller on Windows Server and explore its environment and learn how to ingest events into Splunk SIEM, and generate telemetry using Sysmon related to attacks, and help them detect in the future. 

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
* Click y for yes. Now the package should be installed
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
  * to exit the Splunk user
    
                exit

   To make sure every time the Virtual Machine reboots, splunk runs with the user splunk, enter the following command 
  * cd bin
  * sudo ./splunk enable boot-start -user splunk

  Now the Splunk Server is running, next we have to install Splunk Universal Forwarder on both the Windows target machine and the Windows Server.


  ### Step 8: Installing Splunk Universal Forwarder

  #### On Windows Target Machine:
  * Rename your PC to **target-PC** and restart
  * Set the static IP 192.168.10.100
  * Default Gateway 192.168.10.1
  * DNS Server 8.8.8.8
  * Try accessing Splunk on a web browser at 192.168.10.10:8000

  #### On Windows Server:
  *
  * Set the static IP 192.168.10.7
  * Default Gateway 192.168.10.1
  * DNS Server 8.8.8.8
  * Change the computer name to **ADDC01**

    
 #### On both Windows and Windows Server
  * Go to www.splunk.com -> Free trials and downloads -> Universal Forwarder -> Windows 64-bit -> Download
  * Execute the downloaded file
  * Enter Username and password
  * Receiving Indexer 192.168.10.10:9997
  * Install

### Step 9: Install Sysmon on Windows 10 VM and Windows Server
- Once the test machine is running, open a web browser and go to https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
- Download Sysmon from Microsoft Sysinternals and extract it to a folder.
- Download the configuration file from https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml
- Open PowerShell, run as an administrator.
- To be in the same folder, copy the folder path and paste it into PowerShell.
- Install using the below command

         cd C:\Users\sha\Downloads\Sysmon
        .\Sysmon64.exe -i ..\sysmonconfig.xml

   ### Configuring Splunk to ingest Sysmon Logs
  - Go to the folder where Splunk is installed and check whether the inputs.conf file is available.
  - Configure the inputs.conf file to ingest the Sysmon logs.
  - Open Notepad as Administrator and paste the contents of inputs.conf
    
           C Drive->Program Files->User->Splunk Universal Forwarder->etc->System->local->inputs.conf
    
  - Make sure to restart the Splunk Universal Forwarder services.
    
          Services->Run as Administrator -> SplunkForwarder Service->Change log on to Local System->Restart

### To Create an Index in Splunk

- Open a web browser, enter 192.168.10.10:8000
- login into Splunk -> Settings -> Indexes -> Create a new Index -> Name the Index -> endpoint
-  Add Splunk Add-on for Sysmon to parse the data

### Enabling Splunk Server to receive data

- Go to Settings -> Forwarding and Receiving -> Configure Receiving -> New Receiving Port -> 9997 -> Save


After setting up Splunk and Sysmon on both Windows and Windows Server, we should see two hosts on Splunk

### Step 10: Installing and Configuring Active Directory (AD) on Windows Server 

 ***Objective:*** To install and configure AD on Windows Server and promote it to the Domain Controller (DC) and configure the target Windows to join the newly created domain.

 * Open Server Manager -> Manage -> Add Roles and Features -> Next
 * Select Role-based or feature-based installation
 * Select **ADDC01** -> Next
 * Select Active Directory Domain Services -> Add Features -> Next -> Install

 * Select Flag Icon -> Promote this server to a domain controller
 * Add New Forest -> Root Domain Name -> mytest.local -> Install

After installation, the server will automatically restart, and we can see our new domain **MYTEST\ADMINISTRATOR**

***Creating some New Users on our Domain*** 

* Log in to the newly created domain
* Open Server Manager -> Tools -> Active Directory Users and Computers
* Click on our domain, mytest.local

To create a new unit in the mytest folder
* Right-click -> New -> Organisational Unit -> name it IT
* IT -> Right-click -> New -> User
* Name the User -> Jenny Smith
* Create Password -> Finish

Now we have a new user named Jenny Smith; similarly, we can create any number of users for various organizational units.

* IT -> Jenny Smith
* HR -> Tan Smith

   ***Joining our Windows to the domain and authenticating using Jenny Smith Account***
  
* Change the DNS to 192.168.10.7 which is DC
* Windows -> This PC -> Properties -> Advanced Sysstem Settings
* Computer Name -> Change -> Select Domain -> MYTEST.LOCAL
* Enter username -> administrator
* Create a password
* Restart

 Now the new users Jenny Smith and Tan Smith can log in with their accounts.

 ## Performing Bruteforce Attack using Kali onto our new users and viewing the telemetry on Splunk

 **On Windows**
 * Enable Remote Desktop Connection
 * This PC -> Properties -> Advanced System Settings -> Remote tab -> Allow Remote connections -> Users -> Add Users (Jenny Smith & Tan Smith) -> Apply

**On Kali**
* Setup static IP address 192.168.10.250
* Gateway 192.168.10.1
* DNS Server 8.8.8.8

Ping Google on 8.8.8.8 and Splunk on 192.168.10.10 to check connectivity.
* sudo apt-get update && sudo apt-get upgrade -y
  
 **To create our attack**
 
 * Create directory ***ad-project***
 * Use tool called Crowbar to perform attack, install

         sudo apt-get install -y crowbar
   
 * cd /usr/share/wordlists/
 * sudo gunzip rockyou.txt.gz
 * cp rockyou.txt ~/Desktop/ad-project
 * cd ~/Desktop/ad-project
 * ls -lh (to see the file size)
 * head -n 20 rockyou.txt (to display the first 20 lines)
 * head -n 20 rockyou.txt > passwords.txt (output the file to passwords.txt)
 * cat passwords.txt
 * nano passwords.txt  ( to edit and enter your user password)

* crowbar -h
* crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32

### Telemetry Generated in Splunk

* Search and reporting
  
      index=endpoint tsmith

* As we can see, there are multiple failed login attempts
* index=endpoint tsmith EventCode=4624
* This shows successful login

The entire scenario indicates a successful brute-force attack, which confirms this as a true positive case. Attach all the evidence, document it, and escalate to senior team for further remediation and analysis, which includes 
*containment of the system
*changing credentials
*use of strong passwords
*securing the network
*limiting login attempts 
*IP address blocking, and using multi-factor authentication.











