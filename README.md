<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Group Policy Administration in Active Directory</h1>

<p>
This lab demonstrates how <b>Group Policy</b> is used in an <b>Active Directory</b> environment to centrally manage user and computer settings. Using the domain environment previously deployed in Microsoft Azure, the lab focuses on creating, linking, configuring, and validating Group Policy Objects (GPOs) within the domain.
</p>

<p>
Tasks include creating and linking a new <b>Group Policy Object (GPO)</b>, configuring policy settings for client machines, forcing policy updates, and verifying policy application on domain-joined systems. These exercises simulate common administrative tasks performed in enterprise Windows environments.
</p>

<h2>Lab Prerequisites</h2>

<p>
This lab builds on the Active Directory environment created in previous labs. The following components should already be configured before starting this exercise:
</p>

<ul>
<li>An Active Directory domain deployed in <b>Microsoft Azure</b></li>
<li>A Domain Controller virtual machine (<b>DC-1</b>) running <b>Windows Server 2025 Datacenter</b></li>
<li>A domain-joined client virtual machine (<b>Client-1</b>) running <b>Windows 10 Enterprise</b></li>
<li>Administrative access using the account <b>mydomain.com\jane_admin</b></li>
<li>Existing Organizational Units including <b>_CLIENTS</b> and <b>_EMPLOYEES</b></li>
<li>Multiple domain user accounts created for testing Group Policy enforcement</li>
</ul>

<p>
Refer to the following lab for the domain environment setup:
</p>
<p>
<a href="https://github.com/chrisdegutis/configure-ad"><b>Active Directory Domain Deployment in Microsoft Azure</b></a>
</p>

<h2>Environments and Technologies Used</h2>

<ul>
<li>Microsoft Azure (Virtual Machines / Compute)</li>
<li>Active Directory Domain Services (AD DS)</li>
<li>Group Policy Management</li>
<li>Remote Desktop Protocol (RDP)</li>
<li>PowerShell / Command Prompt</li>
<li>Windows Administrative Templates</li>
</ul>

<h2>Operating Systems Used</h2>

<ul>
<li>Windows Server 2025 Datacenter (Domain Controller)</li>
<li>Windows 10 Enterprise (Domain Client)</li>
</ul>

<h2>High-Level Deployment and Configuration Steps</h2>

<p>
This lab demonstrates centralized administration and endpoint security management in an Active Directory environment by:
</p>

<ul>
<li>Creating and linking a <b>Group Policy Object (GPO)</b> to the <b>_CLIENTS</b> Organizational Unit</li>
<li>Configuring a <b>login banner</b> to display a security notice to users</li>
<li>Restricting access to <b>Control Panel</b> and <b>Command Prompt</b> for standard users</li>
<li>Blocking <b>removable storage devices</b> to help prevent unauthorized data transfer</li>
<li>Updating Group Policy on a domain-joined client machine using <b>gpupdate /force</b></li>
<li>Verifying applied policies using <b>gpresult /r</b></li>
<li>Testing policy enforcement on the client machine</li>
</ul>

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Open Group Policy Management</h3>

<p>
Log into <b>DC-1</b> using the domain administrator account <b>mydomain.com\jane_admin</b>.
</p>

<p>
Open <b>Group Policy Management</b> by selecting <b>Tools → Group Policy Management</b> from <b>Server Manager</b>.
</p>
<hr>

<h3>Step 2: Create a Domain Security Baseline GPO</h3>
<p>
Right-click <b>mydomain.com</b> and select <b>Create a GPO in this domain, and Link it here</b>.
</p>
<p>
Name the new policy <b>Domain Security Baseline</b>.
</p>
<hr>

<h3>Step 3: Configure the Domain Password Policy</h3>

<p>
Right-click the <b>Domain Security Policy</b> GPO and select <b>Edit</b>.
</p>

<p>
Navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy</b>
</p>

<ul>
<li><b>Enforce password history:</b> 5 passwords remembered</li>
<li><b>Maximum password age:</b> 60 days</li>
<li><b>Minimum password length:</b> 10 characters</li>
<li><b>Password must meet complexity requirements:</b> Enabled</li>
</ul>
<hr>

<h3>Step 4: Configure the Account Lockout Policy</h3>

<p>
Within the same GPO, navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy</b>
</p>

<ul>
<li><b>Account lockout threshold:</b> 5 invalid logon attempts</li>
<li><b>Account lockout duration:</b> 15 minutes</li>
<li><b>Reset account lockout counter after:</b> 15 minutes</li>
</ul>
<hr>

<h3>Step 5: Create a Workstation Security Baseline GPO</h3>

<p>
Right-click the <b>_CLIENTS</b> Organizational Unit and select <b>Create a GPO in this domain, and Link it here</b>.
</p>

<p>
Name the policy <b>Workstation Security Baseline</b>.
</p>
<hr>

<h3>Step 6: Configure a Login Banner</h3>

<p>
Edit the <b>Client Security Policy</b> GPO.
</p>

<p>
Navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options</b>
</p>

<ul>
<li><b>Interactive logon: Message title for users attempting to log on</b></li>
<li><b>Interactive logon: Message text for users attempting to log on</b></li>
</ul>
<hr>
