<p align="center">
<img src="[https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQE8PstcPGgZTuEczosANbV4nyHV3wakk2yB2XObdv0Dg&s=10](https://storage.ghost.io/c/e1/89/e189dda3-b4ad-485b-b170-49b8fbc25bd1/content/images/2025/04/azure-file-share.svg)"/>
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
Network file shares are basically servers assigning permission to the client computer with certain categories such as “Read & Write”, “Read Only”, and “No Access”. Administrators can assign permissions by individual or by groups. 
In this exercise, there will be a demonstration on what to assign for each group. Make sure to have DC-1 and Client-1 VMs from the previous labs turned on before starting.

Create Sample File Shares with Various Permissions
Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)
 
Connect/log into Client-1 as a normal user (mydomain\<base.case>)
 

On DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”
 
Set the following permissions (share the folder):

a.	Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
 

b.	Folder: “write-access”, Group: “Domain Users”, Permissions: “Read/Write”
 

c.	Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
 

Attempting to access files from a Client Account/Normal User
On Client-1, navigate to the shared folder (Follow these steps: start, run, \\dc-1)
 

Try to access the folders DC-1 just created. 
Which folders can Client-1 access? 
 
As shown above, Client-1 can access the read-only folder in the sharepoint. However, Client-1 can’t add new documents in read-only folder as shown below:
 

Client-1 can also access the write-access folder in the network
 

Which folders can Client-1 create stuff in?
Client-1 can only add new documents in the write-access folder as it’s authorized by the domain controller
 

This is logical since the domain controller admins are the ones who are in charge of giving permission to the clients on what they can or can’t access and if they have permissions to make edits or simply read the files in that particular folder.
For example, Client-1 can’t access the no-access folder as base.case isn’t a domain admin
 

Create an “ACCOUNTANTS” Security Group, assign permissions, and test access
Return to DC-1. Create new Organizational Unit within Active Directory Users and Computers called _GROUPS
 
Also within DC-1, in Active Directory, create a security group called “ACCOUNTANTS”
 
On the “accounting” folder you created earlier, set the following permissions:
a.	Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
 

Go over to the Client-1 VM. The accounting folder is now visible on Client-1
 

On Client-1, as base.case, try to access the “accountants” folder. It should fail.
 
 
Log out of Client-1 as base.case
 

On DC-1, make base.case a member of the “ACCOUNTANTS” Security Group
 

Sign back into Client-1 as base.case and try to access the “accounting” share in \\DC-1\ -. Does it work now?
 
This was the example of Client-1’s base.case accessing DC-1’s folder due to DC-1’s jane_admin granting permission. As the domain admins, they can also grant permission to multiple users to access select folders.

This officially concludes the Active Directory Lab. Be sure to delete the VMs and resource groups in Azure so charges don’t reoccur.
 



</p>
<br />
