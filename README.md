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
  We first create our Domain Controller by creating an Azure Virtual Machine with the Windows Server 2022 image, with the default settings.

  ![image](https://github.com/anbere/configure-ad/assets/90169033/ab9c2f0c-7bea-40e4-b235-83aa2654965d)

  Next we create another Azure Virtual Machine with a Windows 10 image, and we make sure that it is in the same Virtual Network as our Domain Controller we just created.

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
