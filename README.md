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

<h2>High-Level Steps</h2>

- Creating the Resource Group and Virtual Machines 
- Step 2
- Step 3
- Step 4

<h2>Creating the Resource Group and Virtual Machines </h2>

Step 1
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

Step 2
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

Step 3
------
I created another VM named “LinuxVM” using the same process except for two thing. First, the "image" I choose was a Ubuntu Server x64, which is a Linux os. Second, instead of creating a new virtual network, I used the virtual network I created called “Lab2Vnet”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.0.png)

I wanted to ensure that both VMs were in the same virtual network so that they would be able to communicate more privately, securely, and efficiently. 

I went into “LinuxVM” to check the “Virtual network/subnet”, which was “Lab2Vnet/snet eastus-1”. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.1.png)

Then I went into “WindowsVM” to check the “Virtual network/subnet”, which was “Lab2Vnet/snet eastus-1”, showing they both have the same vnet. 

![image alt](https://github.com/brianknutson/azure-network-protocols/blob/550aa3850bd128f0b3ab9d6bdb60d8d4832a4fdb/3.2.png)


<h2>Actions and Observations</h2>

Step 1
------
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()
![image alt]()

