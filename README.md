<h1>Active Directory Home Lab</h1>

<h2>Description</h2>
This lab focuses on setting up Active Directory Domain Services, using 2 virtual machines: One will act as the domain controller which will provide internet to our network, while the other will act as a client within the network. I will explain where to download the files, how to set up the virtual machines, and how to install and configure Active Directory along with Windows 11 for a home lab environment.
<br/>
<br/>
<img src="https://imgur.com/Xz62YRB.jpg" height="70%" width="70%" alt="Network Diagram"/>
<br/>
The diagram shows network flow from top to bottom. Internet will go through the domain controller (DC), which will be passed through the virtual machine (VM) to the Client. 
<h2>Utilities Used</h2>

- <b>Oracle VirtualBox</b>

<h2>Environments Used </h2>

- <b>Windows 11</b> (24H2)
- <b>Server 2022</b>

<h2>Walk-through</h2>

<p align="center">
<h3>Download the ISOs</h3>
 
- https://www.microsoft.com/en-us/software-download/windows11
- https://www.virtualbox.org/wiki/Downloads

First, download two ISOs: the Windows 11 image and the Windows Server 2022 image. Then, download Oracle VirtualBox and install it. <br/>


<h3>Creating the VM for the DC</h3>
Two VMs will need to be created: One for the DC, and one for the client. Let's start with the first one. Inside VirtualBox, click on "new", located near the top.  Name it "DC". Select the path that points to your Windows 11 ISO and then select the Windows 11 (64 bit) from the dropdown menu. Next, Allocate as much memory and as many processors as possible for your computer. I did 4 GB of memory and 4 processors. The more you allocate, the faster your VM will run. <br/>
<img src="https://imgur.com/JlkXsLd.jpg" height="80%" width="80%" alt="DC VM Creation"/> <br/>

Next, click on 'Settings', and click on 'Network'. Adapter 1 is already set; it's the internet we get from the home router. We need to create another NIC to pass along to the internal network. On Adapter 2, select 'internal network'. Name it appropriately so you can identify it later. I named it "-INTERNAL-" to easily differentiate it later in the guide. Close the settings. <br/>
<img src="https://imgur.com/uwVevrY.jpg" height="80%" width="80%" alt="Setting up Internal NIC"/> <br/>

Launch the DC VM, using the Windows Server ISO to boot. After loading, the Microsoft screen appears. Select "next", then "Install now". Select "Windows Server 2022 Standard Evaluation (Desktop Experience). Select "next" a few more times and it will begin the lengthy process of installing Windows Server. <br/>
<img src="https://imgur.com/Jm4y71q.jpg" height="80%" width="80%" alt="Installing Windows Server"/> <br/>

When it asks to push any key to continue, do not push anything. Eventually it will end up asking for a user name and password. Keep the default user name "administrator", and for password, enter something easy to remember. I entered "Password1", because this home lab doesn't need strict security. Click "Finish". <br/>
<img src="https://imgur.com/TdcKLDA.jpg" height="80%" width="80%" alt="Password Screen for Windows Server"/> <br/>

At the home screen, press ctrl+alt+del on the keyboard, or go up to "Input" > "Keyboard" > "Ins ctrl+alt+del. Do this each time to unlock the home screen. Next, enter the password you created. <br/>
<img src="https://imgur.com/OvIrjRf.jpg" height="80%" width="80%" alt="Using Ctrl Alt Del"/> <br/>


<h3>Configuring the domain controller</h3>
Inside the newly created DC, select "No" if it asks about networks. The first thing to do is apply some quality of life changes such as the option to resize the screen. Go to File > Insert Guest Additions CD Image. <br/>
<img src="https://imgur.com/qRtTeWK.jpg" height="80%" width="80%" alt="Insert Guest Additions"/> <br/>

Then, go to file explorer and open the Guest Additions Image in Devices and Drives. Double click the amd64 option to run it. Keep selecting next to install it. After it installs, it will ask to reboot. For best results, select, "No" and then do a full shut down of the VM to ensure the guest additions installs properly. Now start the VM again and log back in. From now on, it is possible to resize the VM screen to your liking. <br/>
<img src="https://imgur.com/KwGcA8p.jpg" height="80%" width="80%" alt="Locating Guest Additions drive"/> <br/>

