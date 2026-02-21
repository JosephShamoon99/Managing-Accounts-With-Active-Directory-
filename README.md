<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Creating Dummy Accocunts</h2> 

<p> 
<img width="1227" height="950" alt="Snapshot 41" src="https://github.com/user-attachments/assets/08c71728-bdf1-4096-8e07-aac5106dcdba" /> 
<img width="1213" height="943" alt="Snapshot 42" src="https://github.com/user-attachments/assets/129feb0b-d5aa-4612-9839-a7d91ca17005" /> 
<img width="542" height="427" alt="Snapshot 43" src="https://github.com/user-attachments/assets/a5a5de79-3ebb-44a7-ab3e-0fb1b9f5ead1" /> 
<img width="470" height="388" alt="Snapshot 44" src="https://github.com/user-attachments/assets/1ecdf3d1-0b1f-47b5-895f-e2933d593f26" />
</p>
<p>
The same way, we need to allow Jane Doe to login remotely to the domain controller, we need 
to allow normal users the ability to login to client 1. The first thing that we will 
do is sign into client 1 with the Jane Doe account. Go to system > properties. Then select remote desktop and then remote desktop users. Client 1 is the context of our domain, so we can access groups in our domain. We want any user in our domain to be able to login to client 1 remotely, so select Domain users. 
</p>
<br />   

<p> 
<img width="1723" height="924" alt="Snapshot 45" src="https://github.com/user-attachments/assets/cde0b46f-5e3d-4fa3-9dd5-d42742e4b13c" /> 
<img width="1735" height="933" alt="Snapshot 46" src="https://github.com/user-attachments/assets/8d1e922b-c17b-4089-b474-30f05f7ead33" />
<img width="1753" height="943" alt="Snapshot 47" src="https://github.com/user-attachments/assets/52443d73-5250-41ff-b5ba-f04f6b8f99e0" /> 
<img width="773" height="547" alt="Snapshot 48" src="https://github.com/user-attachments/assets/72a38829-14b8-4bf7-8571-f210a40bb587" />
</p>
<p>
We will now create a bunch of fake users using a powershell script. Start by logging into the domain  controller with the jane doe account. Then open powershell ISE as an admin.  

Create a new script and copy and paste this [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).  

This will create a 1000 users. Click run script. You should see all the accounts being created in the CLI. All users will have the password 'Password1'. You can go to Active Directory computers and users and see all the accounts in the _EMPLOYEES OU.  
</p>
<br />    

<p> 
<img width="704" height="744" alt="Snapshot 49" src="https://github.com/user-attachments/assets/39d5fc50-d10f-4e2d-a516-a5571e875674" />
</p>
<p>
You can now Login to client 1 with one of the accounts we created. I will login with bac.jape. 
</p>
<br />   

<h2>Group Policy and Mananging accounts</h2>  

<p> 
<img width="456" height="221" alt="Snapshot 50" src="https://github.com/user-attachments/assets/11ae1411-67ca-4ba7-86b2-978a8e9caa54" /> 
<img width="864" height="553" alt="Snapshot 51" src="https://github.com/user-attachments/assets/1ff09e3f-65b7-4a8b-847e-48327288a728" /> 
<img width="883" height="624" alt="Snapshot 52" src="https://github.com/user-attachments/assets/2d387026-fb93-4df7-bcbb-179df3baf3ac" />
<img width="962" height="650" alt="Snapshot 53" src="https://github.com/user-attachments/assets/cd614967-1ada-426a-918f-03f7a7b7c45e" />
<img width="1065" height="651" alt="Snapshot 54" src="https://github.com/user-attachments/assets/9c63e4a1-44b1-4c61-8c0c-c5cc0270cf17" />
<img width="1095" height="652" alt="Snapshot 55" src="https://github.com/user-attachments/assets/cf0df3db-d0f5-4463-b2de-e578c4e6d83c" />
<img width="1096" height="682" alt="Snapshot 56" src="https://github.com/user-attachments/assets/28fa8026-d0c6-4066-abf1-92559582bfde" />
<img width="1182" height="550" alt="Snapshot 57" src="https://github.com/user-attachments/assets/5a7d6c90-1c2c-4438-9287-17b192cb359a" />
<img width="1026" height="388" alt="Snapshot 58" src="https://github.com/user-attachments/assets/7567262d-7439-4b14-bfd6-7b139a99bbf6" />
</p>
<p>
We will need to configure a group policy that will only allow users a certain number of login 
attempts before their account is locked out for security reasons. This is to stop any hackers from 
guessing a user's password with an unlimited amount of guesses. Right click start > run and type in gpmc.msc. This will open the group policy management console. Navigate to the group policy objects OU and right click default domain policy to edit it. This will open the group policy management editor. Computer Configuration > Policies > Windows settings > Security Settings > Account Policies > Account Lockout Policy. Account lockout duration is how long a user will be locked out and account lockout threshold will be how many appointments a user gets before they are locked out. Set Account lockout duration to 30 mins and set account lockout threshold to 5 attempts  You can confirm the account settings in the group policy management console by clicking the settings tab. It will take 90 mins for these settings to be updated to all computer clients in the domain or you can automatically update the policy on client 1 by using the command prompt as an admin and type  the command gpupdate /force with the jane doe account.  
</p>
<br />    

