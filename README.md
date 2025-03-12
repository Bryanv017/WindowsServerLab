# WINDOWS SERVER HOME LAB 
## Description
This project is intended to set up an Windows Server home lab using Hyper-V as my virtualization tool. This lab simulates a corporate environment, so I need to set up two virtual machines: one of them running Microsoft Windows Server 2019 as my Domain Controller on which I’m installing Active Directory services, creating user accounts, configuring security groups and group policy objects, and setting a DHCP server. The other machine with Windows 10 installed to use it as my client computer. 

# Links 
•	How to set up Hyper-V in Windows: https://www.itechtics.com/enable-hyper-v-windows-10-home/#install-hyper-v-in-windows-10-home

•	Microsoft server 2019: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019 

•	Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10 

# LAB WALK-THROUGH 
First I need to setup my two virtual machines in my Hyper-V enviroment. 

![alt text](image.png)

Then I need to create a virtual switch in Hyper-V, in this case it would be a private virtual switch so my two VMs can send data to each other. 

![alt text](image-1.png)

After creating my switch, I need to change the network adapter on both my VMs so they can use the switch.  

![alt text](image-2.png)

I then log into my windows server machine to assign it a static IP address,  in this case I am going to use the address 192.168.1.10 
![alt text](image-3.png)

Now I will install Active Directory Domain Services and set up my domain controller on the server.
video1 

After the domain controller is set, I will log into my client computer and set up an IP address that is part of our network 192.168.1.0/24, so it can communicate with the server. Then I will rename the computer to CLIENT01 and join it to our Domain. 
video2

Now I will make sure that my client can reach the server by pinging it and then double checking that my client computer was succesfully added to my domain. 
![alt text](image-4.png)
![alt text](image-5.png)

Now I can start creating user accounts in my Active directory domain. Here I have two options:

First would be directly creating each user on the server
![alt text](image-6.png)
![alt text](image-7.png)

I can also create a script in powershell that will help me create users based on a list of full names that I have defined in a file 
![alt text](image-9.png)

I can check my domain controller server and see that all my users were created 
![alt text](image-10.png)

Now I can try logging into my client computer using one of the accounts previously created 
![alt text](image-11.png)

I will create three new organizational units to simulate three different departments in our domain and add some users to them 
![alt text](image-12.png)

In the Tech Support OU I am going to create a security group so that I can add the users of that group to the Administrator group to grant admin rights to them so they can log into the domain controller and perform all task related to technical support.
![alt text](image-13.png)
![alt text](image-14.png)
![alt text](image-30.png)

I am going to create a shared folder in my server for HR and Sales department 
![alt text](image-16.png)

Now I will assing permissions to that folder so that only users that are part of the specific department have access to the shared folder 
![alt text](image-17.png)
![alt text](image-18.png)

If I log in with one of the accounts from the HR users I can see that I can access the HR shared folder 
![alt text](image-19.png)
But if I try to access the Sales shared folder I wont let me 
![alt text](image-20.png)

I will create a Group Policy Object to map the shared folder to a network drive to the users
![alt text](image-22.png)
Now when I log in If I log in with one of the accounts from the HR users I can see that I have the network drive mapped so users don't have to go to the shared path in the server to find it
![alt text](image-23.png)

I will do the same for the Sales department 
![alt text](image-24.png)
![alt text](image-25.png)

At the beginning, I used a static ip address for our client computer. But now, I will configure the DHCP server in our Windows server so that computers connected to our local network can get their IP addresses from the server. 

First I will need to install the DHCP role in my server 
![alt text](image-26.png)

after the installation is completed, I will create a new DCHP scope for the IP address range I want my computers to receive 
![alt text](image-27.png)
I am also going to exclude the IP address that my Domain Controller is using, in this case 192.168.1.10, since it is part of that range.
![alt text](image-28.png)
Now I can see that I have the new scope created and active 
![alt text](image-29.png)

I can log into my client computer with an admin account and change the network adapter options from static IP to obtain IP address automatically
![alt text](image-31.png)
I can now see that the IP address of my client computer change from 192.168.1.11 to 192.168.1.2 
![alt text](image-32.png)
Also, on our DHCP options on our server we can see that DHCP lease with the same address for our client computer 
![alt text](image-33.png)