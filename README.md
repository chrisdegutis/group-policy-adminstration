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
This lab demonstrates how <b>Group Policy</b> can be used to centrally manage security and configuration settings within an <b>Active Directory</b> environment. Both <b>domain-level</b> and <b>workstation-level</b> policies were implemented to simulate how enterprise organizations enforce security baselines and maintain consistent system configurations across their network infrastructure.
</p>
<ul>
<li>Creating a <b>Domain Security Baseline</b> Group Policy Object linked to the domain to enforce authentication security standards such as password complexity requirements and account lockout protections.</li>
<li>Configuring domain-wide <b>Password Policy</b> settings including password history, minimum password length, and complexity requirements to strengthen identity security.</li>
<li>Implementing an <b>Account Lockout Policy</b> to protect against repeated failed authentication attempts and reduce the risk of brute-force password attacks.</li>
<li>Creating a <b>Workstation Security Baseline</b> Group Policy Object linked to the <b>_CLIENTS</b> Organizational Unit to centrally manage endpoint configuration for domain-joined client machines.</li>
<li>Configuring workstation security controls including login banners, restrictions on Control Panel and Command Prompt access, blocking removable storage devices, and disabling consumer-focused Windows features.</li>
<li>Enforcing automatic workstation locking through a <b>Screen Lock Policy</b> that requires a password after a period of inactivity to help protect unattended systems.</li>
<li>Ensuring <b>Windows Defender Firewall</b> is enabled through Group Policy to maintain endpoint network protection across all domain-joined client systems.</li>
<li>Managing <b>Windows Update</b> behavior through Group Policy to centrally schedule update installation and maintain consistent patch management across client machines.</li>
<li>Applying and validating the configured policies by updating Group Policy on the client machine and verifying applied policies using administrative tools such as <b>gpupdate</b> and <b>gpresult</b>.</li>
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
<img width="800" height="2012" alt="image" src="https://github.com/user-attachments/assets/f02cb090-a5d0-4eca-bfa8-f1177b572513" />
<hr>

<h3>Step 2: Create a Domain Security Baseline GPO</h3>
<p>
Right-click <b>mydomain.com</b> and select <b>Create a GPO in this domain, and Link it here</b>.
</p>
<p>
Name the new policy <b>Domain Security Baseline</b>.
</p>
<img width="800" height="671" alt="image" src="https://github.com/user-attachments/assets/18839c00-1777-4df9-b9a0-2e40ac6d0044" />
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
<img width="800" height="1404" alt="image" src="https://github.com/user-attachments/assets/f78eb541-c0e2-4991-b662-0380cda55931" />
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
<img width="800" height="1406" alt="image" src="https://github.com/user-attachments/assets/74e9f59b-013e-41e7-a6b0-eff20454bc07" />
<hr>

<h3>Step 5: Create a Workstation Security Baseline GPO</h3>
<p>
Right-click the <b>_CLIENTS</b> Organizational Unit and select <b>Create a GPO in this domain, and Link it here</b>.
</p>
<p>
Name the policy <b>Workstation Security Baseline</b>.
</p>
<img width="800" height="1368" alt="image" src="https://github.com/user-attachments/assets/73c7a686-7dc7-417f-86a2-c10a722c175d" />
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
<li><b>Interactive logon: Message title for users attempting to log on</b>
<p>Authorized Use Notice</p>
</li>
<img width="800" height="1548" alt="image" src="https://github.com/user-attachments/assets/e128e2b1-afec-49e4-a724-6f4d7324244b" />

<li><b>Interactive logon: Message text for users attempting to log on</b>
<p>This computer system is the property of the organization and is intended for authorized use only. 
Users may access this system only with explicit permission and for approved business purposes.</p>

<p>All activities on this system may be monitored, recorded, and audited. 
By accessing this system, you acknowledge that you have no expectation of privacy and consent to monitoring.</p>

<p>Unauthorized use of this system is strictly prohibited and may result in disciplinary action, 
revocation of access privileges, and/or criminal prosecution.</p>
</li>
<img width="800" height="1590" alt="image" src="https://github.com/user-attachments/assets/054519d5-0ed1-45b0-bdaf-a0c84b9f8bd8" />
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
<img width="800" height="1986" alt="image" src="https://github.com/user-attachments/assets/7a974a99-d5b5-46da-95f3-7ca0fab7ad13" />
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
<img width="800" height="1928" alt="image" src="https://github.com/user-attachments/assets/d1c9b281-1364-449c-9aff-a9fc7fa0a188" />
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
<img width="800" height="1920" alt="image" src="https://github.com/user-attachments/assets/6de6e380-08fb-42c4-a2dd-e3948344c4c0" />
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
<img width="800" height="1888" alt="image" src="https://github.com/user-attachments/assets/d82e2c07-fa9a-44f6-b97e-f1d3d5b2767c" />

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
<p>
For the <b>Scheduled install day</b> select <b>1 - Sunday</b> and for the <b>Scheduled install time</b> select <b>03:00</b>
</p>
<p>This ensures the updates happen outside of business hours, least disruption to employees, and enough time before Monday's workday</p>
<img width="800" height="1920" alt="image" src="https://github.com/user-attachments/assets/be13f078-6b32-4e31-8cd5-6e3c08ec22f0" />

<hr>

<h3>Step 12: Configure a Screen Lock Policy</h3>

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
<img width="800" height="1882" alt="image" src="https://github.com/user-attachments/assets/709636d6-6bca-49d0-9a54-5ecd3a02e76d" />

<hr>

<h3>Step 13: Configure Windows Defender Firewall Through Group Policy</h3>

<p>
Navigate to:
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
<img width="800" height="1634" alt="image" src="https://github.com/user-attachments/assets/572bca72-3adb-43e2-ba25-af1829182971" />

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


