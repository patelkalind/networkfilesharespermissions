<p align="center">
<img src="https://myworldofit.net/wp-content/uploads/2014/02/webdav-part1.png"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) - Part 3 (Network File Shares and Permissions)</h1>
This tutorial outlines the implementation of file sharing between administrators and clients within the Active Directory Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Recap</h2>

In Part 2, we configured DNS using the DC-1 and Client-1 VMs from the first part of the Active Directory lab, where we showed how to change a DNS address and how to ping and flush the DNS cache using PowerShell. 

This officially concludes the Active Directory series on Network File Shares and Permissions.

<h2>Deployment and Configuration Steps</h2>

Network file shares are servers that assign permissions to client computers in categories such as “Read & Write”, “Read Only”, and “No Access”. Administrators can assign permissions to individuals or groups. 

In this exercise, we will demonstrate what to assign for each group. Make sure DC-1 and Client-1 VMs from the previous labs are turned on before starting.

*Create Sample File Shares with Various Permissions*

Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)


<img width="713" height="578" alt="image" src="https://github.com/user-attachments/assets/4f1963e1-8fef-4615-855c-e0a06983c52d" />


 
Connect/log into Client-1 as a normal user (mydomain\<base.case>)


<img width="713" height="394" alt="image" src="https://github.com/user-attachments/assets/ba850074-1d75-4fdb-8c2d-bf73b92d6c32" />
 

On DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”


<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/ae49fa30-c573-469e-afca-37bf5ef31ce9" />


 
Set the following permissions (share the folder):

a.	Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
 
<img width="885" height="741" alt="image" src="https://github.com/user-attachments/assets/659ecbf3-750c-43f8-baf0-b7063c602fcd" />



b.	Folder: “write-access”, Group: “Domain Users”, Permissions: “Read/Write”
 
<img width="885" height="741" alt="image" src="https://github.com/user-attachments/assets/78a4c1ef-de1f-4d54-a8e6-5342e7d31112" />


c.	Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
 
<img width="885" height="741" alt="image" src="https://github.com/user-attachments/assets/34ace6f7-cf0a-42d1-9510-0d365e429600" />


*Attempting to access files from a Client Account/Normal User*

On Client-1, navigate to the shared folder (Follow these steps: Start, Run, \\dc-1)
 
<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/c5d4f6f4-42cb-40cf-a8e3-bb53861f017f" />



Try to access the folders DC-1 just created. 


Which folders can Client-1 access? 


<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/7005d95d-8998-47c6-b61b-e4c516700224" />


 
As shown above, Client-1 can access the read-only folder in SharePoint. However, Client-1 can’t add new documents in the read-only folder, as shown below:
 
<img width="688" height="349" alt="image" src="https://github.com/user-attachments/assets/59926b95-bfb4-445d-821c-0338a3f94f0b" />



Client-1 can also access the write-access folder on the network
 
<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/a5681399-3a7a-433a-9a53-29fd399f6f00" />



Which folders can Client-1 create stuff in?

Client-1 can only add new documents in the write-access folder, as it’s authorized by the domain controller
 
<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/a7e72f5a-ec73-4cd7-9747-5e0aa8d53898" />



This is logical since domain controller admins are in charge of giving clients permission to access what they can and can’t, and whether they can edit files or simply read them in that particular folder.

For example, Client-1 can’t access the no-access folder as base.case isn’t a domain admin
 
<img width="813" height="311" alt="image" src="https://github.com/user-attachments/assets/6fe75dd8-52d1-4b9c-a6f1-a8f27647019b" />


*Create an “ACCOUNTANTS” Security Group, assign permissions, and test access*


Return to DC-1. Create a new Organizational Unit within Active Directory Users and Computers called _GROUPS


<img width="683" height="590" alt="image" src="https://github.com/user-attachments/assets/0c2b3b34-56fe-4bf6-a080-0655a86b829b" />


 
Also within DC-1, in Active Directory, create a security group called “ACCOUNTANTS”


<img width="683" height="634" alt="image" src="https://github.com/user-attachments/assets/e1f7c321-c45c-4001-9cff-68054be30013" />


 
On the “accounting” folder you created earlier, set the following permissions:

a.	Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
 
<img width="885" height="741" alt="image" src="https://github.com/user-attachments/assets/c060d9a5-c29b-4f75-a7d6-4d839f15cd44" />



Go over to the Client-1 VM. The accounting folder is now visible on Client-1
 
<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/5083399c-0b41-4540-a7bc-70666707ac3d" />


On Client-1, as base.case, try to access the “accountants” folder. It should fail.
 
<img width="813" height="311" alt="image" src="https://github.com/user-attachments/assets/79ce1992-88ef-467c-bf4b-1851277ee76a" />


 
Log out of Client-1 as base.case
 
<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/c04fd374-4530-4053-b35f-362127216807" />


On DC-1, make base.case a member of the “ACCOUNTANTS” Security Group
 

<img width="714" height="393" alt="image" src="https://github.com/user-attachments/assets/c32738ae-aa5f-4e72-8190-0b624ed3025d" />


Sign back into Client-1 as base.case and try to access the “accounting” share in \\ DC-1 \. Does it work now?


<img width="974" height="513" alt="image" src="https://github.com/user-attachments/assets/8839ed6b-421e-4e78-9e89-29a36808099d" />


 
This was the example of Client-1’s base.case accessing DC-1’s folder due to DC-1’s jane_admin granting permission. As the domain admins, they can also grant permission to multiple users to access select folders.




This officially concludes the Active Directory Lab. Be sure to delete the VMs and resource groups in Azure so charges don’t recur.
 
<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/130e5e22-8a80-4d68-814f-cdd6ece5abef" />





</p>
<br />
