# PowerShell-System-Administration-Automation-Lab

Project Overview
I created this PowerShell home lab in Oracle VirtualBox to develop practical automation skills commonly required for entry-level IT Support, Helpdesk, and Junior System Administrator positions.
The lab uses my existing Windows Server 2019 domain controller, DC01, and Windows client computers, PC-A01 and PC-B01. These machines simulate a small business environment where administrators manage employee accounts, maintain computers, monitor system health, and perform routine support tasks.
Throughout this project, I will use PowerShell scripts to collect device inventories, generate system health reports, create and manage Active Directory users, perform employee onboarding and offboarding, back up sample files, and schedule recurring administrative tasks.
This document will record my scripts, configuration steps, screenshots, troubleshooting, and verification results as I complete the lab.

Lab Objectives

The main objectives of this project are to:
- Use my existing VirtualBox machines to build a system administration automation lab
- Configure PowerShell remote management between the server and client computers
- Run administrative commands on multiple computers from DC01
- Collect computer information such as operating systems, memory, and disk space
- Export device inventories and system health reports to CSV files
- Monitor disk space, selected services, and Windows event logs
- Automate the creation of Active Directory users using CSV input
- Assign users to Organizational Units and security groups
- Practice password resets, account unlocking, and employee offboarding
- Automate backups of sample files and verify successful restoration
- Schedule recurring scripts using Windows Task Scheduler
- Implement error handling and logging to track script execution
- Troubleshoot connectivity, permissions, and script errors
- Develop PowerShell experience that can be demonstrated in the workplace


Technologies Used
- Oracle VirtualBox
- Windows Server 2019
- Windows 10 or Windows 11
- Windows PowerShell
- Active Directory Domain Services
- Active Directory PowerShell module
- Active Directory Users and Computers
- Domain Name System
- PowerShell Remoting
- Windows Remote Management
- Common Information Model cmdlets
- Windows Event Logs
- Windows Task Scheduler
- CSV files for script input and reporting
- Robocopy for file backup automation
- VirtualBox snapshots
- GitHub for project documentation

I began this lab by creating clones of the machines I created in my Active Directory Lab.
<img width="598" height="543" alt="image" src="https://github.com/user-attachments/assets/6c9f2d27-5488-4899-ad41-e85c896eb991" />

So the first thing I should do before I start scripting is to enable my Client PCs (PC-A01 & PC-B01) to receive powershell commands from another computer, but I came across a small issue.
<img width="969" height="424" alt="image" src="https://github.com/user-attachments/assets/be1675e7-45af-46de-8d14-d17ec13a02e8" />

The error was basically telling me that my network connection was public and I have to set it to Domain or Private, but the issue is that it should be set to Domain already. So I ran the command "Get-NetConnectionProfile" to see what the network is set to, and I figured out that it was Ethernet 1 which connects to the internet is set to public and Ethernet 2 which is connected to the other endpoints is set to Domain.
<img width="370" height="260" alt="image" src="https://github.com/user-attachments/assets/ebf5ef69-9875-4017-a047-e0b51be0ce21" />

So to bypass this error I have to Disable ethernet 1, then do the command and renable the ethernet after.
<img width="543" height="176" alt="image" src="https://github.com/user-attachments/assets/66dad86f-5ebd-402b-abc6-da9f3dd702ed" />
<img width="355" height="142" alt="image" src="https://github.com/user-attachments/assets/496dfa59-f37c-4775-aff5-d9ddb2183f87" />

Now that I got it working, I ran the Test-WSMan command on DC01 which checks whether a computer’s Windows Remote Management service responds over the network.
<img width="604" height="376" alt="image" src="https://github.com/user-attachments/assets/0fa994e2-9964-43f7-81a4-ddd41862aaa2" />

I then ran a command that sends another command to both PC-A01/B01 to give me the hostname of the device.
<img width="628" height="86" alt="Screenshot 2026-10-08 232520" src="https://github.com/user-attachments/assets/0f2330d8-54b5-4b38-96f5-ecb7d9c1f1f1" />

Now we have verified that the scripts are actually working.
