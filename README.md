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
If you have not completed the environment setup, refer to the following project:
</p>

<p>
<b>Active Directory Domain Deployment in Microsoft Azure</b>
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
