<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This demonstration I will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 11 Pro (21H2)
- Ubuntu Server 24.04 LTS -x64 Gen2 (Linux) 

<h2>High-Level Steps</h2>

- Creating the Resource Group and Virtual Machines 
- Downloading Wireshark
- Observing Packets with Wireshark

<h2>Creating the Resource Group and Virtual Machines </h2>


Step 1 - Creating the Resource Group 
------
Before I can observe various network traffic to and from Azure Virtual Machines with Wireshark, I need to create the resource group for my virtual machines (VMs). A resource group is a container that holds related cloud resources together, such as virtual machines and virtual networks. 

To create a resource group, I went to the "Resource Groups" in Azure, then I clicked on "Create". 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/1.0.png)

After I named the resource group as "Networking_Activities", I pressed "Next". Then I pressed "Next" again under the "Tags" tab. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/1.1.png)

Finally, under the "Review + create", I clicked on "Create". 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/1.2.png)

Now, under the "Resource Groups" page in Azure, it shows the newly created "Networking_Activities" resource group. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/1.3.png)

Step 2 - Creating the Windows Virtual Machine (VM) 
------
Now it's time to create a virtual machine. 

In Azure, I went to the "Virtual Machine" page and then clicked on "Create".   

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.0.png)

In “Basics”, for the “Resource group”, I picked the newly created “Networking_Activities”. The name for the VM was to be “WindowsVM”. The region “East US”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.1.png)

The image was to be Windows 11 Pro version 25H2. Image, in this case, stands for what type of device. For the size, I picked Standard_D2s_v3 - 2vcpus, so the VM would run at a decent pace. Then I input the username and password for this VM. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.2.png)

Finally, in the “Basics” section, I confirm for the licensing, then I pressed “Next” until the “Networking” section.  

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.3.png)

Because I wanted to create my own virtual network to change the name, I clicked on “Edit virtual network”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.4.png)

I changed the name to “Lab2Vnet”, and I then pressed “Save”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.5.png)

Under the “Networking” section, I pressed “Review + create”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.6.png)

After the VM passed the validation process to ensure the settings were correct and that the VM would work after creation, I clicked on “Create”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.7.png)

I went into the resource group “Networking_Activities”, and I saw all the newly created cloud resources, including the virtual network (Lab2Vnet) and the VM (WindowsVM). 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/2.8.png)

Step 3 - Checking the Virtual Network
------
I created another VM named “LinuxVM” using the same process except for two thing. First, the "image" I choose was a Ubuntu Server x64, which is a Linux os. Second, instead of creating a new virtual network, I used the virtual network I created called “Lab2Vnet”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.0.png)

I wanted to ensure that both VMs were in the same virtual network so that they would be able to communicate more privately, securely, and efficiently. 

I went into “LinuxVM” to check the “Virtual network/subnet”, which was “Lab2Vnet/snet eastus-1”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.1.png)

Then I went into “WindowsVM” to check the “Virtual network/subnet”, which was “Lab2Vnet/snet eastus-1”, showing they both have the same vnet. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.2.png)


<h2>Downloading Wireshark</h2>

In order to get on the VM, I need to use Remote Desktop, which is a technology that allows a user to access and control another computer over the internet or network. 

I simply search for Remote Desktop in the search bar for the program. Then I put in the public IP address of the Windows VM, and then the credentials. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/6e43a4fcf119d0bf0274436683e7a0321f2d255e/4.0.png)

In order to observe traffic over a computer network, I need to download Wireshark. Wireshark is an open-source network protocol analyzer that allows users to capture and observe traffic running on a computer network. 

I went to the Wireshark website to download the program. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.1.png)

Since I am using a Windows operating system, I chose the “Windows x64 Installer” for the download. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.2.png)

After the download was finished, I opened the file, and then I clicked “Next”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.3.png)

I continued to click “Next”, choosing to check the default options. Finally, I pressed “Install”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.4.png)

The Npcap is a program needed for Wireshark to function, and its download file appears to have it download.

I just installed it.  

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.5.png)

After both programs’ downloads are complete, I click on “Finish”.   

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.6.png)


<h2>Observing Packets with Wireshark</h2>

Step 1 - Configuring a Firewall (Network Security Group) and Observing ICMP Traffic 
------
I opened the Wireshark program, clicked on “Ethernet”, and then the blue fin icon to start observing packets. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.7.png)

I wanted to check for ICMP traffic with Wireshark, so I filtered for ICMP in the top bar. ICMP traffic is what ping uses. The ping command is used to test the connectivity between two devices.

Because I haven’t used the ping command yet, no traffic is shown. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.8.png)

Using the  SecureShell program, I pinged the “LinuxVM” using its private IP address. Because there is a 0% loss, all packets were received, meaning the two VMs can connect. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.9.png)

Because I just used the ping command, I can now observe ICMP in Wireshark. 

The reason why Wireshark shows 8 events instead of PowerShell’s 4 events is that Wireshark captured both the request from the “WindowsVM” and the reply from the “LinuxVM”.

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.91.png)

Next, I will do a perpetual ping from the “WindowsVM” to the “LinuxVM”, using the command “ping 172.16.0.6 -t”. The string of numbers is the private IP address of the Linux VM.

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.92.png)

Now, within Wireshark, there is a repeat of ICMP traffic due to the perpetual ping from the “WindowsVM” to the “LinuxVM”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.923.png)

To achieve this, I needed to go into Microsoft Azure and then into the Network settings of the Linux VM. After I had to press “LinuxVM-nsg”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/4.93.png)

