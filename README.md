<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark and experiment with Network Security Groups. <br />


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
First you will need to create two VMs on Azure. One machine will be a Linux machine, and the other will be a Windows 10 machine. Both will have two CPUs and must be on the same VNET. Once that's done, we can go back to the Windows machine and download Wireshark. Once installed, open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol—ping tests connectivity between hosts. When we filter Wireshark only to capture ICMP packets and ping our Linux machine's private IP address, we can see the packets on Wireshark. 
</p>
<p>
<img src="https://i.imgur.com/iFYdOiV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
We can inspect each packet and see the data sent in each ping. The picture below demonstrates that. 
</p>
<p>
<img src="https://i.imgur.com/tIG5VAl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it. While the Windows machine is pinging the Linux machine, we will go to the Linux machine and block inbound ICMP traffic on its firewall. Once we do that, we will stop receiving echo replies from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow traffic by allowing ICMP on Azure's Linux Network Security Groups page. 
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
Next, we will use our Windows machine to establish an SSH connection to the Linux machine. SSH has no GUI. It just gives the user access to the machine's CLI. We will set the Wireshark filter to capture SSH packets only. When we SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5," Wireshark starts to capture SSH packets immediately.
</p>
<p>
<img src="https://i.imgur.com/cPvIvNN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, we will use Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol. This works on ports 67/68. It's used to assign IP addresses to machines. We will request a new IP address with the command "ipconfig /renew." Once we enter the command, Wireshark will capture DHCP traffic. 
</p>
<p>
<img src="https://i.imgur.com/rJxQlMd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Time to filter DNS traffic. We will set Wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" This command essentially asks our DNS server what Google's IP address is. 
</p>
<p>
<img src="https://i.imgur.com/fCLX8ZZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Lastly, we will filter for RDP traffic. When we enter tcp.port==3389, traffic is spammed non-stop because we are using Remote Desktop Protocol to connect to our Virtual Machine
</p>
<p>
<img src="https://i.imgur.com/acWI0Hj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