<p> 
<img width="797" height="627" alt="Snapshot 59" src="https://github.com/user-attachments/assets/61c9caf4-4041-465a-83bd-d39fb6f4eb79" /> 
<img width="797" height="627" alt="Snapshot 60" src="https://github.com/user-attachments/assets/fb7656d4-359b-4422-ade1-2853e3b98d13" /> 
<img width="786" height="717" alt="Snapshot 61" src="https://github.com/user-attachments/assets/cd607c2e-f318-4033-9f9a-cf8cec61de10" /> 
<img width="788" height="624" alt="Snapshot 62" src="https://github.com/user-attachments/assets/d6920fde-32c3-4608-ae7f-ee160d2bed85" />
</p>
<p>
With our lockout policy now in place, let's lockout one of our users accounts on purpose 
to see how we can unlock their account. This is a very common problem in the help desk. Pick any account from the _EMPLOYEES OU and try to login to client 
1 with the wrong password more than 5 times. You should get a notification saying this account is now locked out. Login into DC-1 with the jane account and go active directory users and computers and navigate to _EMPLOYEEES OU. You can find the account more easily by right clicking the _EMPOLYEE OU, click find and type in the user's account username. Click on the account and select the account tab. Check the box to unlock the account. You should be able to login to client 1 with the account now. You can also reset a user's password by right licking it and selecting a new password. You can also unlock an account there as well. You should be able to login to client 1 with that account now.  
</p>
<br />    

<p> 
<img width="785" height="628" alt="Snapshot 63" src="https://github.com/user-attachments/assets/1620e0df-3540-4bcc-b643-31ee3513a278" />
<img width="779" height="636" alt="Snapshot 64" src="https://github.com/user-attachments/assets/7ddd50b3-8803-400c-bc22-80467c9973db" />
</p>
<p>
For whatever reason you need to, you can disable a user's account. This will prevent them from logging into any clients in the domain. Right click the user's account and click the disabled account. You should see a little down arrow when it is disabled. You can enable an account by right clicking it and selecting the enabled account. The little arrow should go away.
</p>
<br />   

<p> 
<img width="465" height="231" alt="Snapshot 65" src="https://github.com/user-attachments/assets/63b825d2-f10b-44a7-b5ff-48b0f8c9117e" /> 
<img width="803" height="573" alt="Snapshot 66" src="https://github.com/user-attachments/assets/9174fe43-ace2-4f65-89fc-d44fe3e3827a" /> 
<img width="1208" height="650" alt="Snapshot 67" src="https://github.com/user-attachments/assets/37e1c1b4-1e75-4c62-839d-a43357dd8964" /> 
<img width="1099" height="507" alt="Snapshot 68" src="https://github.com/user-attachments/assets/1cf6dfef-da18-4cfd-80ae-150e0d87edde" />
</p>
<p>
You can login into client 1 as an admin and use eventvwr.msc to view logs on the client. Navigate to windows logs > security to view logon attempts to the client. You can find the logon attempts of a specific account by right clicking security and selecting find. Type in the account you want to see and you can view each log from that account.
</p>
<br />   
