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
- Step 2
- Step 3
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
<p>
<img src="https://i.imgur.com/z8L3NWy.jpg" height="80%" width="80%" alt="Creating Virtual Machine Ubuntu Server"/>
</p>