Now it's time to set up the internal NIC. On the bottom right of the taskbar, click on the computer icon and click on "Identifying...No internet". This takes you to the Ethernet screen where you can click "Change adapter options". Ethernet 1 is most likely the home router which doesn't need to be changed. You can verify by right clicking -> status -> details. The IPv4 Address should resemble your default gateway, for example, 10.0.x.x. Rename this connection as "Internet". I will rename Ethernet 2 as "-INTERNAL-" so that it can be distinguished from "Internet" easily. 
<img src="https://imgur.com/jAoQsY5.jpg" height="80%" width="80%" alt="Internet and INTERNAL"/> <br/> 

Right click on the -INTERNAL- ethernet, click properties, then double click "Internet Protocol Version 4 (IPv4). Referring back to the original network diagram, enter the information for the Internal NIC: <br/>

IP: 172.16.0.1 <br/>
Mask: 255.255.255.0 <br/>
Gateway: <empty> <br/>
DNS: 172.16.0.1 (same as IP) <br/>

<img src="https://imgur.com/kFM9iKW.jpg" height="80%" width="80%" alt="IP settings"/> <br/>
 
Close out of these boxes when finished. <br/>

Next, rename the PC by right clicking the start menu at the bottom left, click system, then "Rename This PC". I will name it "DC" to abbreviate Domain Controller. After this, restart for changes to take effect. <br/>
<img src="https://imgur.com/mkzKiS7.jpg" height="80%" width="80%" alt="Renaming PC"/> <br/>


<h3>Installing Active Directory Domain Services</h3>
The next big step is installing Active Domain Directory Services (AD DS), and creating a domain. With the Server Manager Dashboard open, click on "Add roles and features". Click "Next" 3 times until reaching the Select Server Roles screen. Check the box with "Active Directory Domain Services" and click next until you are able to install, and then do so. Installation may take several minutes. <br/>
<img src="https://imgur.com/6h6aQdW.jpg" height="80%" width="80%" alt="Installing AD DS"/> <br/>

Let's configure what we just installed. Click on the yellow exclamation mark at the top of the screen and click "Promote this computer to a domain controller". Add a new forest. For Root domain name, I will choose the name I pre-selected in the diagram: homelabdomain.com. On the next page, domain controller options, I recommend an easy to remember password. Again, I will use Password1. Continue by hitting "next" several times while it sets everything up, and then "install". This will take a couple of minutes and then automatically restart. <br/>
<img src="https://imgur.com/UDXLnru.jpg" height="80%" width="80%" alt="Creating homelabdomain"/> <br/>

Upon startup, you'll notice the domain name now appears. Log in again. <br/>

The next step is creating an administrative account for the domain. Click on Start -> Administrator > Windows Administrative Tools -> Active Directory Users and Computers. On the left side of this new window, click on homelabdomain.com (or whatever you named yours), and then right click it and select New -> Organizational Unit. <br/>
<img src="https://imgur.com/SY9GCEH.jpg" height="80%" width="80%" alt="Navigating OUs"/> <br/>

Name it "_ADMINS" and uncheck the box to protect the container. Select "OK". Right click the newly created _ADMINS folder, click new -> user. Enter your first and last name. For the user logon name, I will follow the first initial-full last name format. For example, my logon name will be "bbitterman". Additionally, we need to clarify which type of user we are creating, so I will put an "a-" in front, to signify "administrator". Therefore, the logon name for my administrator account will be "a-bbitterman". When finished, click "next". Enter your easy-to-remember password in the next box. Make sure all boxes are unchecked EXCEPT "Password never expires". Since this is a home lab, ease of use is more important than security. Click "Finish".
<img src="https://imgur.com/YbhBmqm.jpg" height="80%" width="80%" alt="Creating Admin User"/> <br/>

You will notice your name in the _ADMINS folder now. To make it an admin user, right click it and click properties. Navigate to the "member of" tab. Click on "Add..." and in the box, enter "domain admins". Check name and it should be underlined to show it recognizes your entry. Click OK and then Apply. <br/>
<img src="https://imgur.com/KF4XjGF.jpg" height="80%" width="80%" alt="Domain admins group"/> <br/>

