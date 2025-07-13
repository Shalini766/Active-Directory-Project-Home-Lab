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







