# AD-DS-Home-Lab-Test-Draft
This repository contains the steps on how I set up a basic home lab running Active Directory.
[AD.DS.Walkthrough.md](https://github.com/user-attachments/files/27917355/AD.DS.Walkthrough.md)
# Active Directory Home Lab

## Intro

I wanted to familiarize myself with Windows Active Directory service to gain hands-on experience and prepare myself for real-life scenarios. First I followed the guided project *Administer Active Directory Domain Services* by Microsoft Learn, then I created my own lab to test my knowledge and eventually test out other features. I learned a good bit about:

* Using Server Manager in Windows Server  
* Creating and deploying domains  
* Configuring OUs, Groups, and GPOs   
* Establishing and enforcing passwords  
* Maintaining security of Active Directory

This project served to help me gain a deeper understanding of Active Directory. 

## Tools Used

Hyper-V  
Powershell

## Environments Used

Windows 10 Pro (64-bit)  
Windows Server 2022

## Walkthrough

### Initial Setup

1. Enabled Hyper-V on Windows 11\.  
2. Created a NAT internal switch for communication between server and workstations.![][image1]  
3. Downloaded Windows Server 2022 ISO file.

### Domain Controller 

1. Created a new VM for the domain controller with Windows Server ISO file and connected it to the internal NAT switch.  
2. Created static IPv4 address in ethernet properties.![][image2]  
3. Renamed domain controller to “COMPANY-DC01” in Settings and restarted the machine.  
4. In Server Manager’s Add Roles and Features menu, added Active Directory Domain Services role, confirmed installation selections, and restarted the machine.   
5. In the Server Manager menu, promoted the server to domain controller, added a new forest “company.internal,” configured DSRM password, chose default options, and restarted the machine. 

### Server Domain Member Server 

1. Created a new VM for the domain member server with Windows Server ISO file and connected it to the internal NAT switch.  
2. Created static, private IPv4 address in ethernet properties.  
   ![][image3]  
3. Renamed Domain Member Server to “SYSTEM-MBR01” in Settings and restarted the VM.  
4. Under the Local Server section in Server Manager, changed the domain to “COMPANY” in the System Properties menu.  
   ![][image4]  
5. In Server Manager’s Add Roles and Features menu, added Active Directory Domain Services role, and confirmed installation selections.  
   ![][image5]  
6. In the Server Manager menu, promoted the server to domain controller, added domain controller to the existing domain (company.internal), configured DSRM password, selected default options, and restarted the machine. Promoting this server to a domain controller is important if the main domain controller is unavailable for any reason.   
     
7. Transferred the RID Master role from COMPANY-DC01 to COMPANY-MBR01 in Active Directory Users and Computers Operations Masters menu. ![][image6]

### Active Directory Site 

1. Created an Active Directory site on COMPANY-DC01 and a subnet for that site.

### Organizational Units and Groups

1. In COMPANY-DC01, created Organizational Units “Cranberry” and “Washington” in Active Directory Users and Computers. ![][image7]  
2. Created “Cranberry Administrators” and “Washington Administrators” security groups in their respective OU’s with universal group scopes.  
3. Created users “CranberryContractor” and “WashingtonContractor,”and added them to their respective Administrative Groups, as well as a Protected User Group.![][image8]  
4. Set user accounts “CranberryContractor” and “WashingtonContractor” to expire at the end of 2026\.  
   ![][image9]  
5. Configured city attributes to the CranberryContractor and WashingtonContractor user  accounts.  
   ![][image10]  
6. Used the Find Users, Contacts, and Groups console to verify the changes. ![][image11]

### Password Policy Configuration

1. Used the Delegate Control wizard to delegate the ability to reset passwords and force password changes to the Cranberry Administrators group in the Cranberry OU.![][image12]  
2. Configured password domain policies using the Group Policy Management Editor console.

![][image13]

3. Configured Domain Admin Password Policy in the Active Directory Administrative Center.   
   ![][image14]  
     
4. Enabled Active Directory Recycle bin.![][image15]  
   

### Configure Security Settings

1. Restricted NTLM authentication in the Group Policy Management Editor console. Microsoft has deprecated NTLM authentication in favor of Kerberos due to severe vulnerabilities.![][image16]

2. Created GPOs “CranberryOUPolicy” and “WashingtonOUPolicy” and enabled auditing of User Account Management in the Cranberry OU and WashingtonOU.![][image17]  
3. In the same console, assigned the Cranberry Administrators and Washington Administrators to the “Deny log on as a service” policy to prevent users from running unauthorized processes.   
   ![][image18]
