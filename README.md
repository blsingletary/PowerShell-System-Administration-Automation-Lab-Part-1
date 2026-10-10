# PowerShell System Administration Automation Lab Part 1

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

<h2>Setup</h2>

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

<h2>Folder Creation</h2>
Next step I created folders on the Admin PC using the New-Item command.
<img width="897" height="269" alt="image" src="https://github.com/user-attachments/assets/82fe3f34-dc84-417c-8451-0cad22c3cbbf" />
This script created three new folders:
The Scripts folder which I can use to store all my powershell scripts
The Reports folder to store information about computer inventory and health reports
and The Logs folder to store script execution details and errors

Now the folder is visible in file explorer
<img width="761" height="278" alt="image" src="https://github.com/user-attachments/assets/f4a84a41-a961-49a9-9fb2-43ff402471dc" />

<h2>Script #1 Lab Inventory</h2>
Now that I have a folder where I can store my scripts I can start making them
This time I'll be using Powershell ISE since it supports the creation of scripts
For my first script I'm going to make one that collects the basic system information of our machines

I created a new powershell file and named it Get-LabInventory
<img width="1403" height="731" alt="image" src="https://github.com/user-attachments/assets/7b83edc3-1405-4ca0-a2e7-aca58644c311" />

Now for the actual script
<img width="775" height="715" alt="image" src="https://github.com/user-attachments/assets/f7f06be3-27aa-43a0-b702-5ac612cfbdb0" />
<img width="764" height="181" alt="image" src="https://github.com/user-attachments/assets/e55c58d5-94a8-49f7-bbdc-bdd4bfcde8c2" />


Basically what this script does is it sends out a command that all machines (DC01, PC-A01, & PC-B01) receive and creates a timestamp of the current date and time. It processes each of the selected machines to retrieve the basic system information from them like the Computer name, Windows Version, Ram, and Disk Size. It also lists if there was an error during this process. After it finishes processing each of the machines, it creates a csv file in the Reports folder which I can view using the Notepad App.
<img width="750" height="149" alt="image" src="https://github.com/user-attachments/assets/15787686-1ec7-4588-a3d6-cb16bc070011" />

Now lets see what happens when I run the script while PC-B01 is turned off.
<img width="750" height="132" alt="image" src="https://github.com/user-attachments/assets/e14c56c6-0866-4874-918c-973f3d9bb968" />
<img width="1379" height="138" alt="image" src="https://github.com/user-attachments/assets/89666cf2-1a9d-4dfa-b13d-9f34f8466041" />

<h2>Script #2 Lab Health</h2>
Now for the next script
<img width="679" height="1113" alt="image" src="https://github.com/user-attachments/assets/a0fa49c5-d2ad-48c0-bc14-84b4d6ede171" />

This script will give us the status of the overall heatlh of each machine by checking:
Available storage remaining, if WinRM and Netlogon are running, and Active Directory and DNS. Based on these factors the script will give each machine a label (Healthy, Warning, Failed)
<img width="466" height="155" alt="image" src="https://github.com/user-attachments/assets/15b9b706-172a-45a4-99f8-a0a24d14bed5" />
<img width="779" height="133" alt="image" src="https://github.com/user-attachments/assets/932446d2-83f2-4f7e-bee5-d4e0c4a00342" />

<h2>Script #3 Errors</h2>
Now for the last script
<img width="689" height="952" alt="image" src="https://github.com/user-attachments/assets/1ba259c3-596d-4d05-b1b5-b161785c8bb1" />
<img width="561" height="333" alt="image" src="https://github.com/user-attachments/assets/b098ec70-482d-41ab-a6dc-7ca76559728e" />

This script will check each of the machines for any error events recorded during the last 24 hours. It collects Each event's time, ID, source, and message. It will then save a csv file into the reports folder, but it will also save it to the logs fiolder as well.
<img width="686" height="366" alt="image" src="https://github.com/user-attachments/assets/d5f291fe-293e-4f09-815c-94399082692a" />
<img width="1019" height="496" alt="image" src="https://github.com/user-attachments/assets/dae2efd6-d756-4cf6-8f76-5ad20b696ee6" />
<img width="1019" height="453" alt="image" src="https://github.com/user-attachments/assets/ecfbe80d-8836-49c9-9472-5d4fe53e90e2" />
