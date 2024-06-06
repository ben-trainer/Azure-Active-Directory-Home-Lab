# Azure-Active-Directory-Home-Lab

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="AD logo"/>
</p>

<h1>Azure Active Directory Lab Summary</h1>

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
To begin we are going to need to create 2 Azure Virtual Machines which will both have a minimum of 2 VCPUs. One will house Active Directory and serve as our Domain Controller, and the other will simulate our client computer.
Virtual Machine 1 - DC-1
Virtual Machine 2 - CLIENT-1


<h2>Installation Steps</h2>


After creating two VMs we will open up our Domain Controller VM and go under Networking and click the Network interface
 
</p>
<br />

<p>
<img src="https://i.imgur.com/Pwa2s6z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now navigate into ipconfig1 and change the IP addressing to static

<p>
<img src="https://i.imgur.com/IsQIKJM.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

After remoting into CLIENT-1 and pinging DC-1 we can see there is no connectivity from CLIENT-1 to DC-1

<p>
<img src="https://i.imgur.com/buohO4q.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

To fix this we will remote into DC-1 and type in wf.msc into our search bar

<p>
<img src="https://i.imgur.com/Frtinsi.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We navigate to inbound rules, filter by protocol so we can see ICMPv4 rules. We will set the two “Core Network Diagnostics Echo Requests” rules to allow.
  
<p>
<img src="https://i.imgur.com/v3IxrJD.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

We will now see connectivity to DC-1 from CLIENT-1

<p>
<img src="https://i.imgur.com/TNi3FXf.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Now we will begin installing Active Directory Domain Services. Navigate to Add roles and features > Next > Next > Next > Check Active Directory Domain Services > Add Features > Next > Next > Install. Let the install finish and continue.
  
<p>
<img src="https://i.imgur.com/R9dT5sL.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

Now we are going to click the flag and click “Promote this server to a domain controller”

<p>
<img src="https://i.imgur.com/IOdPMG4.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We’ll click “Add a new forest” name it bensdomain.com > Next, Enter credentials > Next > Next > Next > Next > Install, and the VM should close its self out.

<p>
<img src="https://i.imgur.com/noYgb4n.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

Now in order to login we must wait for the VM to be restarted and specify the domain name when we log in since our DNS has been set up. We will type in bensdomain.com\labuser for the user name now instead of just labuser.

<p>
<img src="https://i.imgur.com/0OFFt81.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Once our DC-1 VM is finally loaded up we can open up our Server Manager > Go to Tools > Active Directory Users and Computers

<p>
<img src="https://i.imgur.com/0YRWX0X.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

We will right click bensdomain.com > New > Organizational Unit, create an employees and admins unit.
  
<p>
<img src="https://i.imgur.com/UD34Bht.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We will now create new folders under the admins organizational unit, and add a new user named jane-admin.

<p>
<img src="https://i.imgur.com/OQ0SJmo.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Our user still doesn’t have admin rights despite being in the _ADMINS organizational unit, to assign admin right we need to right click Jane Doe > Properties > Member Of > Domain Admins > Add, and search for domain under object names to find the domain roles that we can assign. Assign Jane Doe with Domain Admins > Apply > Ok
  
<p>
<img src="https://i.imgur.com/FpOZgF5.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

We will now sign out of our labuser account and log into Jane Doe’s admin account at bensdomain.com\jane_admin and enter our password.
  
<p>
<img src="https://i.imgur.com/hnw3vr6.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We can see the log in was successful into the Domain by typing “whoami” into the command prompt

<p>
<img src="https://i.imgur.com/tuYrdiv.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

We will now right click the Windows (Start) icon and click System > Rename This PC (Advanced) > Change > Domain, and attempt to connect to bensdomain.com

<p>
<img src="https://i.imgur.com/CdKEkWO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
To fix this we will go to Azure portal and navigate to DC-1 VM > Networking > Network Settings, and copy DC-1’s NIC private IP address, and navigate to CLIENT1 VM and we will set the DNS to DC-1’s private IP address. We can do this by going to CLIENT1 VM > Networking > Network Settings > NIC > DNS servers > custom, and paste the private IP address. Don’t forget to click Save.

<p>
<img src="https://i.imgur.com/Fkka8wa.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

Now we need to restart CLIENT1 from the portal. After opening the CLIENT1 VM back up and logging into labuser we can confirm and test the DNS connectivity using ipconfig /all and ping the private IP address.

<p>
<img src="https://i.imgur.com/2ZRUG3R.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
When we attempt to join the domain again we can see that we are prompted to enter credentials, rather than an error.

<p>
<img src="https://i.imgur.com/16xzPw8.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We can log in with our jane_admin account and can confirm we joined the domain when prompted with this image. We will be forced to restart the computer.
  
<p>
<img src="https://i.imgur.com/64OAYC0.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
We will then remote back into the CLIENT-1 VM and log in using the jane_admin account. We will now right click the Windows Icon > System > Remote Desktop > Select users that can remotely access this PC > Add, type in “domain users” > Check names > Ok > Ok

<p>
<img src="https://i.imgur.com/1VhdmUO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Next we will go to our log into DC-1 with jane_admin and open Powershell ISE as an admin, and we will create multiple users. Create a new file and paste our Powershell user creation script in. 

<p>
<img src="https://i.imgur.com/e7jNr93.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

We will edit the script and change the 10000 accounts to be created to only 1000. Now we will run the script to test the result, by clicking the green play button, or F5. If we are not able to run the script it is because we did not launch Powershell ISE an administrator.
  
<p>
<img src="https://i.imgur.com/O7wzDCM.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>

We can now view the users created within the _EMPLOYEES Organizational Unit.

<p>
<img src="https://i.imgur.com/rvV28iU.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

Now we will test it by logging out of CLIENT-1 and log back in with one of the accounts that was created. Using our domain name and \ the user name and password we selected in the script.

<p>
<img src="https://i.imgur.com/h8TBCjW.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

After confirming that account works we will now log in to another account on the CLIENT-1 VM, after signing off of our balo.liso account we will attempt log ins on another account unable to get the password right. Now we will perform a simple password reset and unlock the account after our numerous failed attempts.

<p>
<img src="https://i.imgur.com/J3d7AZC.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>

Congratulations you have set up Active Directory within Azure, and performed a password reset, unlocking the client's account.

<p>
<img src="https://i.imgur.com/6Kxbibl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>


