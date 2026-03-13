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
Log into <b>DC-1</b> using the domain administrator account:
</p>
<p>
<b>Username:</b> mydomain.com\jane_admin<br>
<b>Password:</b> Cyberlab123!
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

<h3>Step 7: Restrict Access to Control Panel</h3>

<p>
Navigate to:
</p>

<p>
<b>User Configuration → Policies → Administrative Templates → Control Panel</b>
</p>

<p>
Enable:
</p>

<p>
<b>Prohibit access to Control Panel and PC settings</b>
</p>
<hr>

<h3>Step 8: Prevent Access to Command Prompt</h3>

<p>
Navigate to:
</p>

<p>
<b>User Configuration → Policies → Administrative Templates → System</b>
</p>

<p>
Enable:
</p>

<p>
<b>Prevent access to the command prompt</b>
</p>
<hr>

<h3>Step 9: Block Removable Storage Devices</h3>

<p>
Navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access</b>
</p>

<p>
Enable:
</p>

<p>
<b>All Removable Storage classes: Deny all access</b>
</p>
<hr>

<h3>Step 10: Disable Microsoft Consumer Experience</h3>

<p>
Navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Administrative Templates → Windows Components → Cloud Content</b>
</p>

<p>
Enable:
</p>

<p>
<b>Turn off Microsoft consumer experiences</b>
</p>
<hr>

<h3>Step 11: Configure Windows Update Through Group Policy</h3>

<p>
Navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Update → Manage end user experience</b>
</p>

<p>
Enable:
</p>

<p>
<b>Configure Automatic Updates</b>
</p>

<p>
Set to:
</p>

<p>
<b>4 - Auto download and schedule the install</b>
</p>
<hr>

<h3>Step 12: Configure a Screen Lock Policy</h3>

<p>
On <b>DC-1</b>, edit the <b>Workstation Security Baseline</b> GPO.
</p>

<p>
Navigate to:
</p>

<p>
<b>User Configuration → Policies → Administrative Templates → Control Panel → Personalization</b>
</p>

<p>
Enable the following policies:
</p>

<ul>
<li><b>Enable screen saver</b></li>
<li><b>Password protect the screen saver</b></li>
<li><b>Screen saver timeout</b></li>
</ul>

<p>
Set the <b>Screen saver timeout</b> to <b>900 seconds</b> (15 minutes).
</p>

<p>
This policy helps secure unattended workstations by automatically locking the user session after a period of inactivity.
</p>
<hr>

<h3>Step 13: Configure Windows Defender Firewall Through Group Policy</h3>

<p>
Within the same <b>Workstation Security Baseline</b> GPO, navigate to:
</p>

<p>
<b>Computer Configuration → Policies → Windows Settings → Security Settings → Windows Defender Firewall with Advanced Security</b>
</p>

<p>
Select <b>Windows Defender Firewall with Advanced Security</b>, then ensure firewall protection is enabled for the <b>Domain Profile</b>, <b>Private Profile</b>, and <b>Public Profile</b>.
</p>

<p>
This helps ensure that firewall protection remains enabled on domain-joined workstations and prevents users from disabling an important layer of endpoint security.
</p>
<hr>


<h3>Step 14: Update Group Policy on Client-1</h3>

<p>
Using <b>Remote Desktop Connection</b>, connect to the client virtual machine <b>Client-1</b>.
</p>

<p>
Log in using the following domain administrator credentials:
</p>

<p>
<b>Username:</b> mydomain.com\jane_admin<br>
<b>Password:</b> Cyberlab123!
</p>

<p>
After logging in, open <b>PowerShell</b> by right-clicking the <b>Start Menu</b> and selecting <b>Windows PowerShell</b>.
</p>

<p>
Run the following command to immediately apply the newly configured Group Policy settings:
</p>

<p>
<b>gpupdate /force</b>
</p>

<p>
This command forces the workstation to refresh and apply the latest policies from the domain controller instead of waiting for the default Group Policy refresh interval.
</p>
<hr>

<h3>Step 15: Verify Applied Group Policies</h3>

<p>
While still logged into <b>Client-1</b>, open <b>PowerShell</b> if it is not already open.
</p>

<p>
Run the following command to view the Group Policy Objects currently applied to the computer and user:
</p>

<p>
<b>gpresult /r</b>
</p>

<p>
Review the output and locate the section labeled <b>Applied Group Policy Objects</b>.
</p>

<p>
Confirm that both of the following policies appear in the list:
</p>

<ul>
<li><b>Domain Security Baseline</b></li>
<li><b>Workstation Security Baseline</b></li>
</ul>

<p>
This verifies that both the domain-level and workstation-level Group Policy configurations are successfully applied to <b>Client-1</b>.
</p>
<hr>

<h3>Step 16: Test the Policies</h3>

<ul>
<li>Sign out and verify the login banner appears</li>
<li>Attempt to open Control Panel</li>
<li>Attempt to open Command Prompt</li>
<li>Test removable storage access if available</li>
</ul>
<hr>

<h2>Conclusion</h2>

<p>
In this lab, <b>Group Policy</b> was used to implement centralized configuration and security management within an <b>Active Directory</b> environment hosted in <b>Microsoft Azure</b>. Two separate Group Policy Objects were created to demonstrate how policies can be applied at different scopes within a domain.
</p>

<p>
A <b>Domain Security Baseline</b> policy was configured at the domain level to enforce authentication standards such as password complexity requirements and account lockout protections. These settings help strengthen identity security across the entire domain by ensuring consistent password policies and protecting against repeated failed authentication attempts.
</p>

<p>
A second policy, the <b>Workstation Security Baseline</b>, was applied to the <b>_CLIENTS</b> Organizational Unit to manage workstation security and user environment settings. These configurations included displaying a login banner, restricting access to Control Panel and Command Prompt, blocking removable storage devices, disabling consumer-focused Windows features, configuring Windows Update behavior, enforcing screen lock policies, and ensuring Windows Defender Firewall protection remains enabled.
</p>

<p>
After configuring these policies, Group Policy updates were applied to the client machine and verified using administrative tools such as <b>gpupdate</b> and <b>gpresult</b>. The applied policies were then tested on the domain-joined workstation to confirm that the security and configuration settings were successfully enforced.
</p>

<p>
This lab demonstrates practical experience with <b>Active Directory Group Policy administration</b>, <b>domain security baselines</b>, and <b>enterprise workstation management</b>. These are foundational skills used by system administrators and IT support professionals to centrally manage security, enforce organizational standards, and maintain consistent configuration across enterprise environments.
</p>