Next, I went to “Inbound security rules”, and after I clicked on “Add” to make an inbound security rule to stop incoming ICMP traffic. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/5.0.png)

For the destination port ranges, I put in * because it means any port in this case. 

The protocol was to be ICMPv4 since I wanted to stop that flow of traffic. Therefore, I chose the action to be “Deny”. 

I put the “Priority” to 290, so it will be in a higher priority than the SSH rule, so this new rule, named “DenyPing,” will override the 290 rule. 

Finally, I clicked on "Add".  

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/5.1.png)

Now the “DenyPing” rule is created and is applied. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/87a25e20c2725876e4d509e2c0c73ac4acbf6a79/5.2.png)

Going back to the Windows VM’s SecureShell, there are now timed-out requests due to the newly created inbound rule to stop ICMP traffic. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4720d065486562376b357267f797b5d862fb8ff3/5.3.png)

Also, within the Windows VM, Wireshark shows ICMP traffic with no response found because of the new inbound traffic rule on the Linux VM. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4720d065486562376b357267f797b5d862fb8ff3/5.4.png)

Next, within Microsoft Azure, I deleted the “DenyPing” rule, so ICMP traffic will be allowed for the Linux VM. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4720d065486562376b357267f797b5d862fb8ff3/5.5.png)

Because I deleted the “DenyPing” rule, the SecureShell on the Windows VM shows a continue stream of replies instead of the request timed out, meaning the Linux VM is now accepting ICMP traffic once again. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4720d065486562376b357267f797b5d862fb8ff3/5.6.png)

Finally, Wireshark on the Windows VM shows a similar trend. Because of the deletion of the “DenyPing” rule, there are requests and replies from the Windows VM and Linux VM, respectively, for ICMP traffic. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4720d065486562376b357267f797b5d862fb8ff3/5.7.png)

Step 2 - Observe Secure Shell (SSH) Traffic 
------
Now, I will observe for SSH traffic with Wireshark, so I filtered for SSH in the top bar. SSH is a network protocol used to access a remote device securely. 

Because I didn’t connect to another remote device on the Windows VM, there is no traffic shown. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.0.png)

Opening PowerShell, I typed ssh labuser@172.0.16.6, where the number is the private IP address of the Linux VM. 

Wireshark now showed SSN traffic, since I was using the SSH command to remotely access another device. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.1.png)

After confirming  I wanted to access the Linux VM, I entered the password for the Linux VM, giving me a welcoming script and some details of the system. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.3.png)

I typed the following command "id" to see the id of the current user using the Linux VM; in this case, the user is "labuser". 

After I used the command "hostname", to tell me that the current device is named "LinuxVM". 

Finally, I used the command "uname -a", which tells me basic information about the system, such as the OS name, hostname, and kernel version. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.4.png)

With every command, Wireshark is capturing the SSH traffic. Even a single keystroke in PowerShell is traffic that can be captured. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.5.png)

I used the "exit" command to end the connection to the Linux VM. To confirm, I used the "hostname" command to see what device I was actively on, which was then the Windows VM. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.6.png)

I also checked Wireshark to see the reset packet that was sent, which was highlighted red. This reset packet tells us that the connection was ended. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/6.7.png)

Step 3 - Observe Dynamic Host Configuration Protocol (DHCP) Traffic 
------
Next, I will observe for DHCP traffic with Wireshark, so I filtered for DHCP in the top bar. DHCP is a protocol where a system automatically gives devices an IP address when they connect to a network.

There's no DHCP traffic shown because the protocol hasn't run yet.

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/7.0.png)

Before I went into PowerShell, I needed to make a simple script so that it would allow Wireshark to capture all the packets I needed. 

I opened Notepad and made the following script:

ipconfig /release
ipconfig /renew

The "ipconfig /release" is a command that will release the Windows VM IP address, meaning the VM won't have an IP address. However, the following command "ipconfig /renew" will ask the DHCP server to give the device an IP address. 

If I just used the command "ipconfig /release", the VM would automatically try to ask the DHCP server for an IP address. Normally, this would be fine, but for a reason I don't know, Wireshark doesn't capture all the packets for DHCP, which is why I needed the script. 

I saved the file in the "ProgramData" folder in the C drive, naming the file "dhcp.bat". 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/7.1.png)

In PowerShell, I used the command "cd c:\programdata" to get to that directory to run the newly created file - dhcp.bat. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/7.2.png)

Finally, I used the following command ".\dhcp.bat" to run the file. Because the script gave commands to run DHCP, Wireshark was able to capture all its packets, showing all the steps of that protocol. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/7.3.png)


Step 4 - Observe Domain Name System (DNS) Traffic 
------
The following traffic I wanted to observe is the DNS protocol, which is a protocol that translates a website name into a IP address.

Because I haven't given PowerShell a command related to the DNS protocol and filtered for DNS traffic, no traffic is shown. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/8.0.png)

I used the command "nslookup disney.com" in PowerShell. The response gave me the Disney website's IP address, and within Wireshark, DNS traffic is shown. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/8.1.png)

Step 5 - Observe Remote Desktop Protocol (RDP) Traffic
------
The final traffic I wanted to observe is RDP traffic. RDP is a network protocol that allows a user to remotely access and control another device over a network connection. 

Because I have access to the Windows VM using RDP and RDP needs to be continuously running for me to access the VM, Wireshark is constantly capturing packets related to RDP. Even moving the mouse within the VM is sending packets to Wireshark.  

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/4031796df3927e9452d5b05f209618ae7738aabb/9.0.png)

