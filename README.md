<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this walkthrough, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
</p>
<p>
First, create two VMs (Virtual Machines) on Azure. One machine will be a Linux machine and the other will be a Windows 10 machine. Both will have 2 vcpus and they must be on the same VNET (Virtual Network). Then, navigate to the Windows machine and download Wireshark. (Wireshark Download: https://www.wireshark.org/download.html) Once installed, open Wireshark and filter for ICMP traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping tests connectivity between hosts. When we filter wirehsark to only capture ICMP packets and Ping the private IP address of your linux machine you can visually see the packets on wireshark. 
</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Inspect each individual packet and see the actual data that is being sent in each ping. 
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until you decide to stop it. While the Windows machine is pinging the Linux machine, go to the Linux machine and block inbound ICMP traffic on it's firewall. This will stop it from recieving echo replies from the Linux machine. Block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. You can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. 
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Next use your Windows machine to SSH to the Linux machine. SSH has no GUI, it just gives the user access to the machines CLI. Set the wireshark filter to capture SSH packets only. When you SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5" you can see that wireshark starts to immediately capture SSH packets.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol and works on ports 67/68. It is used to assign IP addresses to machines. Request a new ip address with the command "ipconfig /renew". Upon entering the command prompt, Wireshark will capture DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Time to filter DNS traffic. Set Wireshark to filter DNS traffic. Initiate DNS traffic by typing in the command "nslookup www.google.com". This command asks our DNS server what is google's IP address?
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally. filter for RDP traffic. When you enter tcp.port==3389, then traffic is spammed non-stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
