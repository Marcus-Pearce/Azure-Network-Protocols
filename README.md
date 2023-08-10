# Azure-network-protocols
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, HTTP, RDP,DNS,ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04(Linux)

<h2>High-Level Steps</h2>

- Create 2 Virtual Machines
- Install Wireshark on Windows 10 VM
- Utilize the command line
- Configure Network Security Group
- Observe protocol traffic

<h2>Actions and Observations</h2>

<p>
Create a Windows 10 Virtual Machine and put it in a new group named RG-Protocols.Name the Virtual Machine VM-Windows. Use at least 2 Virtual CPU's when setting up first VM. Allow time for deployment so that the Virtual Network can be created and the second VM can be on the same network.


![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/b6d52a99-155b-4b8f-9159-828cf5317647)


</p>

<p>
Creat a Linux Virtual Machine and add it to the same Resource group created previously. Name it VM-Linux

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/c9a548fe-3a0d-41e1-9b26-c3e71aee3a28)

</p>
<p>
VM-Linux will automatically join the Virutal Network created in the first VM's deployment.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/93f6faad-3702-4300-8ee0-3dcb3d28d607)

</p>

<p>
Use Remote Desktop to connect to VM-Windows using the Public IP Address.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/1003eea5-8f9d-4ce6-ae99-7289aecb950c)

</p>
<br />

<p>
Open Microsoft Edge and google Wireshark. Download and install.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/57588dfb-7cde-440b-a4f3-ead0ae2375c4)

</p>

<p>
NCAP installation will prompt during Wireshark. Install and continue installing Wireshark.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/89bed05a-fecd-43c0-af79-09e3441d4458)

</p>

<p>
Run Wireshark after installation. Filter for ICMP and observe the traffic.Notice how there is no traffic once filtered because ICMP is the protocol ping uses.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/040d9191-8ae2-4937-a0d2-b908f8daf124)


</p>

<p>
Open Command Prompt by hitting the Windows Key + r. Then type cmd and hit enter.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/802a4c29-2521-436d-a2e4-a3bef6179511)

</p>

<p>
Copy the Private IP Address of VM-Linux and try to ping it from VM-Windows. 

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/f6d0237c-2780-411a-a372-fe7ade522815)

</p>
<br />

<p>
Observe the traffic on Wireshark. Notice the requests and replies between VM-Windows(10.0.0.4) and VM-Linux's(10.0.0.5) Private IPs.
  
![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/5072a2e9-1631-4ccc-ba68-74daac79e96c)

</p>

<p>
Ping facebook.com.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/efb61d2a-01d9-46f6-acae-e95a41c4b58a)

</p>
<br />

<p>
Observe the traffic on Wireshark on VM-Windows.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/0a1c1af0-a084-40ce-9f74-0fd4c2cef8eb)

</p>
<p>
Start a perpetual ping to VM-Linux's Private IP Address(10.0.0.5) using command ping -t 10.0.0.5 .

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/05a98a1f-5787-4ced-962d-4e479991a9d3)

</p>
<br />

<p>
On your main computer go to the Network security group settings of VM-Linux by going to Networking and clicking the Security group. Go to Inbound Security Rules and add a new security rule to deny all ICMP traffic from anywhere. Place it with a priority value higher than the first rule so that it happens first. Type DO_NOT_ALLOW_FROM_ANYONE in the name field.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/03947248-6bdd-462b-aaa3-1fffdb0bd4a7)

</p>
<p>
Observe the command line in VM-Windows. Observe how the ping is now being timed out due to the ICMP protocol being blocked.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/8eb2bcc3-4ae0-4cc3-87d6-5bc8e0b94d9d)

</p>
<br />

<p>
In Wireshark observe how Vm-Linux is no longer sending replies back. Only requests from VM-Windows Private IP Address.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/721a43b3-b792-4587-86be-16065a6c638e)

</p>
<p>

Delete the inbound security rule denying the ICMP protocol in the Network Security settings of VM-Linux and observe the traffic on the command line and Wireshark. Now stop the ping by hitting Control + C on the command line.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/36b24dc5-0f6c-46e7-b547-077143ee903c)

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/9ff6c33f-a7af-4f0b-b947-81a384b44a55)

</p>
<br />
<p>
Now on Wireshark filter for SSH. Notice there is no activity currently.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/d33ccbbe-fc38-4688-8c94-76752434622d)

</p>
<p>
SSH into VM-Linux using ssh 10.0.0.5 and use credentials created during VM-Linux setup. 
  
![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/2540133f-a492-4192-8dc2-decd3e112559)


</p>
<br />
<p>
Observe the SSH traffic on Wireshark. Exit SSH by typing exit.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/f8f64b0e-db20-41d3-87a4-b101705f0304)


</p>
<p>
On Wireshark filter for DHCP

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/8741cfb3-580a-4af5-9e59-27adb59e3642)

</p>

<p>
In the command line run command ipconfig /renew to try to issue your computer a new IP Address.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/2e398a80-d121-4fc6-9545-76501c5f1020)


</p>
<p>

Observe the DHCP traffic on Wireshark.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/e116fc42-9ae0-4eb9-ab1c-023bd731bc54)

</p>

<p>

Filter for DNS on Wireshark and observe the DNS traffic.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/abc6ed89-1f03-41d3-8cd6-a8c543dbd249)

</p>
<p>
In the command line run the command nslookup google.com and nslookup facebook.com to find out what their IP Addresses are. You can use -4 to get only IPv4 addresses.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/94ebc02b-7a89-4363-85ca-bdc10638f5ab)

</p>

<p>
Observe the DNS traffic on Wireshark. 

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/8a3d94b9-94d6-441c-8155-c605d5af1c0d)

</p>

<p>
Filter for tcp.port == 3389 and observe the non stop traffic because that it the port that RDP(Remote Desktop Protocol) uses and you are using it to be inside of VM-Windows.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/88083e59-454e-4790-a654-c93663591ca6)

</p>

<p>

Pull up a browser and go to facebook.com.
![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/c03b1819-8a19-44db-94d7-364940019fbb)

</p>


<p>
Set the filter on Wireshark to Http and observe the traffic.

![image](https://github.com/Marcus-Pearce/Azure-network-protocols/assets/140969692/a1d31ff4-1d2c-4b94-a0f4-3a103aeac9e5)

</p>
