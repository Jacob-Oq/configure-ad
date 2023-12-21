<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
With Active Directory installed on the domain controller VM, it is time to create new Organizational Units and Users. With the Active Directory Users and Computers console open, right click on the domain you created and make a new Organizational Unit.
</p>

![AD-Lab 27](https://github.com/Jacob-Oq/configure-ad/assets/150084528/8540b4c2-742d-479b-842d-8fcf5ecc6b17)

![AD-Lab 28](https://github.com/Jacob-Oq/configure-ad/assets/150084528/1fab876f-3ba1-4292-9fa8-24f45f8da1e8)


<p>Let's create two Organizational Units, _EMPLOYEES and _ADMINS.</p>

![AD-Lab 29](https://github.com/Jacob-Oq/configure-ad/assets/150084528/be33d08a-73a2-4d20-a9c8-11f98c48d9ce)

![AD-Lab 30](https://github.com/Jacob-Oq/configure-ad/assets/150084528/dc1cd4e2-cb93-4e39-9dea-aabdb0e76a43)

![AD-Lab 31](https://github.com/Jacob-Oq/configure-ad/assets/150084528/8850cc49-a11e-4310-bb9a-91c6748aacbb)

<p>
Within the _ADMINS OU, we can create new Users. Some can be given administrative privileges through the use of a Security Group. 
</p>

![AD-Lab 32](https://github.com/Jacob-Oq/configure-ad/assets/150084528/30635621-ff6e-4551-b816-4811e0149fbc)

![AD-Lab 33](https://github.com/Jacob-Oq/configure-ad/assets/150084528/2be70925-0e24-4608-b95b-58b855684ab0)

![AD-Lab 34](https://github.com/Jacob-Oq/configure-ad/assets/150084528/3f3580e0-c9e8-46a8-aaf6-a5ee307b2116)

![AD-Lab 35](https://github.com/Jacob-Oq/configure-ad/assets/150084528/e2574e11-2138-4c53-86f4-d1aa39ca7a18)


<p>
To grant admin privileges to a User, right click on the user and open their Properties. Click "Member Of" then "Add" to apply the appropraite security group.</p>

![AD-Lab 36](https://github.com/Jacob-Oq/configure-ad/assets/150084528/2ff273b2-8a8a-4c44-a0a3-e63075438dac)

  
<p>In this case, we will add them to the Domain Admins security group.</p>

![AD-Lab 37](https://github.com/Jacob-Oq/configure-ad/assets/150084528/5cbe5cb7-8707-4bcd-a16d-751e77628f0b)

![AD-Lab 38](https://github.com/Jacob-Oq/configure-ad/assets/150084528/83bcdbd9-e496-4ccb-ad2c-47f4f94ee1e4)



<br />
<p>
Before the client computer can join the domain, it is important to configure the DNS settings first. The DNS server must point to the domain controller's private IP address. On the Azure portal, open the Networking tab and click on Network Interface. In the DNS servers, enter the domain controller's private IP address and save the changes. Restart the client VM in order to ensure the DNS changes are saved.
</p>

![AD-Lab 39](https://github.com/Jacob-Oq/configure-ad/assets/150084528/253ba15a-689e-492c-801a-dd8446d72945)


<br />
<p>
Now we can join the client VM to the domain. In the System menu of the client VM, click on Rename this PC (advanced) and Change. Enter the domain and necessary credentials in order to let the client join the domain. It is important to note that the login credentials have to be input within the context of the domain path.
</p>

![AD-Lab 42](https://github.com/Jacob-Oq/configure-ad/assets/150084528/c9ce595a-9a9f-4987-90a0-305c6a365515)

  
<p>The client should now be part of the domain. On the domain controller, the client should now appear in Computers in the Active Directory Users and Computers panel. 
</p>

![AD-Lab 45](https://github.com/Jacob-Oq/configure-ad/assets/150084528/d50db304-a0d8-469e-b3a9-f5b4ade93c31)


<br />
<p>Before users in the domain can use the client computer, "Remote Desktop" has to be enabled for non-administrative users. While logged in as the administrator, open "System Properties." Click on "Remote Desktop" and Select users that can remotely access this PC. Grant "Domain Users" access to Remote Desktop. Non-administrative users can now log in to Client-1. 
</p>

![AD-Lab 42](https://github.com/Jacob-Oq/configure-ad/assets/150084528/f3b9ec39-8d2f-4a4a-98fb-c6a71f5b3df2)

![AD-Lab 43](https://github.com/Jacob-Oq/configure-ad/assets/150084528/613c2bb7-0006-447a-8325-da992803aa3e)

![AD-Lab 44](https://github.com/Jacob-Oq/configure-ad/assets/150084528/69381c21-4a97-4276-9ec4-4cd15dddab1d)

<p>  
Normally a Group Policy can do the same and allows changes to many systems at once. For the purposes of this lab, a Group Policy won't be used to make this change.
</p>

<br />
<p>Creating users can be done manually or through the use of a script. Here we use a PowerShell script. The PowerShell script can be found <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1)"> here. </a> On the domain controller, open PowerShell ISE as an administrator (and make sure you are logged in with an admin account on the domain controller). Create a new file and paste the script into ISE console. Run the script and observe the accounts being created. </p>

![AD-Lab 47](https://github.com/Jacob-Oq/configure-ad/assets/150084528/68f471ea-0b3f-4b94-80e3-3eb91733b8aa)


<br />
<p>After creating the users, Client-1 can now be signed in as one of the new users that were created from the PowerShell script. Pick a name and simply sign in to the client with the context of the domain. there are a few ways to confirm who is looged in and to what machine using the command line. You can do a search in the start menu and type "cmd" then enter. Once there you can type "whoami" to see the user and "hostname" to see which console you are logged into.</p>

![AD-Lab 48](https://github.com/Jacob-Oq/configure-ad/assets/150084528/2a7a74ae-c080-41b7-bb9e-c0b2156b4aaa)


<br />
<h2>Lessons Learned</h2>
<p>
Doing this lab should help teach how to set up Active Directory and join clients to the domain. Also how to create users and assign the necessary permissions. Active Directory is not difficult to learn despite all the menu navigation that takes place.
</p>


