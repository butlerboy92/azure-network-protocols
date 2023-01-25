<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Steps</h2>

- Create VMs (Windows 10, Linux (Ubuntu Server))
- Install/Run Wireshark on VM with Windows 10
- Observing by Filtering Protocols
- Step 4

<h2>Actions and Observations</h2>

<strong>Step 1:</strong> Creating Virtual Machines
<br />
<br />

<p>
Using Azure, create two virtual machines to use for this. One being Windows 10 and the other Ubuntu Server.
<br />
<br />
You may name them whatever you'd like and use whatever username and password (<strong>for ubuntu server uncheck SSH and check password so that you may create a username and password for that VM.</strong>)
</p>
<br />
<br />
<em>VM-1 Windows 10</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/TC2WeYr.jpg" height="80%" width="80%" alt="Creating Virtual Machine Windows 10"/>
</p>
<br />
<br />
<em>VM-2 Ubuntu Server</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/z8L3NWy.jpg" height="80%" width="80%" alt="Creating Virtual Machine Ubuntu Server"/>
</p>

<br />
<br />

<strong>Step 2:</strong>Install/Run Wireshark
<br />
<br />
<p>
On the virtual machine with Windows 10, download Wireshark (Windows Installer 64-bit) and continue with all the default options.
<br />
<br />
Npcap will pop up to install, go ahead and install that with defaults and wireshark will continue to install after.
<br />
<br />
Open Wirehsark in the VM, click Ethernet and then click the blue fin at the top left under 'File' to begin capturing packets. Notice all the traffic already happening that happens in the background.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/hRYo3PE.jpg" height="80%" width="80%" alt="Wireshark capturing packets"/>
</p>

<br />
<br />

<strong>Step 3:</strong> Observing Various Protocols
<br />
<br />
<strong>Step 3.1:</strong> ICMP (Internet Control Messaging Protocol)

<br />
<br />

<p>
In Wireshark type in icmp to filter only icmp traffic (nothing should show at the moment)
<br />
<br />
Back in Azure Portal, go to the Ubuntu Server VM and obtain the private IP address
<br />
<br />
Back in the VM with Windows 10, open Windows Powershell and in the command line type ping (Private IP address from Ubuntu Server VM)
<br />
<br />
Observe what the traffic in Wireshark, you'll notice requests and replies, and in powershell it will tell you how many packets sent, and how many made it or were lost.
</p>

<br />
<br />
<em>Grabbing Ubuntu Server VM's private IP from Azure</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/YxeS3EG.jpg" height="80%" width="80%" alt="Ubuntu Server VM's Private IP Address"/>
</p>

<br />
<br />
<em>Pinging Ubuntu Server VM's private IP with Powershell in Windows 10 VM</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/WaTEtVt.jpg" height="80%" width="80%" alt="Pinging Ubuntu Server VM's Private IP Address from Windows 10 VM"/>
</p>

<br />
<br />
<storng>Step 3.1.1:</strong>Blocking ICMP in Ubuntu's Network Security Group
<br />
<br />

<p>
In Windows 10 VM, in Powershell, send a continous ping by typing in the command line ping (Private IP address from Ubuntu Server VM) -t
<br />
<br />
In Azure Portal search for Network Security Group and click on the VM that has Ubuntu Server
<br />
<br />
From there click Inbound security rules, and click Add. Look for ICMP at the radio buttons and make sure it is ticked. Under Action check Deny. For priority set it before 300 just so we can have this rule take place before any other rule.
<br />
<br />
Once this rule is created. go back to Powershell and notice it will say Request timed out, and observe in wireshark how only requests are being shown. 
</p>

<br />
<br />
<em>Pinging Ubuntu Server VM's private IP with Powershell in Windows 10 VM non-stop</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/likADaX.jpg" height="80%" width="80%" alt="Pinging Ubuntu Server VM's Private IP Address from Windows 10 VM non-stop"/>
</p>

<br />
<br />
<em>Creating rule to deny ICMP in Ubuntu's NSG</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/lfxfsvu.jpg" height="80%" width="80%" alt="NSG Inbound Rule to deny ICMP"/>
</p>

<br />
<br />
<em>Observing Ping request timing out</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/46cwHyk.jpg" height="80%" width="80%" alt="Ping request timing out because of new rule"/>
</p>
