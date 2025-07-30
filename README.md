
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory within Azure VM</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Step 1: Install Active Directory</h2>
<p>
If you aren't already, make sure to get logged into DC-1 so we can get started. The first step will be installing Active Directory onto the Windows Server VM. To do this, we open up Server Manager, then click "Add Roles and Features".
</p>
<p>
<img width="842" height="421" alt="image" src="https://github.com/user-attachments/assets/ec0d5236-996a-4125-9682-73a0db30aaca" />
</p>
<p>
After clicking that, it should pop up an install wizard, click Next until you get to Server Roles, then choose Active Directory Domain Services by checking the empty box. 
</p>
<p>
<img width="781" height="554" alt="image" src="https://github.com/user-attachments/assets/c30d69ba-8d93-499a-8ffe-47d3fb7611c0" />
</p>
<p>
Click Add Features.
</p>
<p>
<img width="401" height="428" alt="image" src="https://github.com/user-attachments/assets/9bb549d1-580f-416d-b8cb-8ec3330a6aec" />
<img width="781" height="553" alt="image" src="https://github.com/user-attachments/assets/ad7648a3-fdd2-431d-a4bd-9017a9430e4c" />
</p>
<p>
Then click Next until you get to the Confirmation tab, click Restart the destination server if required, then click Install to install AD DS.
</p>
<p>
<img width="778" height="556" alt="image" src="https://github.com/user-attachments/assets/975fdc57-84cb-46d0-9c79-272e7c6417ec" />
<img width="780" height="557" alt="image" src="https://github.com/user-attachments/assets/2950090d-d54c-4920-8227-2b862d65dca6" />
</p>
<p>
Once it finishes installing, click Close. In the top right of the Server Manager, you should see a flag with a warning icon. CLick it, then click "Promote This Server to a Domain Controller".
</p>
<p>
<img width="563" height="675" alt="image" src="https://github.com/user-attachments/assets/3a8abc3f-1431-40d4-bb1e-26baa0a713dc" />
<img width="375" height="130" alt="image" src="https://github.com/user-attachments/assets/df1630c5-8547-4430-9d50-29041057419d" />
</p>
<p>
After clicking, it should open up another wizard. Make sure you click the option "Add a new forest". For the sake of the Tutorial, we're going to name the Root domain name "mydomain.com" but you can name it whatever you want. Just make sure it's something you're going to remember.
</p>
<p>
 <img width="757" height="557" alt="image" src="https://github.com/user-attachments/assets/e8a0e6c1-c936-4e54-a1e4-5c7c744126dc" />
</p>
<p>
After you click Next, it's going to ask you to create a Directory Services Restore Mode (DSRM) Password. We won't be using this, so just give whatever password you want. I went with Password1. 
</p>
<p>
<img width="744" height="559" alt="image" src="https://github.com/user-attachments/assets/585e7fff-b23f-4cde-a613-588e70a2ef9d" />
</p>
<p>
Click Next again, and make sure the "Create DNS Delegation" option is unchecked.
</p>
<p>
<img width="759" height="561" alt="image" src="https://github.com/user-attachments/assets/7743f11b-7700-411f-8520-aa3ce20a4dad" />
</p>
<p>
After that, you can just click Next until you can click Install, then click Install.
</p>
<p>
<img width="767" height="559" alt="image" src="https://github.com/user-attachments/assets/0208ea05-ad8a-4f60-bcf8-a50b89b79b3d" />
<img width="339" height="136" alt="image" src="https://github.com/user-attachments/assets/89632603-06c6-4169-84f6-4bef641a90b6" />
</p>
<p>
Once it's finished installing, DC-1 will be forced to restart, and your connection to it will be severed, like in the screenshot above. Once it comes back up, go ahead and use RDP to connect back into it and we can move on to the next step!
</p>

<br />
<h2>Step 2: Create a Domain Admin user within the domain</h2>

