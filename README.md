<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Steps</h2>

- Create VMs (Windows 10, Linux (Ubuntu Server))
- Install/Run Wireshark on VM with Windows 10
- Observing by Filtering Protocols

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

<strong>Step 2:</strong> Install/Run Wireshark
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
<strong>Step 3.1.1:</strong> Blocking ICMP in Ubuntu's Network Security Group
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
<br />
<br />
After observing the ping request timing out go ahead and delete the rule to allow pings to come through again.
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

<br />
<br />

<strong>Step 3.2:</strong> SSH (Secure Shell) port 22

<br />
<br />

<p>
In wireshark change the filter to SSH or tcp.port == 22, then in Powershell type ssh (Obuntu's VM username that was created)@(Obuntu's private ip address)
<br />
<br />
Then type yes and it will ask for the password. Take note that as you are typing the password is will not show up.
<br />
<br />
Now you are connected to Obuntu VM's command prompt and can use some <a href="https://www.hostinger.com/tutorials/linux-commands">Linux commands</a> to mess around
<br />
<br />
Once you finish type exit to get back to VM-1 command prompt and close the SSH connection
</p>

<br />
<br />
<em>SSH Protocol</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/OsrUIjq.jpg" height="80%" width="80%" alt="SSH Protocol"/>
</p>

<br />
<br />
<em>Using some Linux Commands</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/VGwpgXv.jpg" height="80%" width="80%" alt="SSH Protocol with some Linux commands"/>
</p>

<br />
<br />

<strong>Step 3.3:</strong> DHCP Traffic 

<br />
<br />
<p>
In Wireshark filter type in DHCP to show DHCP traffic. Nothing should show.
<br />
<br />
In Powershell type in ipconfig /renew and you will see some traffic happen in wireshark
</p>

<br />
<br />
<em>Renewing IP address in powershell to show DHCP traffic</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/5AMAZDf.jpg" height="80%" width="80%" alt="Renewing IP address"/>
</p>

<br />
<br />

<strong>Step 3.4:</strong> DNS Traffic UDP Port 53

<br />
<br />

<p>
In Wireshark filter to DNS traffic and click refresh to clear any traffic.
<br />
<br />
In powershell type in nslookup www.google.com (this is basically asking what google's ip address are)
</p>

<br />
<br />
<em>Using nslookup to see what is google's ip address</em>
<br />
<br />
<p>
<img src="https://i.imgur.com/OL4TPR7.jpg" height="80%" width="80%" alt="nslookup www.google.com to get ip address for google"/>
</p>

<h2>Finished</h2>
<br />
<br />
<p>
Here we observed various Network Protocols using two Virtual Machines and Wireshark. We also utilized inbound rules in Network Security Groups to block ICMP traffic from being received from one VM to the other. Now that both are set up you can observe more traffic and create more rules just to see how it all works.
</p>

<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>
