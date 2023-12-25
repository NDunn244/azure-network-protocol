<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
First, you will need to create two VMs on Azure. The first virtual machine will be created with Windows 10 and the second machine will be created with Linux(Ubuntu). Both will have two CPUs and they must be on the same VNET. Once they are created, connect to the Windows machine with Microsoft remote desktop and download Wireshark. Once Wireshark is downloaded, we will open the app and begin to filter for ICMP Traffic first. ICMP(Internet Control Message Protocol) is a network layer protocol used by network devices to diagnose network communication issues. Ping uses this protocol. Ping is used to test connectivity between different hosts on the internet or the network. When you filter Wireshark to capture ICMP packets and ping the private IP address of the Ubuntu machine(10.0.0.5) you can see the results shown below in Wireshark. 
</p>
<p>
<img src="https://i.imgur.com/iFYdOiV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
You can inspect each packet and see the actual data that is being sent in each ping as shown below. 
</p>
<p>
<img src="https://i.imgur.com/tIG5VAl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
In the next step, you will perpetually ping both virtual machines with the command ping -t(ping 10.0.0.5 -t). This will continually ping between both virtual machines until you decide to stop it. In the meantime, in the Linux virtual machine, we will block the inbound ICMP traffic on its firewall so the perpetual ping will stop. In Azure, you will go to the Ubuntu virtual machine's network security group and add a new security rule in inbound security rules. We will set the protocol to ICMP and make it deny anything that comes before the SSH rule. Once this is set, the Windows virtual machine will stop receiving echo replies from the Ubuntu machine and will get "Request Time Out" in Powershell.
</p>
<p>
<img src="https://i.imgur.com/aTG3sxe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bTuyDQm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DKZnm6u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, you will filter Wireshark to observe SSH traffic. SSH(Secure Shell) just gives the user access to the machine's CLI(command line interface). Set the Wireshark filter to capture SSH traffic only. When you SSH into the Ubuntu machine with the command prompt "ssh labuser@10.0.0.5" you can see the SSH traffic in Wireshark.
</p>
<p>
<img src="https://i.imgur.com/cPvIvNN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, you will filter Wireshark to observe DHCP traffic. DHCP(Dynamic Host Configuration Protocol) is used to assign your VM a new IP address using the command"ipconfig /renew". This works on ports 67/68. Once you enter the command in Wireshark, you can see the DHCP traffic. 
</p>
<p>
<img src="https://i.imgur.com/rJxQlMd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, you will filter Wireshark to observe DNS traffic. You will initiate DNS(Domain Name System) traffic by typing in the command "nslookup www.google.com." The "nslookup" command asks the DNS server what is Google's IP address. 
</p>
<p>
<img src="https://i.imgur.com/fCLX8ZZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Finally, you will filter for RDP(Remote Desktop Protocol) traffic in Wireshark. When you enter tcp.port==3389, the traffic is spammed non-stop because you are using Remote Desktop Protocol(a live stream) to connect to the Virtual Machine, therefore traffic is always transmitted.
</p>
<p>
<img src="https://i.imgur.com/acWI0Hj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
