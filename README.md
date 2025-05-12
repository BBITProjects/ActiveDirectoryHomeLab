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





















 
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
