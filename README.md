
![image](https://github.com/user-attachments/assets/adf1be2e-acad-4d11-87ff-c27c555618bc)

# Preparing Active Directory Infrastructure in Azure
 
## Description
In this project I create two VMs (Virtual Machines), one running Windows Server, to act as a Domain Controller. The other VM will act as a client, running Windows 10 that will join the domain. In later projects, I will deploy Active Directory(AD), run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real-life IT environment!

## Environments and Utilities Used
* Microsoft Azure
* Virtual Machines
* Remote Desktop Connection

## Operating Systems Used
* Windows Server
* Windows 10

## Project Walk-through:

Navigate to Microsoft Azure and create a resource group:

![image](https://github.com/user-attachments/assets/5d9e56c4-4bf6-4ce9-958f-38621332118d)

Next, create a virtual network:

![image](https://github.com/user-attachments/assets/35dab5be-d2b4-437f-8a1b-f34c4571ceb6)

Once my resource group and network is created, I'll create and set up the virtual machine that will act as our Domain Controller. For the image, make sure you use Windows Server:

![image](https://github.com/user-attachments/assets/ca9c486b-6c47-4017-906d-b77a0c7f46c8)

![image](https://github.com/user-attachments/assets/78d3384d-22f9-4e31-a92a-2f443c656dbb)

In the Networking tab of this VM, I'll make sure it will create itself on the virtual network I just created. I'll leave all other settings default and create this VM:

![image](https://github.com/user-attachments/assets/ffd582ba-4c9e-4650-aac8-2626d407edb9)

Now, I'll create another VM that will serve as the client. The image for this machine should be Windows 10, NOT Windows Server like I did for the previous machine:

![image](https://github.com/user-attachments/assets/e07ccbd6-ff0d-4c47-a651-4836843a1fab)

In the Networking tab of this VM, I'll make sure it will create itself on the same virtual network as the previous machine created. I'll leave all other settings default and create this VM:

![image](https://github.com/user-attachments/assets/b955ea2a-215e-4eee-9146-27a11eef1e72)

I now need to set our DC (Domain Controller) private IP address to "static" as by default it is set to "dynamic". I want this to be static because this DC will double as a DNS (Domain Name System) server, which I will tell our client to use as a DNS server later. If the IP allocation setting were set to dynamic, the IP address could change leaving the DNS configuration of our client invalid. So, I'll go to the network settings of the DC and switch the IP allocation to static:

![image](https://github.com/user-attachments/assets/a7d14428-e491-438c-8efd-048e0eacfe5f)

![image](https://github.com/user-attachments/assets/227751e9-217a-4ca8-b00f-2ae6055e80cf)

![image](https://github.com/user-attachments/assets/238fa25b-274b-4830-ba56-cd46cfe0b1ff)

Next, I'll use Remote Desktop Connection to connect to the DC using its public IP and the login credentials I created when setting up this machine:

Once I'm logged in, the following screen will appear with the Server Manager open. (If this isn't what you're seeing and instead it a regular Windows desktop, you may have connected to the client VM instead or chose the wrong image when creating the DC):

![image](https://github.com/user-attachments/assets/aae19dbf-00e7-4b62-9650-dab2448258b8)

Next, I'm going to disable the firewall (you probably wouldn't do this in real lfe, but for the sake of this lab where nothing is at stake, I'll go ahead and do it). So, to disable the firewall I'll right click on the "Start" button and select "Run". Then type "wf.msc":

![image](https://github.com/user-attachments/assets/f9c9e22e-ee05-4210-9bc5-57ceb1a3abcd)

![image](https://github.com/user-attachments/assets/7e0525d4-bf9f-47e9-9e52-a5ce5d7e8e9f)

![image](https://github.com/user-attachments/assets/be32e342-f0aa-44b7-aac6-459af42c7705)

![image](https://github.com/user-attachments/assets/e49ad03c-cca0-41b6-b366-54be1afcf3a4)

You should see that all the firewall settings are disabled:

![image](https://github.com/user-attachments/assets/96ba94db-aaff-4895-8a2c-7e5e518cb61d)

Next, I need to configure our client's DNS settings to the DC. To start, back in Azure, I'll grab the DC-1 private IP address:

![image](https://github.com/user-attachments/assets/73ccc5a0-69ae-42f5-80a4-362235d337c6)

Then, I'll go to the network setting of the client machine. click on the NIC (Network Interface Card), go to settings, then DNS servers, and switch from "Inherit from the virtual network" to "Custom". Input the DC-1 private IP here and save:

![image](https://github.com/user-attachments/assets/46b69e56-7910-481c-a41a-1badab9a0669)

![image](https://github.com/user-attachments/assets/983eab39-3dab-4115-9ff1-907b893b457f)

After that's saved, I'll restart the client machine:

![image](https://github.com/user-attachments/assets/45289d42-b46d-4e25-a8a1-539eda58f665)

Once the machine has restarted, I'll use a Remote Desktop connection to connect to the client machine using its public IP and the login credentials I created while setting up this machine:

![image](https://github.com/user-attachments/assets/3b16e57e-bde4-4370-9c5b-088439850b2a)

Now that I'm logged in, I will open Powershell and attempt to ping the DC using the ping command and its private IP address. In my case, it'll look like this. (If there is an error and the connection timed out, double-check in Azure to make sure both of the machines are on the same virtual network. If they aren't this is likely causing the error and you'll need to set up the machine again on the same network):

![image](https://github.com/user-attachments/assets/4421597a-4823-4808-930c-1c341c0241d8)


While I'm here I can double check that the DNS server settings are pointing to the DC. I'll run "ipconfig /all" and look for the "DNS Servers" and it should point to our DC if everything is set up properly:

## ![image](https://github.com/user-attachments/assets/37237901-99ab-4a64-ad23-da9c0b3c8a66)

**Active Directory Infrastructure is Now Prepared!
We've successfully created two VMs (Virtual Machines), one running Windows Server, to act as a Domain Controller. The other VM as a client, running Windows 10. Don't forget: In later projects, I will deploy AD, run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real-life environment!**





