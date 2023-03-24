<h1>Active Directory Home Lab With Bulk User Creation</h1>

<h2>Description</h2>
This project is a walkthrough of how I created an Active Directory home lab Environment using VMWare. I set up a Microsoft Server to run Active Directory on it. I then configure a Domain Controller that will allow me to run a domain. After that I ran a Powershell script to create over 1000 users in Active Directory and proceed to log into those newly created accounts on another client that uses the domain I set up to connect to the internet. This lab simulates a business environment. In this lab I'll need a Microsoft Server 2019 ISO, A Windows 10 Enterprise ISO, VMWare and a Powershell script.
<br />

<h2>Languages and Utilities Used</h2>

- <b>Active Directory</b> 
- <b>PowerShell</b>
- <b>CMD</b>

<h2>Environments Used </h2>

- <b>VMWare</b>
- <b>Microsoft Server 2019</b>
- <b>Windows 10</b> (21H2)

<h2>Links</h2>

- <b>VMWare:</b> https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
- <b>Microsoft Server 2019:</b> https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
- <b>Windows 10 ISO:</b> https://www.microsoft.com/en-us/software-download/windows10

<h2 align="center">Program walk-through</h2>

<p align="center">
<b>The network diagram I'll be using for this project</b> <br/>
<img src="https://i.imgur.com/IfxvoYS.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />
<b>For the Virtual Machine that will be hosting my Domain Controller, I need two network adapters. I need the NAT that will use my host IP address from my home router and an Internal Network Adapter so that my DC can communicate with other Virtual Machines. For the Internal Network I will be using VMnet0. Refer to the diagram at the beginning</b> <br/>
<img src="https://i.imgur.com/UHBjxOd.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/7CLcFGU.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<b>After downloading Windows Server 2019 on the Virtual Machine the first thing I have to do is configure the two network Adapters I have. One is the external NIC and one is the Internal NIC</b> <br/>
<img src="https://i.imgur.com/SwAH8sj.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Now I have to figure out which NIC is our NAT. It is Ethernet0 because its DNS is localdomain</b> <br/>
<img src="https://i.imgur.com/JE7zCDh.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<img src="https://i.imgur.com/mjczWGf.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>I rename the adapters so it is easier for me to tell which is which and it is important later on when setting up the DC and DHCP</b> <br/>
<img src="https://i.imgur.com/iiYpjCy.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Now I configure the Internal network adapter and assign it an IP address based on the diagram above (172.16.0.1) and I do not need to give it a default gateway because the Domain Controller is the gateway. As for the DNS server I assign it an IP based on the diagram because when we install Active Directory it will install DNS. I set it as a loopback address so it pings itself</b> <br/>
<img src="https://i.imgur.com/JIPk50Q.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Now that I know which network adapter is our external and internal, I go ahead and rename the PC from the long complicated name is has now to just DC (Domain Controller). This forces a restart, which is fine</b> <br/>
<img src="https://i.imgur.com/TklrxzC.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<b>After booting back in I start the process of downloading Active Directory. Video cut short but it downloads successfully.</b> <br/>

https://user-images.githubusercontent.com/108043108/176958623-4276b4d1-3c6e-469c-8875-49007d003aa2.mp4

<br />
<br />
<p align="center">
<b>I installed Active Directory Domain Services, but we never actually set the server (or computer) as the domain. Now I have to actually create the domain</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176959446-c4992d67-9bdc-4a5b-8229-e6d18850e29d.mp4

<br />
<br />
<p align="center">
<b>When the server is promoted to a domain, it forces a restart. When I log back in you can see that the domain was created successfully because my admin account now have MYDOMAIN in front of it!</b> <br/>
</p>
<img src="https://i.imgur.com/BmLk2Gc.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<p align="center">
<b>Now instead of using the built in Admin account, I will create a dedicated domain Admin account </b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176960581-a0934287-267c-464e-8eee-8b85ae523910.mp4

<br />
<br />
<p align="center">
<b>I created a domain specific admin account, but it does not have admin priviledges. I have to go into Active Directory and promote this new account to Administrator. When I do that I then log out of the built in Admin account and into my newly created Domain Admin account!</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961105-3f8df2e0-f1e2-490b-a457-e1dd3fc2470a.mp4

<br />
<br />
<p align="center">
<b>Now I need to install and configure the RAS/NAT so that my Windows 10 client computer will be able to access the internet through the internal network via the Domain Controller</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961534-0afd1ae0-c421-4af7-ae76-7b2cefe43bdd.mp4

<br />
<br />
<p align="center">
<b>Now that the role is installed I need to configure the Routing and Remote Access</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961801-c6ed71d1-4f12-4775-82d5-853cac5260da.mp4

<br />
<br />
<p align="center">
<b>Great! Now that Remote Access is installed and configured, it is now time to Install a DHCP Server. This will allow our Windows 10 clients to be assigned an IP address and allow them to browse the internet.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962485-229ae237-b9e9-4178-b857-4741d318d33e.mp4