<p>
Once you're back and logged in to DC-1, click the Start Menu in the bottom left corner of the screen. When you use RDP this time, make sure that when you log in, you log in as mydomain.com\labuser instead of just using labuser as the username. Make sure you use the correct slash. Then, once logged in, click Windows Administrative Tools, then Active Directory Users and Computers.
</p>
<p>
<img width="447" height="565" alt="image" src="https://github.com/user-attachments/assets/21015c2e-c596-44a4-ae23-341b0a5ee40f" />
<img width="646" height="675" alt="image" src="https://github.com/user-attachments/assets/85049779-16b1-42fd-95bc-e25b29e7d2fe" />
<img width="646" height="671" alt="image" src="https://github.com/user-attachments/assets/7fe1dc94-2bd2-4abf-921f-1f87af9b37b6" />
<img width="745" height="530" alt="image" src="https://github.com/user-attachments/assets/0080e6f4-c0ba-46aa-9994-85843317d116" />
</p>
<p>
Inside of there, we'll create a new Organizational Unit (OU) called "_EMPLOYEES". We do this by right-clicking mydomain.com, then clicking New -> Organizational Unit.
</p>
<p>
<img width="746" height="531" alt="image" src="https://github.com/user-attachments/assets/88a51c14-f4c2-46d7-9177-9dd281d35376" />
<img width="424" height="369" alt="image" src="https://github.com/user-attachments/assets/5c301ff2-8a76-47bd-a218-93db3a37e149" />
<img width="743" height="520" alt="image" src="https://github.com/user-attachments/assets/db5fe6cb-4255-4143-9f72-edaa44df71ba" />
</p>
<p>
Create another OU named "_ADMINS"
</p>
<p>
<img width="746" height="533" alt="image" src="https://github.com/user-attachments/assets/d8904301-5b81-4f4e-af8e-6c2412281f34" />
<img width="431" height="375" alt="image" src="https://github.com/user-attachments/assets/e133d5ca-73ea-4fa1-8337-510c04275949" />
<img width="747" height="517" alt="image" src="https://github.com/user-attachments/assets/5778a348-b215-4948-9721-542b174f4c9d" />
</p>
<p>
Now, we're going to create a new ADMIN employee named "Jane Doe". You can do this by clicking on the _ADMINS OU and then right-clicking -> New -> User
</p>
<p>
<img width="791" height="548" alt="image" src="https://github.com/user-attachments/assets/7394f0f4-b37d-4a10-bec2-1efd54dedf23" /></p>
<p><img width="443" height="382" alt="image" src="https://github.com/user-attachments/assets/d3f57466-b5bf-4bbc-a2e1-2328f2950c71" /></p>
<p><img width="429" height="375" alt="image" src="https://github.com/user-attachments/assets/e78e95b5-1bc0-460f-acaa-ceca2e2c5822" /></p>
<p></p><img width="429" height="371" alt="image" src="https://github.com/user-attachments/assets/85aebbbd-4dd7-4a12-8263-bceaa2450055" /></p>
<p>
<img width="746" height="535" alt="image" src="https://github.com/user-attachments/assets/f8bb0da7-9f19-49e2-9438-bbf12728a13f" />
</p>
<p>
Next, we'll add Jane to our "Domain Admins" Security Group. To do this, right-click the User, then click Properties -> Member Of -> Add -> Type Domain Admins -> Check Names -> OK 
</p>
<p>
<img width="742" height="519" alt="image" src="https://github.com/user-attachments/assets/2fb07263-4894-468a-bc84-681a85c4c803" /></p>
<p><img width="409" height="539" alt="image" src="https://github.com/user-attachments/assets/fd89a5ea-90e4-425e-9a83-9c6960fed046" /></p>
<p><img width="448" height="247" alt="image" src="https://github.com/user-attachments/assets/b0690f80-9d33-4c09-bbfb-227719cac1d1" /></p>
<p>
<img width="453" height="246" alt="image" src="https://github.com/user-attachments/assets/60dff625-f240-4b27-bdbd-d61d3783f3f5" /></p>
<p>
<img width="405" height="532" alt="image" src="https://github.com/user-attachments/assets/906b49a3-f529-4cbb-b2dc-c3a12ab3daac" />
</p>
<p>
Now that Jane has been properly configured, we're going to close the connection to DC-1, then re-login as "mydomain.com\jane_admin" and use Jane as our admin account from now on.
</p>
<p>
<img width="444" height="541" alt="image" src="https://github.com/user-attachments/assets/bb263d09-d85d-4ea8-afab-2c028af5d3cd" />
</p>
<p>
Once you're logged back in as Jane. We'll move on to the final step!
</p>
<br />

