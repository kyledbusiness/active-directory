<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory with PowerShell Automation, Group Policy, and Secure File Sharing in Azure</h1>
This lab demonstrates the deployment of an Active Directory environment in Azure, including tasks such as user account creation, Group Policy configuration, and automating user provisioning with PowerShell. Additionally, we will explore setting up network file shares and assigning permissions. The lab also includes monitoring logs using Event Viewer to ensure troubleshooting and auditing capabilities are in place. <br />

<h2>Environments and Tools</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop Protocol (RDP)
- Windows Server 2022 (Domain Controller)
- Windows 10 (Client)
- PowerShell ISE for automation
- Event Viewer for log monitoring

<h2>Step 1: Deploy Azure Resources</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Windows Server 2022 VM for the Domain Controller (DC-1).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Windows 10 VM for the Client machine (Client-1).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensure both machines are in the same Resource Group and Virtual Network (Vnet).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set the Private IP address for DC-1 to static.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Verify connectivity using Azure Network Watcher.
</p>
<br />

<h2>Step 2: Install Active Directory</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into DC-1 and install Active Directory Domain Services (AD DS).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote DC-1 to a Domain Controller, setting up a new forest with the domain name (e.g., cyberlab.com).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart DC-1 and log in as cyberlab.com\labuser.
</p>
<br />

<h2>Step 3: Create Users and Organizational Units</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using Active Directory Users and Computers (ADUC), create three OUs: _EMPLOYEES, _ADMINS, and _CLIENTS.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a domain admin account, jane_admin, and assign it to the Domain Admins group.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in as cyberlab.com\jane_admin for further administration.
</p>
<br />

<h2>Step 4: Join the Client Machine to the Domain</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set the DNS settings of Client-1 to point to DC-1’s Private IP.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart Client-1 and join it to the domain cyberlab.com.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Verify the successful domain join in ADUC by confirming Client-1 appears in the Computers container.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Move Client-1 into the _CLIENTS OU.
</p>
<br />

<h2>Step 5: Configure Group Policy for Remote Desktop</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using Group Policy Management Console (GPMC), create a GPO that allows domain users access to Remote Desktop.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Apply this GPO to the _CLIENTS and _EMPLOYEES OUs.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Run gpupdate /force on Client-1 to ensure the policy takes effect.
</p>
<br />

<h2>Step 6: Automate User Account Creation with PowerShell</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open PowerShell ISE as an administrator on DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Execute a PowerShell script to create 10 thousand user accounts in the _EMPLOYEES OU.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Confirm successful account creation using ADUC.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Test one of the newly created accounts by logging into Client-1.
</p>
<br />

<h2>Step 7: File Shares and Permissions</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create Network File Shares on DC-1

Log into DC-1 as jane_admin and create the following folders on the *C:* drive:
  
- read-access
- write-access
- no-access
- accounting

Set Permissions on Folders

- For read-access, assign Domain Users the Read permission.
- For write-access, assign Domain Users the Read/Write permission.
- For no-access, assign Domain Admins the Read/Write permission.

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Test File Share Access

On Client-1, log in as a normal user (e.g., cyberlab.com\john_employee).

Open the Run dialog and type \\DC-1 to access the shared folders.

Test access to each folder:

- read-access: User should be able to view files but not modify.
- write-access: User should be able to both view and modify files.
- no-access: User should be denied access.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create ACCOUNTANTS Security Group

On DC-1, create a Security Group named ACCOUNTANTS in ADUC.

Assign Read/Write permissions to the ACCOUNTANTS group on the accounting folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Test ACCOUNTANTS Permissions

On Client-1, attempt to access the accounting folder as john_employee (should fail).

Add john_employee to the ACCOUNTANTS security group.

Re-log into Client-1 and confirm access to the accounting folder now works.
</p>
<br />

<h2>Step 8: Viewing Logs in Event Viewer</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Event Viewer on DC-1 and navigate to Windows Logs > Security.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
View logs for:

- Successful/failed logon attempts.
- Group Policy application events.
- Network logon attempts.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Use Event IDs (e.g., 4624 for logon and 4625 for failed logon) to filter and identify specific events.
</p>
<br />

<h2>Conclusion</h2>

<p>
This lab provided comprehensive hands-on experience with deploying and managing Active Directory in an Azure environment. We configured key aspects such as user accounts, Group Policy, and file shares, alongside setting and testing access permissions. Automation with PowerShell enabled bulk user creation, and Event Viewer was utilized to monitor security and troubleshooting logs. Overall, this lab demonstrated practical skills essential for managing an Active Directory infrastructure and ensuring secure, organized user access.
</p>
<br />
