# AD-DS-Home-Lab-Test-Draft
This repository contains the steps on how I set up a basic home lab running Active Directory.
[AD.DS.Walkthrough.md](https://github.com/user-attachments/files/27917355/AD.DS.Walkthrough.md)
# Active Directory Home Lab

## Intro

I wanted to familiarize myself with Windows Active Directory service to gain hands-on experience and prepare myself for real-life scenarios. First I followed the guided project *Administer Active Directory Domain Services* by Microsoft Learn, then I created my own lab to test my knowledge and eventually implement out other features as I continue my education. I learned a good bit about:

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
2. Created a NAT internal switch for communication between server and workstations.<img width="1920" height="1032" alt="image1" src="https://github.com/user-attachments/assets/d604f1c1-1e05-4630-87a8-bb29a6227303" />
3. Downloaded Windows Server 2022 ISO file.

### Domain Controller 

1. Created a new VM for the domain controller with Windows Server ISO file and connected it to the internal NAT switch.  
2. Created static IPv4 address in ethernet properties.<img width="1368" height="875" alt="image2" src="https://github.com/user-attachments/assets/818805af-3e6b-4276-ba17-1998e72ed1ed" />
3. Renamed domain controller to “COMPANY-DC01” in Settings and restarted the machine.  
4. In Server Manager’s Add Roles and Features menu, added Active Directory Domain Services role, confirmed installation selections, and restarted the machine.   
5. In the Server Manager menu, promoted the server to domain controller, added a new forest “company.internal,” configured DSRM password, chose default options, and restarted the machine. 

### Server Domain Member Server 

1. Created a new VM for the domain member server with Windows Server ISO file and connected it to the internal NAT switch.  
2. Created static, private IPv4 address in ethernet properties.<img width="1368" height="875" alt="image3" src="https://github.com/user-attachments/assets/d4a18156-293d-4314-83de-32869aaa882b" />
3. Renamed Domain Member Server to “SYSTEM-MBR01” in Settings and restarted the VM.  
4. Under the Local Server section in Server Manager, changed the domain to “COMPANY” in the System Properties menu.<img width="1368" height="875" alt="image4" src="https://github.com/user-attachments/assets/5f714eb7-5a4e-4d05-82bf-6856df9ccbbd" />
5. In Server Manager’s Add Roles and Features menu, added Active Directory Domain Services role, and confirmed installation selections.<img width="1250" height="1032" alt="image5" src="https://github.com/user-attachments/assets/ed751c2f-2096-4e9f-932b-a108d771d19b" />
6. In the Server Manager menu, promoted the server to domain controller, added domain controller to the existing domain (company.internal), configured DSRM password, selected default options, and restarted the machine. Promoting this server to a domain controller is important if the main domain controller is unavailable for any reason.   
7. Transferred the RID Master role from COMPANY-DC01 to COMPANY-MBR01 in Active Directory Users and Computers Operations Masters menu.<img width="960" height="1032" alt="image6" src="https://github.com/user-attachments/assets/db948d84-a969-47ad-a041-5f19a6e8fbfc" />

### Active Directory Site 

1. Created an Active Directory site on COMPANY-DC01 and a subnet for that site.<img width="960" height="1032" alt="image7" src="https://github.com/user-attachments/assets/9a0ea2d1-2b92-4421-89ea-a0c12beb40ff" />

### Organizational Units and Groups

1. In COMPANY-DC01, created Organizational Units “Cranberry” and “Washington” in Active Directory Users and Computers.<img width="960" height="1032" alt="image8" src="https://github.com/user-attachments/assets/b4f57de7-b613-496b-b060-949c6a32e426" />
2. Created “Cranberry Administrators” and “Washington Administrators” security groups in their respective OU’s with universal group scopes.  
3. Created users “CranberryContractor” and “WashingtonContractor,”and added them to their respective Administrative Groups, as well as a Protected User Group.<img width="960" height="1032" alt="image9" src="https://github.com/user-attachments/assets/29e445b9-1a62-4eab-b5e9-1283b3888c0b" />
4. Set user accounts “CranberryContractor” and “WashingtonContractor” to expire at the end of 2026\.<img width="960" height="1032" alt="image10" src="https://github.com/user-attachments/assets/9d3f4d7e-a025-47e4-ba03-d5883e0fbf8b" />
5. Configured city attributes to the CranberryContractor and WashingtonContractor user  accounts.<img width="960" height="1032" alt="image11" src="https://github.com/user-attachments/assets/01371057-1b02-454b-82c2-c7cd971e16d8" />
6. Used the Find Users, Contacts, and Groups console to verify the changes.<img width="1002" height="875" alt="image12" src="https://github.com/user-attachments/assets/00964b6c-2782-4092-897c-1a72111f9282" />

### Password Policy Configuration

1. Used the Delegate Control wizard to delegate the ability to reset passwords and force password changes to the Cranberry Administrators group in the Cranberry OU.<img width="960" height="1032" alt="image13" src="https://github.com/user-attachments/assets/8e9370dc-3972-43af-b346-1b63ce2e1cd6" />
2. Configured password domain policies using the Group Policy Management Editor console.<img width="960" height="1032" alt="image14" src="https://github.com/user-attachments/assets/b07fc564-594e-4b67-9d4e-e86fa8d62803" />
3. Configured Domain Admin Password Policy in the Active Directory Administrative Center.<img width="960" height="1032" alt="image15" src="https://github.com/user-attachments/assets/a719369b-6f6d-4b05-96a3-4731b63c005f" />
4. Enabled Active Directory Recycle bin.<img width="960" height="1032" alt="image16" src="https://github.com/user-attachments/assets/799e67ae-4084-4d02-887b-5b3b9e8a37d4" />

### Configure Security Settings

1. Restricted NTLM authentication in the Group Policy Management Editor console. Microsoft has deprecated NTLM authentication in favor of Kerberos due to severe vulnerabilities.<img width="960" height="1032" alt="image17" src="https://github.com/user-attachments/assets/20cdad96-29da-44d4-a5ed-bc8e11bbff90" />
2. Created GPOs “CranberryOUPolicy” and “WashingtonOUPolicy” and enabled auditing of User Account Management in the Cranberry OU and WashingtonOU.<img width="960" height="1032" alt="image18" src="https://github.com/user-attachments/assets/c6225688-98ec-42f7-a0d3-0025af4aebb1" />
3. In the same console, assigned the Cranberry Administrators and Washington Administrators to the “Deny log on as a service” policy to prevent users from running unauthorized processes.<img width="1002" height="875" alt="image19" src="https://github.com/user-attachments/assets/0a946ec4-dfd5-40d6-af9e-295ae35b43ae" />
