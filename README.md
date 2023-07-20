<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up resources in Microsoft Azure
- Ensure connectivity between the client and domain controller
- Install active directory
- Create an admin and normal user account in AD
- Join the client to your domain
- Setup Remote Desktop for non-administrative users on the client
- Create additional users and attempt to log into the client with one of the users

<h3>Set up resources in Microsoft Azure</h3>
<p>
In Microsoft Azure, create a Windows Server Virtual Machine within a Resource Group to serve as the domain controller. Use any values for the credentials, but keep them in mind.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/e4e7b384-81df-4cb9-abea-e69697dc1c3b" height="80%" width="80%" alt="DC VM"/>
<br>Create a Windows 10 Virtual Machine within the same Resource Group as the DC VM to serve as the client. Use any values for the credentials, but keep them in mind.<br>
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/7c9a3531-175f-45f7-a89e-339917b7bb59" height="80%" width="80%" alt="Client VM"/>
<br>The next step is to set the DC's NIC private IP address to be static. Go to "Virtual machines" and select your DC VM.
<br>From there, select the "Networking" tab and click on the <b>Network Interface</b>.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/80cf04e8-4ec9-4cb4-bf32-f96e0c1780c5" height="80%" width="80%" alt="Network Interface"/>
<br>Select the "IP config" tab and click on "ipconfig1".
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5cab3fc1-af3f-469f-888f-462d58f84286" height="80%" width="80%" alt="IP Config"/>
<br>Set the private IP to be static and save.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/e5a8b0c9-0cec-4ab6-8070-43d0fd670960" height="80%" width="80%" alt="Static IP"/>
</p>

<h3>Ensure connectivity between the client and domain controller</h3>
<p>
First, copy your client VM's public IP from Microsoft Azure.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/d6cc757c-1489-411a-9020-d0e8ad326514" height="80%" width="80%" alt="Client IP"/>
<br>Connect to your client VM using Remote Desktop with its public IP and credentials. Connect even if you get a certificate error.
<br>Back in Microsoft Azure on your main device, copy your DC's private IP address.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/54d449ff-5ff1-4a25-83cf-6e975f7a8bc0" height="80%" width="80%" alt="DC IP"/>
<br>On your client VM, open command prompt and perpetually ping the DC's private IP address using "ping <ip address> -t"
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/74652fd9-c014-43ed-bfb0-3e09f18301aa" height="80%" width="80%" alt="Ping DC"/>
<br>Open another Remote Desktop connection using your DC's public IP and credentials.
<br>In your DC VM, access "Windows Defender Firewall with Advanced Security". Enable "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" and "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)"
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/280c460d-9f50-4292-8d1e-4a31e9e9d88a" height="80%" width="80%" alt="Enabling Rules"/>
<br>Back on your client VM, you should get replies from your DC VM.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/1096cd93-b178-4c8c-aacf-2f89bf072a12" height="80%" width="80%" alt="Ping Replies"/>
</p>

<h3>Install Active Directory</h3>
<p>
On your DC VM, click "Add roles and features" from the dashboard of your server manager.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/aeecc718-7f18-456e-9642-efb5787afc5d" height="80%" width="80%" alt="Setup"/>
<br>During the setup, select "Active Directory Domain Services". Hit install at the end of the setup.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/08cf343c-01fd-4cf7-b307-0a91922e9116" height="80%" width="80%" alt="Installation"/>
<br>After the installation is complete, click and flag and click "Promote this server to a domain controller"
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/c6c1e6dd-b97e-4847-889c-18607e4eb1f2" height="80%" width="80%" alt="Promotion"/>
<br>Add a new forest and input your root domain name. Enter any password and complete the installation.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/47ddc1ec-c56f-4548-a9b7-dab3ae21ed7f" height="80%" width="80%" alt="Domain"/>
<br>Close your DC VM. Reconnect to it using the same public IP and password but add your domain name before your previous username (must use "\" instead of "/").<br>
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/0927bf30-ce69-4c85-8657-472fb29aec3f" height="80%" width="80%" alt="Reconnect"/>
</p>

<h3>Create an admin and normal user account in AD</h3>
<p>
On your DC VM, access "Active Directory Users and Computers" from the tools of your server manager.
<br>In your domain, create "employees" and "admins" organizational units.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/42191794-4cea-4258-9528-6294bc80f80e" height="80%" width="80%" alt="OU"/>
<br>Create a user within the employees organizational unit. Choose any password, but ensure that it doesn't need to change after every login and never expires. Keep it in mind.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/eb8d0954-0d21-4c14-a559-e627cfc14647" height="80%" width="80%" alt="User Creation"/>
<br>Select the properties of your new user and make it a member of the "Domain Admins" security group. This user will be your primary admin.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/78aa785f-2ec9-4b51-923e-d38d9eb45520" height="80%" width="80%" alt="Domain Admin"/>
</p>

<h3>Join the client to your domain</h3>
<p>
In Microsoft Azure, access your client VM's <b>network interface</b> from the "Networking" tab.
<br>Add the DC's private IP as a DNS server. Restart your client VM.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/0c3824cc-826c-45c9-a00d-0877e0021a42" height="80%" width="80%" alt="DNS"/>
<br>Reconnect to your client VM using Remote Desktop using your normal username. To check if the DC's private IP was added as a DNS server, open command prompt and input "ipconfig /all"
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/eabb64be-0908-4ea6-8096-184bf2fa7815" width="80%" alt="DNS"/>
<br>In your client's "About" settings, click "Rename this PC (advanced)"
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/41cb39dc-6b81-432a-a83f-aec198b0ee74" width="80%" alt="Rename"/>
<br>Click "Change..."<br>
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5497ad00-e7ae-48b1-b805-6f3dee344de7" width="80%" alt="Add Client to Domain"/>
<br>Enter your domain name.<br>
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5238504d-e0e3-4832-aa0f-ce08a54b30e0" width="80%" alt="Domain Name"/>
<br>Enter your admin's credentials for your DC's VM. Save and restart your client VM.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/0e47ac32-6986-4511-aa60-d406ff1bf2cb" width="80%" alt="DC Admin"/>
<br>On your DC, verify that your client appears in the "Computers" section of "Active Directory Users and Computers".
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5e0538e6-a01d-4fb4-b3cc-d2d7b151cdb7" width="80%" alt="Client Verification"/>
<br>For organizational purposes, create a "clients" organization unit and move your client from "Computers" to it.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/cf2bbb71-80f5-4eb9-9fbe-ad48bf173333" width="80%" alt="Organization"/>
</p>

<h3>Setup Remote Desktop for non-administrative users on the client</h3>
<p>
Connect to your client VM using your admin credentials for your DC VM.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/59f15329-7721-4743-b8da-fc02cdb55fb0" width="80%" alt="Client Connection"/>
<br>Go to "Remote Desktop" under your client's system settings. Under user accounts, click "Select users that can remotely access this PC".
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5c2e73de-fb12-4a38-8682-e052109c218c" width="80%" alt="RD Settings"/>
<br>Add "Domain Users" as remote desktop users.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/988c670b-56f2-492a-91fe-cfde1ddc08cf" width="80%" alt="Add Domain Users"/>
</p>
