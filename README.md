<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Infrastructure in Azure (Virtual Machine) for Windows Users</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup a new forest in dc-1
- Create a Domain Admin user
- Join Client-1 to your domain

<h2>Setup a new forest in dc-1</h2>

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is), to do that we have to go back to dc-1 in the Server Manager, Dashboard click on the yellow sign , then click on Add a new forest, root domain name: mydomain.com
<p align left>
<img width="516" height="317" alt="Screenshot 2025-11-30 191926" src="https://github.com/user-attachments/assets/642bf157-b1a1-444d-ac7f-88a9b25a7bec" />

For the Directory Services Restore Password, we are not going to use this but just set the password as: Password1, click on Next
<p align left>
<img width="505" height="368" alt="Screenshot 2025-11-30 192633" src="https://github.com/user-attachments/assets/f557fd13-411a-41f3-aa13-a6314ec583d8" />

In DNS options, uncheck Create DNS delegation, click on Next
<p align left>
<img width="508" height="374" alt="Screenshot 2025-11-30 192908" src="https://github.com/user-attachments/assets/81458507-c7e0-45ed-9da0-44f25dbe4df5" />

Click Next on Additional Option
<p align left>
<img width="512" height="371" alt="Screenshot 2025-11-30 193148" src="https://github.com/user-attachments/assets/12797661-77ec-4122-a621-e1de3618e0b7" />

Click Next on Paths
<p align left>
<img width="505" height="373" alt="Screenshot 2025-11-30 193415" src="https://github.com/user-attachments/assets/4a4e850a-006e-4f74-9f32-7e3d3f2ddb08" />

Click Next on Review Options
<p align left>
<img width="501" height="373" alt="Screenshot 2025-11-30 193634" src="https://github.com/user-attachments/assets/6e35eb8f-2360-4658-a899-16248c0ee150" />

At this point just click on Install and wait until dc-1 turned into the domain controller
<p align left>
<img width="505" height="368" alt="Screenshot 2025-11-30 194121" src="https://github.com/user-attachments/assets/54eae63b-5f62-433e-a53c-4c5acf6d185f" />

Installation is in progress
<p align left>
<img width="511" height="374" alt="Screenshot 2025-11-30 194433" src="https://github.com/user-attachments/assets/82670e4e-08e3-4980-ad2a-559b27d01e4b" />

When this is installation has finished the connection will be restore, we have to log into dc-1 using mydomain.com]\labuser because now dc-1 has been configured as the domain controller
<p align left>
<img width="332" height="241" alt="image" src="https://github.com/user-attachments/assets/56dd8e64-a09c-4d5a-b4b8-3297ac93533d" />

<h2>Create a Domain Admin user</h2>

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. To do this in dc-1 click on Start, Administrative tools and then Active Directory Users and Computers
<p align left>
<img width="298" height="286" alt="Screenshot 2025-11-30 200027" src="https://github.com/user-attachments/assets/8093fa27-50b4-4231-9ed4-7a23fbc91ab5" />

On Active Directory User and Computers, right click on mydomain.com, click on New, 

<p align left>
<img width="404" height="362" alt="image" src="https://github.com/user-attachments/assets/ab53abea-e571-4808-9735-6ec0ef345300" />

Organizational Unit, type _EMPLOYEE
<p align left>
<img width="325" height="282" alt="image" src="https://github.com/user-attachments/assets/2d21c8b2-6397-4f38-833b-c42b986a9e1c" />

Create a new OU named “_ADMINS”
<p align left>
<img width="655" height="563" alt="image" src="https://github.com/user-attachments/assets/23d5a059-ca4b-4746-a77a-f3a11a0e95a2" />

Hit refresh on Active Directory Users and Computers and it will rearranged alphabetically 
<img width="885" height="463" alt="image" src="https://github.com/user-attachments/assets/19ec2526-2bd2-4a9a-a8fe-ba5fa22845a0" />

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! From the Admin folder, right click, click on New, then click on User 
<p align left>
<img width="392" height="333" alt="Screenshot 2025-11-30 202323" src="https://github.com/user-attachments/assets/d35e3d8b-0a29-480f-a1c4-a1d0e21ecf19" />

Uncheck User must change password at next logon and click on Password never expires
<p align left>
<img width="390" height="334" alt="Screenshot 2025-11-30 202649" src="https://github.com/user-attachments/assets/e3b04161-3ce4-4ddb-b378-a17e4d2a1930" />

When completed just click on Finish
<p align left>
<img width="387" height="329" alt="Screenshot 2025-11-30 202921" src="https://github.com/user-attachments/assets/78242686-78c8-4177-8171-fc7f492f6b6a" />

Add jane_admin to the “Domain Admins” Security Group, from the _ADMINS folder, right click on Jane Doe account, click on Properties, click on Member of, Click on Add, type Domain Admins, click on Check Names, click ok, click on Apply, and finally click Ok. Now Jane is an actual domain admin
<p align left>
<img width="490" height="368" alt="Screenshot 2025-11-30 203548" src="https://github.com/user-attachments/assets/e85fcd0c-5b39-40b1-838d-c4a14cf27877" />

Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”
<p align left>
<img width="335" height="236" alt="image" src="https://github.com/user-attachments/assets/4c7ccac1-f816-4851-9e34-48d5b38d56f1" />


<h2>Join Client-1 to your domain</h2>

Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
To get to this screen, right click on Start, click on System, then from the right menu click on Rename this PC (advanced), a new window will open from Computer Name tab, click on Change, a new window will open again this time click on member of and choose Domain (type mydomain.com) and then ok
<p align left>
<img width="515" height="275" alt="Screenshot 2025-11-30 205441" src="https://github.com/user-attachments/assets/0c019527-7545-4f36-97c4-42c626d7ac63" />
<img width="286" height="350" alt="Screenshot 2025-11-30 205948" src="https://github.com/user-attachments/assets/6b6d708b-9990-4278-953a-59fe4fe2658e" />

Use Jane's credentials to join the domain controller, 
<p align left>
<img width="408" height="259" alt="Screenshot 2025-11-30 210318" src="https://github.com/user-attachments/assets/3772e7c0-542f-45e7-8c24-3f8b5740b9b1" />

After this a new window will open welcoming to the mydomain.com, client-1 will restart to apply changes
<p aling left>
<img width="456" height="282" alt="image" src="https://github.com/user-attachments/assets/3bf499c4-6192-49af-9124-810a2ce064ba" />

Login to the Domain Controller and verify Client-1 shows up in ADUC. To verify this in dc-1, type in the search bar Active Directory Users and Computers or click on Start menu and click on Windows Administrative Tools and choose Active Directory Users and Computers.

From Active Directory Users and Computers, expand mydomain.com, click on Computers and you will be able to see that client-1 is there, right click on Properties to inspect more information
<p align left>
<img width="501" height="349" alt="Screenshot 2025-11-30 211555" src="https://github.com/user-attachments/assets/e5f42107-5e52-4476-8ff0-6464cbe6d614" />
# deploying-active-directory
