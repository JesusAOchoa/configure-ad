<p align="center">
<img src="https://imgur.com/p73SOo9.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computers)
- Remote Desktop
- Active Directory Domain Services
- Powershell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>List format/summary</h2>

- Step 1: Deploy two different virtual machines, one with Windows 10, and another with windows 2022 server. The serve will be the DC, and the other will be client.
- Step 2: Format the DC's private IP address from dynamic, to static in order to keep it consistent within the virtual network.
- Step 3: Connect to both Virtual Machines using remote desktop connection, and make sure to login
- Step 4: Start an endless ping from the Client to the DC. If there are no replies, enable Core Networking Diagnostics within the firewall of the DC.
- Step 5: Install Active Directory Domain Services on the DC and promote it as a domain controller.
- Step 6: Establish an administrative account and create Organizational Units (OU) within Active Directory Users and Computers (ADUC). Subsequently, log back in using the admin account.
- Step 7: Configure the Client's DNS settings to use the Private IP address of the DC, and proceed to connect the Client to the DC.
- Step 8: Activate Remote Desktop to allow domain users to access the Client.
- Step 9: Make user accounts using a PowerShell script found <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">here</a> (run PowerShell ISE as administrator)
- Step 10: Connect to the client PC using remote desktop with one of the new accounts generated.

<h2>Deployment & Configuration Steps</h2>

Let's begin our lab by deploying two Virtual Machines (VMs) in Azure. One VM will be running Windows Server 2022, while the other will run Windows 10. It is important to ensure that both VMs are within the same network and subnet. The Windows Server 2022 VM will serve as the Domain Controller (DC), while the Windows 10 VM will serve as the Client machine.

Furthermore, I have configured the DC's Network Interface Controller (NIC) to have a static IP address instead of a dynamic one. This setting will simplify the configuration process later in the lab when we set the Client's DNS settings to the DC's private IP address. Having a static IP address makes it more convenient for remote access to a computer, as there is no need for the device to send renewal requests
<p>
<img src="https://i.imgur.com/xMWxaSv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<p>
Upon establishing Remote Desktop connections to both VMs, I initiated an ongoing ping from the Client to the DC to verify connectivity. However, the ping requests were timing out. To resolve this, I accessed the Windows Defender Firewall on the DC and enabled Core Networking Diagnostics, specifically the ICMPv4 protocol. This adjustment enabled the DC to respond to the ping requests, as indicated in the command-line interface (CLI).
</p>
<img src="https://i.imgur.com/dnYvUTl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Next, we will log back into DC-1 and proceed to install Active Directory Domain Services (AD DS) using the Server Manager Dashboard. After successfully installing AD, I promoted the VM to become a Domain Controller, granting it the ability to manage devices and accounts within the domain. I then configured a new forest with the domain name "mydomain.com". Following the configuration, I restarted the VM and logged back in as the user "mydomain.com\labuser". If the steps were executed correctly, you should now be able to access and run AD Users & Computers as depicted below.
<p>
<img src="https://i.imgur.com/v7QHRGf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<img src="https://i.imgur.com/FlamLHS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
The setup of Active Directory is complete! We will now proceed to create two Organizational Units (OU) named "_ADMINS" and "_EMPLOYEES". Additionally, we will create a new user named "Jane Doe" with the username "Jane_admin" and assign her the role of Administrator. Jane will be added as a member of the Domain Admins Security Group. To finalize this step, we will log out from the default account and log back in using Jane's credentials.
</p> 
<img src="https://i.imgur.com/5GUzjzt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To proceed with the domain setup, I will now join Client-1 to the domain "mydomain.com". Using the Azure portal, we will modify Client-1's DNS settings to reflect the Private IP address of the DC. Once the DNS settings are updated, we will restart Client-1 from within the Azure portal. The provided image below serves as verification that Client-1 is successfully connected to the DC-1 DNS.
</p>
</p>

To enable remote desktop access for non-administrative users on Client-1, we need to log in to Client-1 as an administrator and access the system properties. Within the system properties, select "Remote Desktop" and grant access to "domain users". By enabling this setting for Domain Users, any user accounts within the domain will be able to log into Client-1 as regular users.
</p>
<img src="https://i.imgur.com/fZ2gcOV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> 
</p>
To confirm the ability of normal users to remotely desktop into Client-1, I will utilize a PowerShell script to generate a substantial number of users, specifically 10,000. Once the users are successfully created, we will randomly select one user and establish a remote desktop connection to Client-1. This test will validate the accessibility of normal users to Client-1 via RDP. Also the PowerShell code can be found <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">here</a>
</p>
<img src="https://i.imgur.com/QBffW1K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Utf0x7S.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Bonus Step: How to lock and unlock users' accounts and reset passwords</h3>
To unlock a user's account, simply right-click on the user account and select "Properties." From there, navigate to the "Account" tab and click on "Unlock Account." Additionally, you can right-click on the user account and choose "Reset Password..." to reset the password if needed.
<p>
</p>

<p>
</p>

<p>
</p>

Thank you for exploring my Active Directory tutorial! I trust that you gained valuable insights and developed a better understanding of how to utilize Active Directory. I recommend repeating this exercise multiple times to reinforce your knowledge and proficiency in Active Directory. This is particularly beneficial if you are aiming for an IT role where Active Directory is extensively utilized.

<p></p>

**REMEMBER TO DELETE YOUR RESOURCES AS TO NOT EAT UP YOUR CREDIT**
