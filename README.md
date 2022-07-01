<h1>Active Directory Home Lab With Bulk User Creation</h1>

<h2>Description</h2>
This project is a walkthrough of how I created an Active Directory home lab Environment using VMWare. I set up a Microsoft Server to run Active Directory on it. I then configure a Domain Controller that will allow me to run a domain. After that I ran a Powershell script to create over 1000 users in Active Directory and proceed to log into those newly created accounts on another client that uses the domain I set up to connect to the internet. This lab simulates a business environment. In this lab I'll need a Microsoft Server 2019 ISO, A Windows 10 Enterprise ISO, VMWare and a Powershell script
<br />


<h2>Languages and Utilities Used</h2>

- <b>Active Directory</b> 
- <b>PowerShell</b>
- <b>CMD</b>

<h2>Environments Used </h2>

- <b>VMWare</b>
- <b>Microsoft Server 2019</b>
- <b>Windows 10</b> (21H2)

<h2>Program walk-through:</h2>

<p align="center">
<b>The network diagram I'll be using for this project</b> <br/>
<img src="https://i.imgur.com/IfxvoYS.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />
<b>Now that Active Directory is configured and my Domain Controller is configured as well, I use a Powershell script to create over 1000 user accounts</b> <br/>
<img src="https://i.imgur.com/ISI6fPb.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<b>The script has run successfully and the output confirmations that the user accounts has been created looks amazing. There were some duplicates that were not created, but that is easily solved by adding to the Powershell script a few lines of code that will tell it what to do in case duplicates occure. Perhaps something along the lines of "If a duplicate occurs, add a 1 to the end of the account name" for example. If you want to see the full code used, refer to the top of this repository. The script is under CREATE_USERS.ps1</b> <br/>
<img src="https://i.imgur.com/MhlDg1o.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<b>For the Virtual Machine that will be hosting my Domain Controller, I need two network adapters. I need the NAT that will use my host IP address from my home router and an Internal Netwowrk Adapter so that my DC can communicate with other Virtual Machines. For the Internal Network I will be using VMnet0. Refer to the diagram at the beginning</b> <br/>
<img src="https://i.imgur.com/UHBjxOd.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/7CLcFGU.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<b>It is now time to create a new Virtual Machine that will act as a user in the domain. I name this machine CLIENT1</b> <br/>
<img src="https://i.imgur.com/wvBRBWf.jpg" height="80%" width="80%" alt="Setting up new Virtual Machine"/>
<br />
<br />
<b>I configure the network adapter so that it is not NAT and can't connect to the internet on my network. The only way this Virtual Machine should be able to connect to the internet is by being assigned an IP from the DC on the Server VM. Refer to the Diagram at the beginning. I have to change the network adapter to be on the same internal network as the Domain Controller, in this case VMnet0</b>  <br/>
<img src="https://i.imgur.com/6IjDUEj.jpg" height="80%" width="80%" alt="Configuring the VM Network Adapter"/>
<br />
<br />
<b>After configuring a seperate virtual machine that will simulate an employee logging into the domain. Lets kill two birds with one stone by renaming the computer CLIENT1 and clicking the box to become a member of the mydomain.com domain. I am prompted to give my log in credential and I chose to use the Administrator account I set up earlier</b>  <br/>
<img src="https://i.imgur.com/ceB3tDJ.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<b>I Successfully join the domain as a member!</b>  <br/>
<img src="https://i.imgur.com/euBgIXf.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<b>I log into a user account I created from the Powershell script to test if everything is configured correctly. Instead of logging into the user account created when I made the virtual machine, I try to log into a user created account in MYDOMAIN</b>  <br/>
<img src="https://i.imgur.com/dPeaySX.gif" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<b>Running command promt to see if the client VM is getting the IP address properly assigned by the DC Server. We can see that I was properly leased an IP address by the domain controller (circled red) and when I ping the domain, it works (circled yellow)</b>  <br/>
<img src="https://i.imgur.com/QBWuCS9.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<b>A final test to see that the work environment and bulk users I created is working</b>  <br/>
<img src="https://i.imgur.com/j6ZRHPz.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<b>I head back into my server VM and check the DCHP to see how many addresses has been leased. We can see here circled in red that my CLIENT1 Virtual Machine has been leased an address. If this was a real company environment there would be hudreds, if not thousands of leased addresses in this folder depending on what the lease duration is of course! I set mine to 20 days in this environment</b>  <br/>
<img src="https://i.imgur.com/qvNKa7v.jpg" height="80%" width="80%" alt="Checking leased addresses"/>
<br />
<br />
<b>Here is another way to check how many computers or devices are currently connected to the domain. We can see that my CLIENT1 computer is being properly recognized in Active Directory. Again, if this was a real environment there would probably be thousands of devices in this folder</b>  <br/>
<img src="https://i.imgur.com/A2dMovv.jpg" height="80%" width="80%" alt="Checking the computers in Active Dirctory"/>
<br />
<br />
<b>Here I am scrolling through all the User accounts I created with Powershell. Over 1000 has been created!</b>  <br/>
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
