# Introduction

In this Particular homelab, we'll be installing a server and client VM, setting up Active Directory, creating Organizational Units (OUs), groups, and users, and implementing a Group Policy Object (GPO). Learning Active Directory is really important for anyone in IT because it lays the groundwork for understanding Windows-based networks and provides skills for managing users and computers effectively. By learning Active Directory, we can implement security measures, simplify user onboarding, and manage roles within an organization, while ensuring that configurations remain consistent through GPOs.

## Sections:
1. Installing the Prerequisites
2. Installing the Client VM (Windows 10 Enterprise)
3. Installing the Server VM (Windows Server 2022)
4. Setting Up the Server
5. Assigning a Static IP Address to the Server
6. Creating Organizational Units (OUs), Groups, and Users
7. Joining a Client to the Domain
8. Creating and Implementing a Group Policy Object (GPO)

## Section 1: Installing the Prerequisites
-  Oracle VirtualBox will be used as the hypervisor for this lab, but any other hypervisor can be used (e.g VMWare Workstation Pro)
-  You can dowload it here: https://www.virtualbox.org/wiki/Downloads  
- The following ISOs are required:  
- Windows 10 Enterprise ISO:  
	- Download here: [https://www.microsoft.com/en-us/software-download/windows10ISO)  
- Windows Server 2022 ISO:  
	- Download here: https://www.microsoft.com/en-US/evalcenter/evaluate-windows-server-2022](https://www.microsoft.com/en-US/evalcenter/evaluate-windows-server-2022)

## Section 2: Installing the Client VM (Windows 10 Enterprise)
**2.1** Open your selected hypervisor and create a New Virtual Machine.  
<img width="2560" height="1440" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/717bc87b-1bee-460c-b7f2-6c73ace6752d" />


**2.2** In the New Virtual Machine Wizard, Name your Virtual machine in this instance we will use "Windows 10 Client"  
<img width="2560" height="1440" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/c0bc2c7f-3244-4e2b-b45f-bfc5d0b641b8" />



**2.3** Next we will Select the ISO from your previously downloaded files.)  
<img width="2560" height="1440" alt="Screenshot (25)" src="https://github.com/user-attachments/assets/1ace690a-768f-4415-8e1c-4eb2a880a7c5" />

**2.4** Choose Windows 10 x64 as the operating system version.  
<img width="2560" height="1440" alt="Screenshot (26)" src="https://github.com/user-attachments/assets/64614f74-2de7-4fc7-8f68-58c519259616" />


**2.5** Choose hardware parameters for your Virtualm Machine you can increase processors and base memory if you know your system details and capabilites if not leave it at 1 Processor and 2GB of ram 
<img width="2560" height="1440" alt="Screenshot (28)" src="https://github.com/user-attachments/assets/1730a4b4-72bb-41f0-b09b-deab53d6f374" />



**2.6** Set Your Maximum Disk Capacity   
- I set mine at 50 GB, you can set yours higher or lower to whatever you would like.  
- Do not set it too low though because the OS will not boot then.
<img width="2560" height="1440" alt="Screenshot (29)" src="https://github.com/user-attachments/assets/b246627f-948d-4bef-89e2-594f18e195ea" />


**2.7** After the VM is created ,
- You should be greeted with this screen:
  <img width="2560" height="1440" alt="Screenshot (30)" src="https://github.com/user-attachments/assets/b1b577b5-11c6-4bdd-a494-fe6a1fd0123c" />
**2.8** Power on the VM.   
- You should see this as an example:
<img width="2560" height="1440" alt="Screenshot (33)" src="https://github.com/user-attachments/assets/44afba4d-56fe-48de-a4f9-86ba94c0b7a4" />




**2.9**Set the language, keyboard layout, and time zone as needed. Select Custom: Install Windows Only (Advanced) for the installation type
<img width="2560" height="1440" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/62658dbd-38c8-49a4-a0f1-b6ae7f6ce9fb" />



**2.10** Select the disk partition and click Next to start the installation.  
<img width="2560" height="1440" alt="Screenshot (36)" src="https://github.com/user-attachments/assets/94a01d78-c695-4652-9518-3200454fc6f2" />


