<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain

<h2>Deployment and Configuration Steps</h2>

<p>

For this lab, we are going to create two Virtual Machines on the same Virtual Network. One will be a Domain Controller in which we configure Active Directory on. The other will be a Client machine in which we will use as an entry point for the Users we add to our Active Directory. When creating an Azure Virtual Network, a DNS server is automatically created and all machines point to it. In order to connect our Client machine to the Domain Controller, we will edit the Client's DNS settings to point to the Domain Controller and us it as a DNS Server. We also need to change the ip address settings on our Domain Controller from dynamic to static.


<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  <h2>Setup Resources in Azure</h2>
  
  We first create our Domain Controller by creating an Azure Virtual Machine with the Windows Server 2022 image, with the default settings.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/ab9c2f0c-7bea-40e4-b235-83aa2654965d)

  Next we create another Azure Virtual Machine with a Windows 10 image, and we make sure that it is in the same Resource Group and Virtual Network as our Domain Controller we just created.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/29d33b4d-c637-4d81-8d79-7f91e9f80791) | ![image](https://github.com/anbere/configure-ad/assets/90169033/eadb4cd0-bead-432c-bee8-675ca41ae165)
  |:---:|:--:|

  Once those are created, we will change the Domain Controller's IP Address to static in Azure. Navigate to the IP Configuration for the Network Interface Card that was created for our Domain Controller Virtual Machine, and change the settings to static.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/b472ab62-18f2-4623-9706-cf826856b147)

  After this, all of our resources are fully setup.
  
</p>
<br />

<p>
  <h2>Ensure Connectivity between the client and Domain Controller</h2>
</p>
<p>
  To demonstrate connectivity between our client and Domain Controller, we will enable ICMPv4 on the local Windows Firewall for our Domain Controller. This will allow us to test our connection by being able to ping our Domain Controller from our client machine.

  Login to the Domain Controller, and navigate to Windows Defender Firewall with Advance Security. Within the Inbound Rules pane, find the two rules highlighted below and enable them. This will allow ICMP Echo Requests to reach our Domain Controller.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/3a0dcccd-c399-4d96-adeb-aa7d6705eac8)

  Now, if we login to our client machine and open up Command Prompt, we can ping our Domain Controller by using the following command: `ping 10.0.0.4`. You Domain Controller may have a different private IP address, you can see yours in Azure.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/86bd2dbe-30e4-41ca-81f4-cd73e3339d79)

  Great! We have successfully tested our connection from the Client machine to the Domain Controller.

</p>
<br />

<p>
  <h2>Install Active Directory</h2>

  Within our Domain Controller, we will open up Server Manager, if it was not already opened by default. Then we will click on `Add Roles and Features` which will open an Installation Wizard for us. Click through the Installation Wizard using the default settings until we get to Server Roles, and make sure `Active Directory Domain Services` is checked. Continue through the rest of the Wizard and install.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/7c87ac4f-bc7c-482a-8cf3-9d306d526e49)

  Next we need to promote this server to a domain controller from within our Server Manager.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/5b92882f-05ec-4555-93ec-ce5415f4933d)

  We will setup a new forest as 'mydomain.com'.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/ee2fea8e-d7b3-487e-8c1f-e79f74742f2d)

  Continue through the installation, and the server will restart when finished. You will temporarily lose connection to the virtual machine while it is rebooting. Reconnect to the virtual machine as before, however, now that is setup as a Domain Controller you will sign in with the username 'mydomain.com\labuser' if you used the same naming choices as this lab. We must login this way because we need to specify the context of the user.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/f6186f2d-78b7-4c3e-8aea-876706c50ee7)

  Active Directory has now been successfully installed!
  
</p>
<br />

<p>
  <h2>Create an Admin and Normal User Account in AD</h2>

  We will open up Active Directory by searching for 'Active Directory Users and Computers' from the start menu. Welcome to the UI for Active Directory!

  ![image](https://github.com/anbere/configure-ad/assets/90169033/c6d29b1d-08d0-4154-95d6-6cdbdd5bea06)

  Now in our `mydomain.com` we are going to make two Organizational Units. One named '_ADMINS' and one named 'EMPLOYEES' these will hold our Admin users and Employee users.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/ff5a15d5-afa4-45e5-854c-bf7526129e63)

  Within our '_ADMINS' OU, create a new user, I will name them Jane Doe and for this lab we will use the password settings shown below as this is just for testing purposes.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/aff004ee-093c-4bf5-8e25-7b8fd32767e1) | ![image](https://github.com/anbere/configure-ad/assets/90169033/65cd6de5-352d-475a-89ad-10e03be84832)
  |:---:|:---:|

  We have created a generic user, to promote them to an Administrator account we will do the following: 

  Right click the user -> Properties -> Member Of -> Add -> in the textbox write 'Domain Admins' -> `Check Names` to make sure the group exists -> Hit OK and Apply the changes.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/26ca5583-9534-49e7-b5b7-582d23e60389)

  We will use this Admin account from now on when accessing the Domain Controller. Logout from the labuser and login to the new user we just created.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/4ebfbab3-d084-4cf8-88cb-a169e37ab267)

  Now we will join Client-1 to our Domain!

</p>
<br />

<p>

  <h2>Join Client-1 to your domain</h2>

  In order to join Client-1 to our Domain Controller, we need to change the DNS Server settings on our Client-1. Within Azure, navigate to the Network Interface Card for Client-1 and in the DNS Servers settings, choose 'Custom' and input the private IP address of our Domain Controller that we pinged earlier. 

  ![image](https://github.com/anbere/configure-ad/assets/90169033/ceb62b27-c49a-4a25-8b6e-b63aa6b9d730)

  After updating the DNS Settings, restart the Client from Azure. Once restarted login our Client-1, right click the Start menu and clikc on `System`. Next click on `Rename this PC (advanced)`. Click on `Change` and under Member Of -< Domain: type our Domain name.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/f3a2885f-df7e-4180-b646-f1105a5c733c)

  A Windows Security popup will appear requesting us to enter credentials of an account with permission to join the domain. Use the Admin account information that we previously created. If successful, it will tell welcome you to the domain and restart the Client.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/8f1dbb22-ee8a-4075-b8ea-f9bfdba40b91)



</p>