<h2>Step 3: Join Client-1 to your domain (mydomain.com)</h2>

<p>
For now, stay logged in as Jane to DC-1, but minimize it and open up RDP again. This time, we're going to log into Client-1 as labuser and join the VM to the domain. (This will make it restart)
</p>
<p>
<img width="452" height="412" alt="image" src="https://github.com/user-attachments/assets/426d922a-541d-438b-a21a-ea2c37720129" />
</p>
<p>
Once you're logged into Client-1, go ahead and click the Start Menu in the bottom left corner and click Settings on the bottom left side of the menu. Once there, click System -> About
</p>
<p>
<img width="641" height="669" alt="image" src="https://github.com/user-attachments/assets/bac5e5dc-2482-48be-9cdd-f719a42ab2c0" />
<img width="1182" height="934" alt="image" src="https://github.com/user-attachments/assets/3debe3cf-5dfc-49e2-90ee-f6d42bec55bc" />
</p>
<p>
Inside the About window, in the upper right there should be an option called "Rename this PC (Advanced)", click that, then click Change.
</p>
<p>
<img width="268" height="602" alt="image" src="https://github.com/user-attachments/assets/087f879a-93a5-47f9-b45a-6c868490a6a7" />
<img width="402" height="471" alt="image" src="https://github.com/user-attachments/assets/21fb91bb-8ff8-4655-86e2-8ee90d086566" />
</p>
<p>
Make sure to change the "Member Of" to "Domain:" then type "mydomain.com".
</p>
<p>
<img width="315" height="392" alt="image" src="https://github.com/user-attachments/assets/12cc4dce-f078-4c8a-9334-d2c26cc09968" />
</p>
<p>
When you click OK, it'll ask you to enter the name and password of an account with permission join the domain. Use Jane's credentials.
</p>
<p>
<img width="455" height="284" alt="image" src="https://github.com/user-attachments/assets/b2911a7a-70a6-4c26-9d42-6c577a8a5ef0" /></p>
<p><img width="291" height="152" alt="image" src="https://github.com/user-attachments/assets/ccc57c06-8cce-4db2-9cc8-aede664510d9" />
</p>
<p>
<img width="347" height="181" alt="image" src="https://github.com/user-attachments/assets/eb65325e-b500-4bc8-a8c8-b22434f622a0" />
<img width="344" height="165" alt="image" src="https://github.com/user-attachments/assets/b492ade4-07ad-4791-b647-66a3bfb85be3" />
</p>
<p>
Allow the computer to restart.
</p>
<p>
Now, we're still logged in as Jane to DC-1, so go ahead and open Active Directory Users and Computers and check to see if Client-1 shows up under "Computers"
</p>
<p>
<img width="751" height="528" alt="image" src="https://github.com/user-attachments/assets/2a3d7916-3683-47cb-b84a-e09133214f18" />
</p>
<p>
Once verified that it does, we'll create one final OU named "_CLIENTS" and drag Client-1 into it.
</p>
<p>
<img width="746" height="533" alt="image" src="https://github.com/user-attachments/assets/90c5a885-8709-431a-8b66-cf60ab1c3701" />
<img width="430" height="384" alt="image" src="https://github.com/user-attachments/assets/b31273f1-4209-4576-aa90-0a2e2a504950" /></p>
<p><img width="460" height="162" alt="image" src="https://github.com/user-attachments/assets/be1965c8-768d-4003-a160-8587d67efc2d" /></p>
<p><img width="743" height="528" alt="image" src="https://github.com/user-attachments/assets/1caa3cf8-63ed-43b8-b878-ae1aacc88177" />
</p>