**2.11** After installation, select Domain Join instead and create a local user account.  
- Domain Join is at the bottom-left corner of this screen.

**2.12** Complete the remaining setup options. You can turn off these non-essential settings for this lab.  

Congratulations! You have successfully installed the Client VM.

## Section 3: Installing the Server VM (Windows Server 2022)
- Installing the Server VM will follow the same steps as installing the Client VM.  
- This section will highlight the differences in the installation process.  
- If needed, refer back to Section 2.

**3.1** Create a new virtual machine as outlined in Section 2, but select Windows Server 2022 as the guest operating system.  
<img width="2560" height="1440" alt="Screenshot (40)" src="https://github.com/user-attachments/assets/40581632-6599-4d06-aef0-f64702cf1b6e" />


**3.2** Set the Maximum Disk Capacity.  
- Be sure to not set this too low especially for a server as it will not run/boot properly.  
- I set mine at 20 GB for future use.
<img width="2560" height="1440" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/b332d3b5-5ab8-4692-853a-b8c4d390b906" />


**3.3** Choose the Windows Server 2022 ISO in the VM’s  settings before powering on the server. (Be sure to select Desktop version for a graphical userface) 
<img width="2560" height="1440" alt="Screenshot (42)" src="https://github.com/user-attachments/assets/bfc9618d-b5e1-4bda-8156-06d28e5d6b2d" />


**3.4**Power on the VM and choose Windows Server 2022 Standard Evaluation (Desktop Experience) to install the version with a GUI.

The other version does NOT include a GUI and is only command-line based.

<img width="551" height="410" alt="3 5" src="https://github.com/user-attachments/assets/7dac5dc5-b32c-4855-a084-7aa89e4e8819" />


**3.5** Set up the Administrator password as prompted during the installation.  
<img width="2560" height="1440" alt="Screenshot (48)" src="https://github.com/user-attachments/assets/37a28a1f-14a1-4f48-ae2a-d8becdbc9955" />

Congratulations! You have successfully installed the Server VM.

## Section 4: Setting Up the Server
**4.1** After logging into the server, open Server Manager and select Add Roles and Features.  
<img width="2560" height="1440" alt="Screenshot (49)" src="https://github.com/user-attachments/assets/d101cc2a-9891-431f-a997-0f38c692b6b4" />


**4.2** Leave the defaults selected until you reach the Server Roles page.  
<img width="781" height="514" alt="4 2" src="https://github.com/user-attachments/assets/8a558b02-382b-44be-8b9e-2343db939a4a" />


**4.3** Checkbox Active Directory Domain Services.  
- You can select DHCP and DNS Server for later use.  
<img width="2560" height="1440" alt="Screenshot (50)" src="https://github.com/user-attachments/assets/76c04757-2f8d-4bd1-91d1-d0f661005702" />


**4.4** Continue through the feature options, then click Install at the confirmation window.  
<img width="2560" height="1440" alt="Screenshot (51)" src="https://github.com/user-attachments/assets/b0bcf2ad-f913-4ed6-8bc4-cfeb634fe812" />
<img width="2560" height="1440" alt="Screenshot (52)" src="https://github.com/user-attachments/assets/d2125a41-1a4e-41e7-8183-a9a669a61051" />



**4.5** After installation, select the flag icon in the Server Manager Dashboard and click Promote this server to a domain controller.  
<img width="2560" height="1440" alt="Screenshot (53)" src="https://github.com/user-attachments/assets/8aa94965-2f55-45cd-98f0-e4c499bc6fe3" />


**4.6** Choose Add a new forest and enter a Root domain name (e.g., practice.local). 

<img width="543" height="404" alt="4 6" src="https://github.com/user-attachments/assets/4fd7d838-7288-4bbd-9b9b-ccef573720b4" />


**4.7** Enter a DSRM password on the Domain Controller Options page and leave other options as default.  
<img width="776" height="522" alt="4 7000" src="https://github.com/user-attachments/assets/3510fc01-a1b6-40cf-a254-9aae46322ba5" />



**4.8** Verify that the NetBIOS domain name matches your chosen domain name in all caps (e.g., PRACTICE).  
<img width="569" height="386" alt="4 8" src="https://github.com/user-attachments/assets/dea9edc5-015c-42f7-976b-f13d60fadf64" />


