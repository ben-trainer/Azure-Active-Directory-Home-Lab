# Installing and Configuring Active Directory in Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="AD logo"/>
</p>

<h1>Objectives</h1>

- Azure Setup
- Domain Controller and Client Setup
- Connectivity
- Active Directory Installation
- User Management
- Domain Joining
- Remote Desktop Setup
- Additional User Creation
- Password Reset
<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Microsoft Active Directory
- Remote Desktop Connection

<h2>Operating Systems Used </h2>

- Windows 10 </b> (22H2)
- Windows Server 2022 Data Center

<h2>Prerequisites</h2>
Create 2 Azure Virtual Machines which will both have a minimum of 2 VCPUs. One will house Active Directory and serve as our Domain Controller, and the other will simulate our client computer.

 - Virtual Machine 1 - DC-1
 - Virtual Machine 2 - CLIENT-1


<h2>Installation Steps</h2>

<p>
Open up the Domain Controller VM and go under Networking and click the Network interface 
</p>
<br />

<p>
<img src="https://i.imgur.com/Pwa2s6z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Navigate into ipconfig1 and change the IP addressing to static

<p>
<img src="https://i.imgur.com/IsQIKJM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p>
Remote into CLIENT-1 and ping DC-1 to confirm there is no connectivity from CLIENT-1 to DC-1
</p>

<p>
<img src="https://i.imgur.com/buohO4q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
To fix this we will remote into DC-1 and type in wf.msc into our search bar
</p>

<p>
<img src="https://i.imgur.com/Frtinsi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Navigate to inbound rules, filter by protocol so we can see ICMPv4 rules. We will set the two “Core Network Diagnostics Echo Requests” rules to allow.
</p>

<p>
<img src="https://i.imgur.com/v3IxrJD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Test connectivity to DC-1 from CLIENT-1
</p>

<p>
<img src="https://i.imgur.com/TNi3FXf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Install Active Directory Domain Services. Navigate to Add roles and features > Next > Next > Next > Check Active Directory Domain Services > Add Features > Next > Next > Install. Let the install finish and continue.
</p>

<p>
<img src="https://i.imgur.com/R9dT5sL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Now we are going to click the flag and click “Promote this server to a domain controller”
</p>

<p>
<img src="https://i.imgur.com/IOdPMG4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p> 
We’ll click “Add a new forest” name it bensdomain.com > Next, Enter credentials > Next > Next > Next > Next > Install
 
 - The VM should close its self out
</p>
<p>
<img src="https://i.imgur.com/noYgb4n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Now in order to login we must wait for the VM to be restarted and specify the domain name when we log in since our DNS has been set up.
 
 - Type in bensdomain.com\labuser for the user name now instead of just labuser.

<p>
<img src="https://i.imgur.com/0OFFt81.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Once DC-1 VM is finally loaded up we can open up our Server Manager > Go to Tools > Active Directory Users and Computers
</p>

<p>
<img src="https://i.imgur.com/0YRWX0X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Right click bensdomain.com > New > Organizational Unit, create an employees and admins unit.
</p>
<p>
<img src="https://i.imgur.com/UD34Bht.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
We will now create new folders under the admins organizational unit, and add a new user named jane-admin.
</p>

<p>
<img src="https://i.imgur.com/OQ0SJmo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Our user still doesn’t have admin rights despite being in the _ADMINS organizational unit
 
 - To assign admin rights we need to right click Jane Doe > Properties > Member Of > Domain Admins > Add
 - Search for domain under object names to find the domain roles that we can assign. 
 - Assign Jane Doe with Domain Admins > Apply > Ok
</p>

<p>
<img src="https://i.imgur.com/FpOZgF5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Sign out of our labuser account and log into Jane Doe’s admin account at bensdomain.com\jane_admin and enter our password.
</p>

<p>
<img src="https://i.imgur.com/hnw3vr6.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Test to see if the login was successful into the Domain by typing “whoami” into the command prompt
</p>

<p>
<img src="https://i.imgur.com/tuYrdiv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Right click the Windows Icon (Start) icon and click System > Rename This PC (Advanced) > Change > Domain, and attempt to connect to bensdomain.com
</p>

<p>
<img src="https://i.imgur.com/CdKEkWO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
To fix go to Azure portal and navigate to DC-1 VM > Networking > Network Settings, and copy DC-1’s NIC private IP address
 
 - Navigate to CLIENT1 VM and we will set the DNS to DC-1’s private IP address. We can do this by going to CLIENT1 VM > Networking > Network Settings > NIC > DNS servers > Custom
 - Paste the private IP address. Don’t forget to click Save.
</p>

<p>
<img src="https://i.imgur.com/Fkka8wa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Restart CLIENT1 from the portal. After opening the CLIENT1 VM back up and logging into labuser we can confirm and test the DNS connectivity using ipconfig /all and ping the private IP address.
</p>

<p>
<img src="https://i.imgur.com/2ZRUG3R.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Attempt to join the domain again we can see that we are prompted to enter credentials, rather than an error.
</p>

<p>
<img src="https://i.imgur.com/16xzPw8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Log in with our jane_admin account and can confirm we joined the domain when prompted with this image. 
 - The VM will be forced to restart.
</p>

<p>
<img src="https://i.imgur.com/64OAYC0.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p>
Remote back into the CLIENT-1 VM and log in using the jane_admin account. 
 
 - Right click the Windows Icon > System > Remote Desktop > Select users that can remotely access this PC > Add, type in “domain users” > Check names > Ok > Ok
</p>

<p>
<img src="https://i.imgur.com/1VhdmUO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Log into DC-1 with jane_admin and open Powershell ISE as an admin, and create multiple users. 
 
- Create a new file and paste our Powershell user creation script in.
</p>

<p>
<img src="https://i.imgur.com/e7jNr93.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Edit the script and change the 10000 accounts to be created to only 1000. Now we will run the script to test the result, by clicking the green play button, or F5. 
 
 - If we are not able to run the script it is because we did not launch Powershell ISE an administrator.
</p>

<p>
<img src="https://i.imgur.com/O7wzDCM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
View the users created within the _EMPLOYEES Organizational Unit.
</p>

<p>
<img src="https://i.imgur.com/rvV28iU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Test it by logging out of CLIENT-1 and log back in with one of the accounts that was created. Using our domain name and \
 
 - Example: bensdomain.com\balo.liso
 - Enter the username and password we selected in the script
<p>
<img src="https://i.imgur.com/h8TBCjW.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
After confirming that account works log in to another account on the CLIENT-1 VM
 
 - After signing off of our balo.liso account we will attempt log ins on another account unable to get the password right
 - Now we will perform a simple password reset and unlock the account after our numerous failed attempts
</p>

<p>
<img src="https://i.imgur.com/J3d7AZC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p>
Congratulations you have set up Active Directory within Azure, and performed a password reset, unlocking the client's account.
</p>
<p>
<img src="https://i.imgur.com/6Kxbibl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>


