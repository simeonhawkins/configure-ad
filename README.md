<p align="center">
<img src="https://github.com/user-attachments/assets/c5d2f5fd-c20b-4a6f-909a-340a48d7b74e" alt="osTicket logo"/>
</p>

<h1>Configure Active Directory within (Azure VMs)</h1>

This tutorial provides a step-by-step guide for deploying an on-premises Active Directory environment using Azure Virtual Machines.


<h2>Environments and Technologies Used</h2>

	•	Microsoft Azure (Virtual Machines/Compute)
	•	Remote Desktop
	•	Active Directory Domain Services
	•	PowerShell

<h2>Operating Systems Used</h2>

	•	Windows Server 2022
	•	Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

Step 1: Create Two Virtual Machines in the Same VNet

	•	In this lab, we will create two VMs in the same Virtual Network (VNet):
	•	DC-1: The Domain Controller (will provide Active Directory services).
	•	Client-1: The Client machine that will be joined to the domain.
	•	Change the DC-1 to have a static IP address since it will be serving Active Directory services to the client machine.

Step 2: Configure DNS Settings on Client-1

	•	Set the DNS settings on Client-1 to point to the DC-1 IP address. This will allow Client-1 to resolve the domain controller for authentication.

<p align="center">
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="DNS Configuration"/>
</p>


Step 3: Test Connectivity

	•	From Client-1, try pinging DC-1. The ping may initially fail due to firewall settings on the domain controller.
	•	Enable ICMPv4 on the firewall of DC-1 to allow incoming ping requests.
	•	Test the connectivity again from Client-1. The ping should now succeed.

<p align="center">
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Ping Test"/>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Ping Successful"/>
</p>


Step 4: Install Active Directory on DC-1

	•	Log back into DC-1 to install Active Directory Users & Computers.
	•	Promote the VM to a Domain Controller, and set up a new forest named mydomain.com.
	•	After the setup, restart DC-1 and log back in using:
Username: mydomain.com\labuser.
	•	If the setup was successful, you should be able to access Active Directory Users & Computers.

<p align="center">
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Active Directory Setup"/>
</p>


Step 5: Create Organizational Units and Users

	•	In AD Users & Computers, create two Organizational Units (OUs):
	1.	_EMPLOYEES
	2.	_ADMINS
	•	Create a new user within the _ADMINS OU:
	•	Name: Jane Doe
	•	Username: Jane_admin
	•	Add Jane_admin to the Domain Admins security group.

<p align="center">
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Creating Organizational Units"/>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Creating Jane Admin"/>
</p>


Step 6: Join Client-1 to the Domain

	•	In the Azure Portal, change Client-1’s DNS settings to the DC’s Private IP address.
	•	Restart Client-1 from the Azure portal.

<p align="center">
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Set DNS to DC-1 IP"/>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="DNS Configured Successfully"/>
</p>


Step 7: Verify Client-1’s Domain Connection

	•	Navigate to System Settings > About > Rename this PC (advanced).
	•	Change the domain to mydomain.com.
	•	Enter your domain credentials:
Username: mydomain.com\labuser
	•	Restart Client-1 to complete the domain join.

<p align="center">
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Joining Domain"/>
</p>


Step 8: Configure Remote Desktop Access for Non-Admin Users

	•	Log into Client-1 as an Admin.
	•	Open System Properties > Remote Desktop.
	•	Allow Domain Users to access Remote Desktop.

<p align="center">
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Remote Desktop Configuration"/>
</p>


Step 9: Create Multiple Users Using a PowerShell Script

	•	On DC-1, use a PowerShell script to create thousands of users in the domain.
	•	After the script completes, select one of the created users and log in to Client-1 using Remote Desktop.

<p align="center">
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="PowerShell Script"/>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Generated Users"/>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="User Login"/>
</p>


	•	As shown in the images, we successfully logged in using the generated user bab.hubo, verifying that a normal domain user can RDP into Client-1.

<h2>Summary</h2>

	•	We deployed Active Directory within Azure and set up Client-1 to join the domain.
	•	We created Organizational Units (OUs), added users, and assigned administrative roles.
	•	We configured Remote Desktop access for domain users and verified access using a generated user account.

This lab provides a solid foundation for understanding on-premises Active Directory configurations in a cloud environment. Let me know if you have any questions or need further clarification!