**4.9** Go through the next few pages, leave the default options. Complete the setup by clicking Install on the Prerequisite Check page.  
<img width="780" height="520" alt="909090" src="https://github.com/user-attachments/assets/80f615de-4822-4a66-9d73-05e2fbf7a97f" />


Congratulations! You have successfully set up the Server and Domain Controller.

## Section 5: Assigning a Static IP Address to the Server
- Assigning a static IP address to a server is best practice and important for consistent access and easy troubleshooting.

**5.1** Open the Command Prompt in the Server VM and type `ipconfig` to note the IPv4 Address, Subnet Mask, and Default Gateway.  
<img width="2560" height="1440" alt="Screenshot (57)" src="https://github.com/user-attachments/assets/7755840d-7e1b-46d5-9f47-e62d94bf2400" />


**5.2** Open Ethernet Settings by typing Ethernet in the search bar, then go to Change adapter options.

**5.3** Right-click the Ethernet adapter and select Properties. 
<img width="2560" height="1440" alt="Screenshot (60)" src="https://github.com/user-attachments/assets/4a07b941-4be5-4141-afc7-1f6f3b944a5d" />


**5.4** Choose IPv4 Properties, select Use the following IP address, and enter your IP Address, Subnet Mask, and Default Gateway.  
<img width="364" height="411" alt="ipv4" src="https://github.com/user-attachments/assets/fb288446-9251-4b76-b4ee-2e0d08ebde48" />


Congratulations! You have successfully assigned a static IP address to your server.

## Section 6: Creating Organizational Units (OUs), Groups, and Users
- **Organizational Units (OUs)** are containers that can hold users, groups, computers, and other OUs. They serve as a way to structure and organize directory objects.  
- **Groups** are collections of user accounts, computer accounts, or other groups that simplify the management of permissions and access to various resources of the organization.  
- **Users** are individual accounts that represent people or services. Each user account has unique credentials (username and password).  
- For this lab, I will be going off a naming system based on a common business-environment.  
	- You can name your OUs, Groups, and Users anything you wish. It is best to choose a naming pattern that makes sense to you logically.  
		- For example: OU = DC, Group = Justice League, User = Superman.

**6.1** Open Server Manager and select Tools > Active Directory Users and Computers.  
<img width="2560" height="1440" alt="Screenshot (61)" src="https://github.com/user-attachments/assets/9f795fa1-54f3-40de-9405-bc40bd4210bd" />


**6.2** Right-click your domain (e.g., practice.local) and select New > Organizational Unit.  
<img width="2560" height="1440" alt="Screenshot (62)" src="https://github.com/user-attachments/assets/30f030c7-17ff-40d9-9675-9e1354646f6b" />


**6.3** Create OUs for locations like London, Norwhich , and Cambridge. 
<img width="2560" height="1440" alt="Screenshot (63)" src="https://github.com/user-attachments/assets/0a254945-6486-43a9-8d0d-a4ea4830482c" />

**6.4** Create sub-OUs named Computers, Servers, and Users within each location OU.
<img width="2560" height="1440" alt="Screenshot (65)" src="https://github.com/user-attachments/assets/ad737111-167e-48d3-a00f-b620b57aecc6" />

**6.5** Create a new group within the Users sub-OU (e.g., IT).  
- You will leave the Group Scope and Group Type as the defaults.  
- Just an overview of what these are:  
- Scopes-  
	- **Domain Loca**l Scope is restricted to the domain in which they are created.  
	- **Global** Scope is limited to the domain they are created but can be used across the entire forest.  
	- **Universal** Scope can be used across all domains within the forest.  
- Group Types-  
	- **Security** **Groups** are used to assign permissions to shared resources, allowing members to access specific files or applications.  
	- **Distribution Groups** are usually used for email distribution lists. They do not have security permissions.
<img width="2560" height="1440" alt="Screenshot (66)" src="https://github.com/user-attachments/assets/a183eebf-89c3-4ae8-a6c1-24b0b4230ece" />



