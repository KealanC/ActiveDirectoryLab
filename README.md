# ActiveDirectoryLab

<h2>Description</h2>
For this project my objective was to create an active directory enviroment in a Windows 10 VM and then utilize a powershell script to add 1000 user accounts to the active directory. This was done by configuring a Windows 19 server as a domain controller to facilitate the conection of two network adapters, one which connected to the internet and the other acting as an internal adapter. This will allow the created client to log in by using the created username and password and then be assigned an IP address from the DHCP server created allowing the to connect to the internet via the domain which was also created.

<h2>Utilities Used</h2>

- <b>Windows 10 ISO</b> 
- <b>Windows 19 Server ISO</b>
- <b>PowerShell<b>
- <b>VirtualBox<b>

<h2>Environments Used </h2>

- <b>Windows 10 OS</b>
- <b>Oracle VM VirtualBox<b>

<h2>Program walk-through:</h2>

<p align="center">
The first step I took in this project was to download the relevant files. For this lab it requires the download of two ISO files the first being the Windows 19 Server file and secondly the Windows 10 file, these files will both be used in Oracle VM VirtualBox to created the teo required VM's. The first file which we will be utilizing is the server file, before we start the VM with this file we must configure the adapters as for this project we need two. One adapter will be used to connect to the internet via a NAT connection and the second adapter will be used as an internal network connection. These options must be selected underneath the Network settings of the VM: <br/>
<img width="862" alt="Screenshot 2024-09-08 at 15 36 47" src="https://github.com/user-attachments/assets/63386486-45de-4093-8e91-5feaa45aeee6">
<br />
<br />
Next, we will launch the Windows 19 Server VM which will act as the domain controller. Once successfully set up, navigate to the newtork settings and select the ethernet tab. From there select the 'Change Adapter Settings' and determine which adapter is the one configured earlier to be connected to the internet and which is the internal one. From there rename the adapters appropriately to make it easier to identify later on: <br/>
<img width="1539" alt="Screenshot 2024-09-08 at 15 39 44" src="https://github.com/user-attachments/assets/a33d2e2c-659c-4590-ba1a-b38b9f825e35">
<br />
<br />
Now to configure the IPV4 internal adapter, the details I used are outlined below:  <br/>
<img width="1539" alt="Screenshot 2024-09-08 at 15 44 20" src="https://github.com/user-attachments/assets/e22a7d89-f12b-4591-9203-9909dbc6330c">
<br />
<br />
Next I installed Active Directory domain services and created a domain called mydomain.com. This was done by selecting add roles amd features from the server manager and selecting to add Active Direotory Domain Services to my selected server.: <br/>
<img width="1539" alt="Screenshot 2024-09-08 at 15 47 05" src="https://github.com/user-attachments/assets/6d0ddaff-eea9-4bea-ad5c-722ef815e21a">
<br />
<br />
Following the installation we will be met with a notification to configure the domain. This is where we will also select a name fo rthe domain:  <br/>
<img width="1539" alt="Screenshot 2024-09-08 at 15 52 22" src="https://github.com/user-attachments/assets/c99cf5ed-e71d-4120-a13f-2a3d788f98d6">
<img width="1539" alt="Screenshot 2024-09-08 at 15 53 06" src="https://github.com/user-attachments/assets/0341c516-d450-4921-bd13-e8d21a2a63a0">
<br />
<br />
Now I will create a designated admin account in Active Directory. To do this I first created an ADMINS organizational unit and created an admin account under the unit with my own name:  <br/>
<img width="1545" alt="Screenshot 2024-09-09 at 20 08 09" src="https://github.com/user-attachments/assets/c41f9f2d-5315-449f-98e6-e756fbe6ca2e">
<img width="1545" alt="Screenshot 2024-09-09 at 20 11 22" src="https://github.com/user-attachments/assets/185bbb89-8606-4efc-8bec-ce890ad87365">
<br />
<br />
Now I will install Remote Access Server/Network Address Translation to the domain controller to allow the Windows 10 client we will later create to have access to the internet through the domain controller. This is also done through the add roles and features option from the server manager where this time the box 'Remote Access' must be checked to add features. It is also important to ensure that the routing option is selected before installation. Frome there select Routing and Remote access from server manager tools to configure the NAT connection to the network connection which we specified earlier that is connected to the internet:  <br/>
<img width="1545" alt="Screenshot 2024-09-09 at 21 33 30" src="https://github.com/user-attachments/assets/7911a0b8-7e34-4998-bfe5-85e58c4c484c">
<img width="1545" alt="Screenshot 2024-09-09 at 21 34 53" src="https://github.com/user-attachments/assets/23e422c6-6f13-44ff-8477-fad57a5c3cd6">
<br />
<br />
Now to set up a DHCP server on the domain controller to allow the client to be assigned an IP address when they connect. Once again from the server manager select to add roles and features and select the DHCP server option and then install. Once installed, open the DHCP server and configure a new IPV4 scope for the ip range 172.16.0.100-200 and configure the router for the clients to assign the IP address of the domain controller (172.16.0.1) which will act as the DNS server:  <br/>
<img width="1501" alt="Screenshot 2024-09-09 at 21 45 42" src="https://github.com/user-attachments/assets/d93c76b2-c272-49c4-b575-65a95fce648e">
<img width="1545" alt="Screenshot 2024-09-09 at 21 50 35" src="https://github.com/user-attachments/assets/5dbab3c5-e994-451d-8511-2c9e23d0e8f4">
<br />
<br />
Next we will utilise a PowerShell script to add 1000 users to the active directory which we have created and configured. This is done by having a text file created with one persons first and second names per line separated by a space. In this example the text file is saved as names.txt in a file named AD-PS-master located on the desktop. To run the code to create the user accounts we must open Windows PowerShell ISE and run the command 'Set-ExecutionPolicy Unrestricted' and then navigate to the AD-PS-master file. Then open the PowerShell script in the terminal and run the script and watch as the user accounts are created: <br />
<img width="1544" alt="Screenshot 2024-09-11 at 20 35 08" src="https://github.com/user-attachments/assets/20e56c10-347e-4ca0-bdd9-323b59250ae8">
<br />
<br />
Once the accounts are created it is time to test them on a separate client to see if we are able to log in to the domain which we have created and successfully connect to the internet. To do this we must now set up another VM using the Windows 10 client ISO we downloaded earlier. The imortant part here is to make sure that the network setting for this client is set to use an internal network adapter so it can get a DHCP address from the domain controller. Once the client is set up and logged in we will run the ipconfig command in the command terminal to see if the internet connection to our server is working, as we can see from the IP information below it is the same as configured on the domain controller. Now to check if the user accounts are working, to do this select a username and password from the text file used to create the users and choose to log out and switch users on the client, using the selected name and password try to log in and should everything be correctly configured it will sucessfully log in to the account on the mydomain.com server: <br />
<img width="624" alt="Screenshot 2024-09-11 at 22 48 32" src="https://github.com/user-attachments/assets/d2a46c5d-fd28-4922-8eba-076e70a90607">
<img width="624" alt="Screenshot 2024-09-11 at 22 54 44" src="https://github.com/user-attachments/assets/dd97d11c-c0f6-41ce-9738-b2b1c0fb878a">
<img width="624" alt="Screenshot 2024-09-11 at 22 59 42" src="https://github.com/user-attachments/assets/87b81771-43ae-4b54-94e7-91b049d1e0a2">
<img width="624" alt="Screenshot 2024-09-11 at 23 03 05" src="https://github.com/user-attachments/assets/4751051e-fc44-4de5-9e14-c1c314b55fad">






</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