<br />
<br />
<p align="center">
<b>Now to configure the DHCP and setup a scope. The whole purpose of DHCP is to allows computers on the network to automatically be assigned an IP address. The scope I will be creating will give assign IP addresses in a range, the range being 172.16.0.100-200 so the DHCP will effectively be able to give out 100 IP addresses. I also set the amount of time the IP addresses can be leased out to 20 days. The reason for the lease is when an IP address is assigned, it can't be used by other devices. So if I only have 100 IP addresses and 100 are used, new devices can't be assigned an IP address on the network meaning they can't connect to the internet. A lease is just an amount of time an IP address can be owned (leased) by a device before being recycled. If this was for example a Café that offers wifi and the average time a person spent inside said Café was 2 hours, it would make no sense to lease an IP address to them for 20 days. That would effectively lock up that IP address for that amount of time and no one else could use it. If this were a Café I would recommend setting the lease duration to under 4 hours and give a bigger range. Since this is only VM, the lease duration doesn't matter.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962663-866da4fd-de06-4549-9b86-3925a674bb3d.mp4

<br />
<br />
<p align="center">
<b>To get my powershell script from the internet I need to be able to browse the web. I have to disable the security features on the Domain Controller. If this was an actual production environment I would never do this, security risk. Since this is only a lab environment for myself it is not an issue. I could browse the internet without doing this step but it is annoying because it will spam us warnings for every webpage we visit</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176964588-4b7ab303-c338-4037-8142-996bde30cac3.mp4

<br />
<br />
<p align="center">
<b>Now that Active Directory is configured and my Domain Controller is configured as well, I use a Powershell script to create over 1000 user accounts</b> <br/>
</p>
<img src="https://i.imgur.com/ISI6fPb.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>Here is a video of the script running!</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176953922-60b62f24-fd3f-41b4-ae0c-8780afc7b708.mp4

<br />
<br />
<p align="center">
<b>The script has run successfully and the output confirmations that the user accounts has been created looks amazing. There were some duplicates that were not created, but that is easily solved by adding to the Powershell script a few lines of code that will tell it what to do in case duplicates occur. Perhaps something along the lines of "If a duplicate occurs, add a 1 to the end of the account name" for example. If you want to see the full code used, refer to the top of this repository. The script is under CREATE_USERS.ps1</b> <br/>
</p>
<img src="https://i.imgur.com/MhlDg1o.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>It is now time to create a new Virtual Machine that will act as a user in the domain. I name this machine CLIENT1</b> <br/>
</p>
<img src="https://i.imgur.com/wvBRBWf.jpg" height="80%" width="80%" alt="Setting up new Virtual Machine"/>
<br />
<br />
<p align="center">
<b>I configure the network adapter so that it is not NAT and can't connect to the internet on my local network. The only way this Virtual Machine should be able to connect to the internet is by being assigned an IP from the DC on the Server VM. Refer to the Diagram at the beginning. I have to change the network adapter to be on the same internal network as the Domain Controller, in this case VMnet0</b>  <br/>
</p>
<img src="https://i.imgur.com/6IjDUEj.jpg" height="80%" width="80%" alt="Configuring the VM Network Adapter"/>
<br />
<br />
<p align="center">
<b>After configuring a separate virtual machine that will simulate an employee logging into the domain. Lets kill two birds with one stone by renaming the computer CLIENT1 and clicking the box to become a member of the mydomain.com domain. I am prompted to give my log in credential and I chose to use the Administrator account I set up earlier</b>  <br/>
</p>
<img src="https://i.imgur.com/ceB3tDJ.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>I Successfully join the domain as a member!</b>  <br/>
</p>
<img src="https://i.imgur.com/euBgIXf.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>I log into a user account I created from the Powershell script to test if everything is configured correctly. Instead of logging into the user account created when I made the virtual machine, I try to log into a user created account in MYDOMAIN</b>  <br/>
</p>
<img src="https://i.imgur.com/dPeaySX.gif" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>Running command promt to see if the client VM is getting the IP address properly assigned by the DC. We can see that I was properly leased an IP address by the domain controller (circled red) and when I ping the domain, it works (circled yellow)</b>  <br/>
</p>
<img src="https://i.imgur.com/QBWuCS9.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>A final test to see that the work environment and bulk users I created is working</b>  <br/>
</p>
<img src="https://i.imgur.com/j6ZRHPz.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>I head back into my server VM and check the DCHP to see how many addresses has been leased. We can see here circled in red that my CLIENT1 Virtual Machine has been leased an address. If this was a real company environment there would be hundreds, if not thousands of leased addresses in this folder depending on what the lease duration is of course! I set mine to 20 days in this environment</b>  <br/>
</p>
<img src="https://i.imgur.com/qvNKa7v.jpg" height="80%" width="80%" alt="Checking leased addresses"/>
<br />
<br />
<p align="center">
<b>Here is another way to check how many computers or devices are currently connected to the domain. We can see that my CLIENT1 computer is being properly recognized in Active Directory. Again, if this was a real environment there would probably be thousands of devices in this folder</b>  <br/>
</p>
<img src="https://i.imgur.com/A2dMovv.jpg" height="80%" width="80%" alt="Checking the computers in Active Dirctory"/>
<br />
<br />
<p align="center">
<b>Here I am scrolling through all the User accounts I created with Powershell. Over 1000 has been created!</b>  <br/>
</p>
<img src="https://i.imgur.com/POpjnf9.gif" height="80%" width="80%" alt="Checking the Users created by Powershell"/>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
