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

  Now, if we login to our client machine and open up Command Prompt, we can ping our Domain Controller by using the following command: `ping 10.0.0.5`. You Domain Controller may have a different private IP address, you can see yours in Azure.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/9f11e404-3d7f-4e88-8c79-a3bac27443f8)

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


  
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