Log out and return to the main login screen. On the bottom left, select "Other user" and log in with your admin name, the one that starts with "a-". <br/>

Referring back to the network map, we need to install RAS/NAT (Remote Access Server/Network Address Translation) so that the clients will be able to access internet through the domain controller. On the Server Manager Dashboard, click on "Add roles and features". Click "Next" until you get to the "Select server roles" screen. Check the "Remote Access" box. Click "Next" two times until you get to "Select role services". Check the box to install "Routing". Then keep clicking "Next", and finally, "Install". This may take a few minutes. Close the boxes when finished. <br/>
<img src="https://imgur.com/k9KPFwX.jpg" height="80%" width="80%" alt="Installing Remote Access"/> <br/>

Back in the dashboard, at the top right, select tools -> Routing and remote access. In the box that appears, right click "DC (local)" and select Configure and Enable routing". In the wizard that pops up, click "Next" once, and for the configuration box, make sure "Network address translation (NAT)" is selected. In the box, you should be able to see your NICs. If not, close back to the dashboard and try again. The interface that reaches the public internet should be selected; in this case, it's the one labeled as "internet". Click "Next" and "Finish". <br/>
<img src="https://imgur.com/4HILjOv.jpg" height="80%" width="80%" alt="Routing Internet"/> <br/>

There's still one more DC configurationg step so the clients can recieve internet - setting up a DHCP server. Installing the DHCP server is similar to installing AD DS and Remote Access<br/>

Begin by going back to the dashboard. Click on "Add roles", click next until the checkboxes appear in the "Select Server Roles" box, and check and enable "DHCP Server". Select "Add Features", "Next" several times, and finally, "Install". This will take a few minutes to install. 
<img src="https://imgur.com/IMCqivq.jpg" height="80%" width="80%" alt="Creating DHCP server"/> <br/>

Once finished, in the top right of the dashboard, go to Tools -> DHCP. This is where the scope, or the range of addresses given to clients, is entered. In the dropdown under your domain, right click IPv4 and select "New scope". The name will be the range used. I'll use the range I outlined earlier in the network diagram. Enter "172.16.0.100-200". Click "Next", and enter the starting range (172.16.0.100) and ending range (172.16.0.200). Ensure the length is "24" so the subnet mask is 255.255.255.0. <br/>
<img src="https://imgur.com/NbNGb3C.jpg" height="80%" width="80%" alt="Creating a scope"/> <br/>

Click "Next" to skip exclusions and select the lease time for clients. A lease time of several days for home lab purposes is okay. In the box asking to configure DNS options, select "Yes". and "Next". In the IP address box, assign the IP of the internal NIC. In this case it will be 172.16.0.1. Click "Add". Click "Next" several times until you choose to active the scope and select "Finish". In the DHCP box, right click your domain and click "Authorize". DNS is now complete. <br/>

There is also a freature that needsto be disabled to allow clients to browse the internet. Return to the dashboard, click on "Configure this local server", and disable "Internet Explorer Enhanced Security". <br/>

Before switching to the client side, users need to be created to simulate clients. Return to the "Users and Computers" box from the start menu. Right click the domain and select New -> Organizational Unit. I named it "_USERS" so it was easy to identify. Uncheck the "Protect container" option. Click OK. Right click on the newly created _USERS folder, right click and create a new user. I'm going to name this one Bob Smith with the username "bsmith". <br/>
<img src="https://imgur.com/rJjM13m.jpg" height="80%" width="80%" alt="Creating a scope"/> <br/>

Give him the same easy to remember password as usual, and for this user, I will enable "Password never expires". To simulate a live environment, create another user so there will be more than one to choose from; this time I picked "Pamela Thompson" with the username "pthompson". Her password will also be set to never expire.v<br/>

Everything on the DC side is complete. The domain is created with a domain name, it's recieving internet from the router, which it will translate and pass along to the clients through the VM network. All that remains is installing Windows 11 in a VM and joining our clients to the domain.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