**6.6** Create new users within the Users sub-OU (e.g., Jacob Jones).  
<img width="2560" height="1440" alt="Screenshot (67)" src="https://github.com/user-attachments/assets/36d676a3-2b0d-42f9-a1d6-f16cdab38e07" />

Congratulations! You have successfully created OUs, Groups, and Users.

## Section 7: Joining a Client to the Domain

**7.1** Log into the client VM. Go to Settings > About > Rename this PC (Advanced). 
<img width="2560" height="1440" alt="Screenshot (74)" src="https://github.com/user-attachments/assets/c5b699b3-4f20-486d-a96f-591b6e5d30f4" />

**7.2** In the System Properties window, click Change and select Domain. Enter your domain name (e.g., practice).  
- You may need to type in your domain name with ‘.local’ at the end if it fails to connect.
<img width="2560" height="1440" alt="Screenshot (75)" src="https://github.com/user-attachments/assets/3c84614c-f98d-45a2-9d10-a018bcc74cbf" />

**7.3** Enter the Administrator credentials when prompted. 
<img width="777" height="463" alt="7 2_2" src="https://github.com/user-attachments/assets/c722423e-8bd9-46f8-adf6-e3ec2455c5ac" />

**7.4** Restart the client VM to apply the changes.  

**7.5** Log in as a user created in Section 6 (“Jacob Jones”) to verify connectivity to the domain.  
- You should be able to login successfully as the new user you’ve created.  

- The following steps are optional but recommended for best practices to ensure that the computer objects are placed in the appropriate OU.  
**7.6** Switch to the Server VM and Open Server Manager. Select Tools > Active Directory Users and Computers.  
**7.7** Select .local > Computers. Right click the computer that was renamed and select Move.  
<img width="777" height="463" alt="7 2_2" src="https://github.com/user-attachments/assets/d1d2bb97-b356-432f-a4d9-a2ee9c7fddd1" />


**7.8** Select the sub-OU Computers of the New York OU.  
<img width="777" height="463" alt="7 2_2" src="https://github.com/user-attachments/assets/0c9dd263-7ddd-4504-b7e2-52abe95754f6" />


Congratulations! The client VM is now joined to the domain.

## Section 8: Creating and Implementing a Group Policy Object (GPO)
- A **group policy object** can be thought of as a set of rules or setting that can be applied to groups of users/computers in active directory.  
- The group policy object that I will be creating will disable the Command Prompt for users in the New York OU.

**8.1** Open Server Manager on the Server VM and select Tools > Group Policy Management. 

**8.2** Right-click your domain (e.g., practice.local) and select Create a GPO in this domain and link it here.  
<img width="690" height="483" alt="8 2" src="https://github.com/user-attachments/assets/641b5bfb-d5d2-4854-9b05-b73803b2941b" />

**8.3** Name the GPO (e.g., Disable Command Prompt).  

**8.4** Right-click the GPO and select Edit. Navigate to User Configuration > Administrative Templates > System > Prevent access to the command prompt.  
<img width="853" height="521" alt="8 4" src="https://github.com/user-attachments/assets/dbc1e43e-64f9-4ab2-9587-4f9cfb384618" />


**8.5** Enable the policy to disable the Command Prompt for users.  

**8.6** Link the GPO to the Users OU in New York.  
- This is done by dragging the ‘Disable Command Prompt’ from the ‘Group Policy Objects’ to the ‘Users’ sub-OU.  
- This GPO is placed in the Users sub-OU because it specifically impacts individual users.
<img width="770" height="535" alt="8 6" src="https://github.com/user-attachments/assets/3ebb9523-6227-4f5a-9441-570c7b4ef3cc" />


**8.7** On the server, run `gpupdate` in Command Prompt to apply the GPO changes.  
- This can also be done by restarting the Server.

**8.8** Test the policy on the client VM by trying to open the Command Prompt.  
- You should see the message as shown below.
<img width="768" height="252" alt="8 8" src="https://github.com/user-attachments/assets/fbf4b90a-0f9f-45c7-bea6-ff2c15a211bc" />


Congratulations! You have successfully created and applied a GPO.

# Conclusion

In this lab, we successfully installed a server VM and a client VM, set up a server for Active Directory, created OUs, made a group and a user and implemented a GPO. 
