# WINDOWS SERVER HOME LAB 
## Description
This project is intended to set up an Windows Server home lab using Hyper-V as my virtualization tool. This lab simulates a corporate environment, so I need to set up two virtual machines: one of them running Microsoft Windows Server 2019 as my Domain Controller on which I’m installing Active Directory services, creating user accounts, configuring security groups and group policy objects, and setting a DHCP server. The other machine with Windows 10 installed to use it as my client computer. 

# Links 
•	How to set up Hyper-V in Windows: https://www.itechtics.com/enable-hyper-v-windows-10-home/#install-hyper-v-in-windows-10-home

•	Microsoft Windows server 2019: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019 

•	Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10 

# LAB WALK-THROUGH 
First I need to setup my two virtual machines in my Hyper-V enviroment. 

![Image](https://github.com/user-attachments/assets/ea885004-1506-4301-aa15-d006d8b4e3e4)

Then I need to create a virtual switch in Hyper-V, in this case it would be a private virtual switch so my two VMs can send data to each other. 

![Image](https://github.com/user-attachments/assets/f3d08ef6-19d7-4ad7-84f6-41764bcbb310)

After creating my switch, I need to change the network adapter on both my VMs so they can use the switch.  

![Image](https://github.com/user-attachments/assets/f12f1d83-9284-4755-aeec-c30569743771)

I then log into my windows server machine to assign it a static IP address,  in this case I am going to use the address 192.168.1.10 
![Image](https://github.com/user-attachments/assets/75277ff8-2e60-4157-93fa-ecf673836bb1)

Now I will install Active Directory Domain Services and set up my domain controller on the server.
https://github.com/user-attachments/assets/b8902106-df27-411b-b068-7b088c013858

After the domain controller is set, I will log into my client computer and set up an IP address that is part of our network 192.168.1.0/24, so it can communicate with the server. Then I will rename the computer to CLIENT01 and join it to our Domain. 
https://github.com/user-attachments/assets/639e239e-f9af-4c76-b5fc-b0887567efe1

Now I will make sure that my client can reach the server by pinging it and then double checking that my client computer was succesfully added to my domain. 
![Image](https://github.com/user-attachments/assets/71ce1c73-8500-4fa8-a935-0eb8fb7a0639)
![Image](https://github.com/user-attachments/assets/eb8bd37f-8729-4d84-a12c-0a5cfc2579ba)

Now I can start creating user accounts in my Active directory domain. Here I have two options:

First would be directly creating each user on the server
![Image](https://github.com/user-attachments/assets/0a2856a0-9ff2-4918-9006-08c611b85a3c)
![Image](https://github.com/user-attachments/assets/2bc2bcee-fba3-412e-96e2-38fa4a0fcb4a)

I can also create a script in powershell that will help me create users based on a list of full names that I have defined in a file 
![Image](https://github.com/user-attachments/assets/8207c2ef-5af6-4eae-b0e6-c7dbc19176d3)

I can check my domain controller server and see that all my users were created 
![Image](https://github.com/user-attachments/assets/691ec24b-de38-45b8-b44b-7afd0e799251)

Now I can try logging into my client computer using one of the accounts previously created 
![Image](https://github.com/user-attachments/assets/0e4f9f52-9e58-44ab-a88e-e584a563cf0b)

I will create three new organizational units to simulate three different departments in our domain and add some users to them 
![Image](https://github.com/user-attachments/assets/7812e30e-509d-41df-94b1-5f4d7cd3be56)

In the Tech Support OU I am going to create a security group so that I can add the users of that group to the Administrator group to grant admin rights to them so they can log into the domain controller and perform all task related to technical support.
![Image](https://github.com/user-attachments/assets/7a958d1a-3877-4cda-a127-86d1ff0ebb51)
![Image](https://github.com/user-attachments/assets/79c0d728-fe70-443a-904e-d2d2e24c2f94)
![Image](https://github.com/user-attachments/assets/3135fbe0-0175-46f3-bee9-a8daf8d18de1)

I am going to create a shared folder in my server for HR and Sales department 
![Image](https://github.com/user-attachments/assets/42d435f6-7636-43e3-a50b-a1fe7375008a)

Now I will assing permissions to that folder so that only users that are part of the specific department have access to the shared folder 
![Image](https://github.com/user-attachments/assets/658aa0b2-27d1-45b4-8d6a-9be5afbef359)
![Image](https://github.com/user-attachments/assets/1fa5b3a5-9be9-44a4-b854-2a77d933a4ca)

If I log in with one of the accounts from the HR users I can see that I can access the HR shared folder 
![Image](https://github.com/user-attachments/assets/20a92642-4290-4c53-bcc1-915d53b87b3a)
But if I try to access the Sales shared folder I wont let me 
![Image](https://github.com/user-attachments/assets/4324140c-98a2-48bc-bc78-6ae06bf63c2b)

I will create a Group Policy Object to map the shared folder to a network drive to the users
![Image](https://github.com/user-attachments/assets/c0ada600-4c33-4236-b813-7e02a0e8b11e)
Now when I log in If I log in with one of the accounts from the HR users I can see that I have the network drive mapped so users don't have to go to the shared path in the server to find it
![Image](https://github.com/user-attachments/assets/285860c2-5b13-4e01-a0b5-c8e32374e813)

I will do the same for the Sales department 
![image-24](https://github.com/user-attachments/assets/8e77c741-96a4-44ab-bf9a-93524c6244ec)

![Image](https://github.com/user-attachments/assets/bf5eb772-7935-4206-8ee2-f555295d4269)

At the beginning, I used a static ip address for our client computer. But now, I will configure the DHCP server in our Windows server so that computers connected to our local network can get their IP addresses from the server. 

First I will need to install the DHCP role in my server 
![Image](https://github.com/user-attachments/assets/c0dc8706-64c9-4375-aa87-2aeb59ad5c45)

after the installation is completed, I will create a new DCHP scope for the IP address range I want my computers to receive 
![Image](https://github.com/user-attachments/assets/6ca6b17e-3c8a-421c-8a5c-9279e8420cae)
I am also going to exclude the IP address that my Domain Controller is using, in this case 192.168.1.10, since it is part of that range.
![Image](https://github.com/user-attachments/assets/06b5a6cd-50c8-4b68-9403-41563beeac09)
Now I can see that I have the new scope created and active 
![Image](https://github.com/user-attachments/assets/f7936e9a-c2db-4897-a645-386f18985a05)

I can log into my client computer with an admin account and change the network adapter options from static IP to obtain IP address automatically
![Image](https://github.com/user-attachments/assets/3d351c64-c436-49e4-89e6-8648da2f971d)

I can now see that the IP address of my client computer change from 192.168.1.11 to 192.168.1.2 
![Image](https://github.com/user-attachments/assets/4f12b733-5a02-4db1-8eac-e0bf1ee51efc)
Also, on our DHCP options on our server we can see that DHCP lease with the same address for our client computer 
![Image](https://github.com/user-attachments/assets/d5a3c907-9c37-4665-963f-c8e3c2da05d9)
