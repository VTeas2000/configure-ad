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

- VM Creation
- Step 2
- Step 3
- Step 4

<h3>VM Creation</h3>
<p>
In Microsoft Azure, create a Windows Server Virtual Machine within a Resource Group to serve as the domain controller. Use any values for the credentials, but keep them in mind.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/e4e7b384-81df-4cb9-abea-e69697dc1c3b" height="80%" width="80%" alt="DC VM"/>
<br>Create a Windows 10 Virtual Machine within the same Resource Group as the DC VM to serve as the client. Use any values for the credentials, but keep them in mind.</br>
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/7c9a3531-175f-45f7-a89e-339917b7bb59" height="80%" width="80%" alt="Client VM"/>
<br>The next step is to set the DC's NIC private IP address to be static. Go to "Virtual machines" and select your DC VM.
<br>From there, select the "Networking" tab and click on the <b>Network Interface</b>.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/80cf04e8-4ec9-4cb4-bf32-f96e0c1780c5" height="80%" width="80%" alt="Network Interface"/>
<br>Select the "IP config" tab and click on "ipconfig1".
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/5cab3fc1-af3f-469f-888f-462d58f84286" height="80%" width="80%" alt="IP Config"/>
<br>Set the private IP to be static and save.
<img src="https://github.com/VTeas2000/configure-ad/assets/60052902/e5a8b0c9-0cec-4ab6-8070-43d0fd670960" height="80%" width="80%" alt="Static IP"/>
</p>
