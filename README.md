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
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Domain Controller Virtual Machine and Client Virtual Machine Inside of Microsoft Azure
- Change the Private IP Address settings for the Domain Controller to Static
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/sEa0gie.png" height="80%" width="80%" alt="Domain Controller Setup"/>
</p>
<p>
In this tutorial, we will create a Domain Controller VM and Client VM inside of Microsoft Azure, with the purpose of installing Active Directory to the Domain Controller and then connecting the client VM to the Domain Controller so that it can join the Domain. We will also be creating Active Directory accounts utilizing Windows Powershell. To get started, we will log in to our Microsoft Azure account ( please make sure you have your tenant and subscription set up) and start setting up our Domain Controller VM. To do this click the virtual machine icon on the first page and hit the blue "create" button on the next page and select Azure virtual machine. Once you are on the "Create a virtual machine" page you will need to select the following option, for Resource group you will need to create a new one and give it a name. This Resource group should be selected when you create your client VM next. We are going to call our Resource group AD-01 but you can call it whatever you would like. Next, you need to give your virtual machine a name, in our case, we will call it DC-1. Next, you will need to select your region, for example, if you live in California you would select US West. You can leave the defaults in for availability options, availability zone, and Security type. For Image/OS, we need to choose Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2. The VM architecture can be left at x64 and the Azure Spot discount can be ignored. For size, we would need to choose Standard E2s_v3 -2 vcpus, 16iB memory to ensure that our VM doesn't slow during this tutorial. Lastly, we need to create a username and password for when we login into our controller. Once that is done, click "Review+Create" and the "Create" again on the next page to finish this process.
</p>
<br />

<p>
<img src="https://i.imgur.com/1B1At0i.png" height="80%" width="80%" alt="Client VM"/>
</p>
<p>
Now that the domain controller has been deployed we will want to give Microsoft Azure and minute or two to finish creating the resources and then we will move on to creating the Client VM. To create the Client VM we will mimic the same process that we used to create the Domain Controller. We will head to the main virtual machine page and select "create". Once we are on the VM creation page we will select the same resource group that we used for the domain controller, in our case that would be AD-01. Next, we will need to give the VM a name and we will call ours Client-01. All of the other options will mimic what was chosen for the domain controller except for Image/OS, which will need to be Windows 10 Pro, version 22H2 - x64 Gen 2 and our username and password( if you decide to choose a different username and password). Once that is completed we want to click the networking tab to ensure that the Client VM is on the same virtual network as the Domain Controller. In our example our virtual network is called DC-1 -vnet, and the name of your virtual network should be whatever you named your domain controller -vnet. Once you have confirmed that the virtual network is the same, click "Review + Create" and then "Create" on the next page to get your client VM spun up.
</p>
<br />

<p>
<img src="https://i.imgur.com/8lkv8Hq.png" height="80%" width="80%" alt="Static IP Address"/>
</p>
<p>
Now that we have created both the Domain Controller and Client VM, we will want to go into the domain controller's network interface and ensure that the IP address is changed to static instead of dynamic. When an IP address becomes static it never changes and you would want that for a server so that it is easily found on the network and it isn't constantly being changed ever so often by DHCP. To do this click "virtual machines" and then click the name of your domain controller and then click "Networking" under the Settings headline and then click the Network Interface (in our case it is called dc-1958_z1). After clicking that you will be taken to the network interface page and you will click "IP configurations" under Settings. Click config at the bottom and next to the allocation option select "Static" instead of Dynamic and click save to have this change take effect.
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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
