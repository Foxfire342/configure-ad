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
- Join the Client VM To the Domain Controller
- Create Additional Active Directory Users using Powershell

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
<img src="https://i.imgur.com/CLjtrS3.png" height="80%" width="80%" alt="Remote Into Client VM"/>
</p>
<p>
Now that we have set the domain controller's IP address to static, we will need to test the connectivity between the client VM and the Domain VM. To do this we will use remote desktop to remote into the client VM to send a ping request to the domain controller. If the ping request is rejected then we will need to change the security settings in the Domain Controller to allow it to receive that traffic. To remote into the Client VM we will need to copy the public IP address ( this can be found by clicking the "virtual machine icon" and clicking the client VM and then it can be found near the right-hand side of the page) and paste it into Microsoft Remote Desktop, if you have Windows or Remote Desktop if you have a Mac. For Mac users after adding the PC to Remote Desktop, click on the added account and you will have to enter in your username and password that you created in the beginning for this VM. Click "Continue" and you will be logged into your client VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/oaa4fu1.png" height="80%" width="80%" alt="Pinging the Domain Controller"/>
</p>
<p>
Now that we have officially remoted into our client VM, let's try pinging the Domain Controller. To do this we will need to open up Windows PowerShell which can be found by typing into the search bar. Once Powershell has launched we will type in ping -t along with the private IP address of the domain controller. To find the private IP address of the domain controller simply head to the virtual machine page in Azure and click the domain VM. The private IP will be listed on the very first page. After entering the command you will notice that the pings aren't being accepted by Domain Controller which means that we will have to remote into the DC to configure the firewall settings to allow ICMP traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/yVFWoeg.png" height="80%" width="80%" alt="Microsoft Windows Defender"/>
</p>
<p>
To remote into the domain controller we will follow the same steps that we used for remoting in the Client VM. For Microsoft Remote Desktop and and Remote Desktop (Mac Version) you will need the public IP address( in our example it's 20.84.105.51) for the domain controller and the username and password to successfully log in. Once you are logged in search for Windows Defender Firewall in the search bar and open it up. Once the program is opened, click on "Inbound Rules". Then at the top of the screen, under View, click "Hide Console Tree" and also click "Hide Action Pane" to see more of the inbound rules. Next, sort the list by Protocol by clicking "Protocol" and scroll down to the ICMPv4 protocols. To allow the client's pings to come through, right-click both "Core Networking Diagnostics and select "Enable Rule". 
</p>
<br />

<p>
<img src="https://i.imgur.com/AuEFMJK.png" height="80%" width="80%" alt="Recheck Ping At Client VM"/>
</p>
<p>
Now that those ICMP rules have been enabled on the domain controller, let's shift back to the Client VM and observe the change. As you can see the ping requests are no longer being rejected by the domain controller. 
</p>
<br />

<p>
<img src="https://i.imgur.com/XlShpn8.png" height="80%" width="80%" alt="Active Directory Installation"/>
</p>
<p>
 Next, let's install Active Directory onto our domain controller. To do this we will head to our domain controller virtual machine via remote desktop and click the "start" button and then click "Server Manager". Once we are in the Server Manager we will click "Add roles and features". You will click "Next" on the first three pages and then on the Server Roles page you will need to click "Active Directory Domain Services" and then click next at the bottom. You can click "Next" through the rest of the pages and click "Install" on the confirmation page. Once the installation is complete you can close out the install window.
</p>
<br />

<p>
<img src="https://i.imgur.com/NvpMJIk.png" height="80%" width="80%" alt="Creating The Domain Controller"/>
</p>
<p>
Now that Active Directory has been officially installed we need to also make this Domain Controller an official domain controller. To do this click on the notification flag that appeared in your server manager after installing Active Directory. Then click "promote this server to a domain controller". In the deployment configuration window, click add a new forest on the first page and give it a root domain name. We will call our root domain "mydomain.com" but you can call yours whatever you would like. Click next and then create a password for the Directory Services Restore Mode ( we will not be using Restore Mode in this tutorial). Click Next for the next couple of panes and then click "Install" on the prereq page. Once the installation is done the Domain Controller will restart which will kick you out of your remote desktop instance. You will have to log into the remote instance again but with a slightly different username which will be explained in the next step.
</p>
<br />

<p>
<img src="https://i.imgur.com/BZJjW8v.png" height="80%" width="80%" alt="Login to Domain Controller"/>
</p>
<p>
  To log into the domain controller remote desktop instance we can no longer use a regular username like before because now we have officially promoted our server to a domain controller. Instead, we must reference the rootdoman\username, so for us, it would be mydomain.com\Foxfire342. The password will be the same with no changes.
</p>
<br />

<p>
<img src="https://i.imgur.com/hWin4bK.png" height="80%" width="80%" alt="Organizational Units"/>
</p>
<p>
Once you are back in the domain controller, head back into the server manager and then click "Tools" up top, and then select Active Directory Users and Computers. We will be setting up two organizational units in Active Directory called _EMPLOYEES and _ADMIN. To do this right click mydomain.com(or whatever your root domain name is), select "New" and select organizational unit. 
</p>
<br />

<p>
<img src="https://i.imgur.com/5DNibYA.png" height="80%" width="80%" alt="Creating A User"/>
</p>
<p>
Next, we will create a new user that we will use to log into the Domain Controller for the remainder of this tutorial. Foxfire342 is a nice name but it wouldn't realistically be our username at an organization in real life. In order to create a new user we will right-click our _ADMIN OU and click "New" and then select "User". You can put in any name that you like but in the user logon name, you will want to put in the firstname_admin. In our example, we used the name brittny_admin. On the next page, you want to input a password for this new user and de-select "User must change password at next logon" and select "Password never expires". After that click finish on the next page and you will have created your first user in Active Directory.
</p>
<br />

<p>
<img src="https://i.imgur.com/XPp43Z9.png" height="80%" width="80%" alt="Adding User as an Admin"/>
</p>
<p>
Now that we have created this user, we need to give the user Admin access because it is in the Admin folder. To do this we will right-click on the user and select "Properties".
In the propertied window select the "Member Of" tab and click "add". Next type "Domain Admins" into the white space and click "Check Names". Once the name is underlined, click ok, and then click "apply" in properties to save the changes and then click ok. Now your user is officially a domain admin!
</p>
<br />

<p>
<img src="https://i.imgur.com/VTACePT.png" height="80%" width="80%" alt="Logging in with the next user"/>
</p>
<p>
Now that the user has been created and has admin access, let's login under this new account instead. To do this, disconnect from the domain controller remote instance and log back in as the new user. In our case, the new login username would be mydomain.com/brittny_admin. Once you are logged in under the new user, to verify that you are under that user account pull up the command prompt and type in whoami. You should see your username displayed in the response. 
</p>
<br />

<p>
<img src="https://i.imgur.com/3PD3JZo.png" height="80%" width="80%" alt="Connecting Client to Domain Controller"/>
</p>
<p>
So now that we have created a user with admin access, we will want to connect our client VM to the domain controller. If we try to connect our client VM to our domain controller now, we will get an error message because the virtual network DNS server that the client VM uses will be unable to find the domain controller. To resolve this issue we will have to change the DNS server IP address to the IP address of the domain controller so that the client VM can join the domain. Above is an example of what will happen if you use the virtual network DNS server to try and find the domain controller. 
</p>
<br />

<p>
<img src="https://i.imgur.com/te22hz4.png" height="80%" width="80%" alt="Change the DNS IP Address"/>
</p>
<p>
In order to adjust the DNS setting for the client VM we will need to head back to the Microsoft Azure page and click the client VM. Next, click "Networking" under setting and then click the Network Interface. Then on the next page click "DNS Servers" under the settings header and then select "custom" and type in your domain controller's private IP address and click save at the top to save the changes. In our case, the private IP address is 10.0.0.4. Once this change is saved we will need to go back into the main client VM page and hit restart at the top of the page so that the DNS cache can be flushed. 
</p>
<br />

<p>
<img src="https://i.imgur.com/3eeaq3Z.png" height="80%" width="80%" alt="Joining the Domain Controller"/>
</p>
<p>
Now that the Client VM has been restarted so that the DNS changes can take effect we will log back into your Client VM remote instance and connect to the domain controller. To do this right-click the start button and click "System". Next, click "Rename this PC" and then click the "Change" button in the system properties window. Under Member of, select Domain and type in your root domain. In our example, our root domain would be mydomain.com. Click ok and you will be prompted to enter a username and password with permissions to join this domain. In our previous steps, we already created a new user and so we will use this username and password to finish this connection. Click ok and then you should see a window that pops up and says "Welcome to the {whatever your root domain name is} domain". Then you will have to restart your Client VM for the changes to take effect. 
</p>
<br />

<p>
<img src="https://i.imgur.com/kSfuQUI.png" height="80%" width="80%" alt="Remote Desktop Setting"/>
</p>
<p>
 Now that the Client VM has restarted, let's log back into the remote desktop instance for the Client VM using the new username and password that we used to connect the Domain Controller and Client VM together. Once you are logged in we will need to change the system properties so that all domain users can login into the Client VM remotely instead of just admin users. To do this we would need to right-click the start menu, click "systems" and then click "Remote desktop" under related settings. Next, click " Select users that can remotely access this PC" under the User Accounts header and then click the "Add" button in the Remote Desktop Users window. In the white space type "Domain Users" to allow all users to access the Client VM remotely and then click "Check Names" and click ok and then click ok in the next window to save the changes.
</p>
<br />

<p>
<img src="https://i.imgur.com/Cax9Mnq.png" height="80%" width="80%" alt="Creating Additional Users"/>
</p>
<p>
For the last major step in this tutorial, we will create 1000 new users by utilizing PowerShell. To do this log out of your Client VM and log back into your Domain Controller via Remote Desktop using the same user. In our case, we would be using brittny_admin. Once logged in type Powershell ISE into the search bar and right-click it and select run as administrator. Once in Powershell, click the new script button and then copy the script found on this page "https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1" and paste it into the Powershell script window. Next edit the "Number_OF_ACCOUNTS_TO_CREATE and change it to 1000 and then edit "PASSWORD_FOR_USERS" to a password of your choice. Once this is done hit the green "run script" button at the top.
</p>
<br />

<p>
<img src="https://i.imgur.com/nmCrE96.png" height="80%" width="80%" alt="User Account Creation"/>
</p>
<p>
After running the script you should start seeing user accounts being created from the output being generated below the script. Another way to verify would be to head to Active Directory Users and Computers and then click the _EMPLOYEE organizational unit to see the new users being populated. 
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
