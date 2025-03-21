# azure-network-protocols
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

<h2>Dealing with Account Lockouts</h2>

<p>
In this lab, we will simulate a scenario where multiple incorrect login attempts trigger an account lockout. 
This is an essential security measure to prevent brute-force password attacks.
</p>

<h3>Step 1: Log into DC-1</h3>

<ul>
  <li>Ensure that <strong>DC-1</strong> is powered on in the Azure Portal.</li>
  <li>Use an account we created in the previous lab.</li>
  <ul>
    <li><strong>Username:</strong> <code>mydomain.com\randomuser from powershell</code></li>
    <li><strong>Password:</strong> <code>password1</code></li>
    <li>Open the Group Policy management console and create the GPO.</li>
  </ul>
</ul>

<h3>Step 2: Select a Test User Account</h3>

<ul>
  <li>Open <strong>Active Directory Users and Computers (ADUC)</strong>.</li>
  <li>Navigate to the <code>_EMPLOYEES</code> or <code>_CLIENTS</code> OU.</li>
  <li>Pick a random user account that was previously created.</li>
</ul>

<h3>Step 3: Attempt to Log In with Incorrect Passwords</h3>

<ul>
  <li>Log out of DC-1 or switch to a different system (Client-1).</li>
  <li>Try logging in with the chosen user account.</li>
  <li>Enter an incorrect password and attempt to log in.</li>
  <li>Repeat this process <strong>10 times</strong> to trigger an account lockout.</li>
</ul>

<h3>Step 4: Observe the Lockout Behavior</h3>

<ul>
  <li>After several failed attempts, the system should prevent further login attempts.</li>
  <li>If the account is locked, an error message should appear stating that the account has been locked out.</li>
  <li>Take note of how many attempts it took for the lockout to occur.</li>
</ul>

<h3>Next Steps</h3>

<p>
Now that we have observed an account lockout, we will move on to configuring and managing lockout policies in the next steps.
</p>











<h2>Dealing with Account Lockouts</h2>

<p>
In this lab, we will simulate a scenario where multiple incorrect login attempts trigger an account lockout. 
This is an essential security measure to prevent brute-force password attacks.
</p>

<h3>Step 1: Log into DC-1</h3>

<ul>
  <li>Ensure that <strong>DC-1</strong> is powered on in the Azure Portal.</li>
  <li>Log in using the domain administrator account:</li>
  <ul>
    <li><strong>Username:</strong> <code>mydomain.com\jane_admin</code></li>
    <li><strong>Password:</strong> <code>Cyberlab123!</code></li>
  </ul>
</ul>

<h3>Step 2: Select a Test User Account</h3>

<ul>
  <li>Open <strong>Active Directory Users and Computers (ADUC)</strong>.</li>
  <li>Navigate to the <code>_EMPLOYEES</code> or <code>_CLIENTS</code> OU.</li>
  <li>Pick a random user account that was previously created.</li>
</ul>

<h3>Step 3: Attempt to Log In with Incorrect Passwords</h3>

<ul>
  <li>Log out of DC-1 or switch to a different system (Client-1).</li>
  <li>Try logging in with the chosen user account.</li>
  <li>Enter an incorrect password and attempt to log in.</li>
  <li>Repeat this process <strong>10 times</strong> to trigger an account lockout.</li>
</ul>

<h3>Step 4: Observe the Lockout Behavior</h3>

<ul>
  <li>After several failed attempts, the system should prevent further login attempts.</li>
  <li>If the account is locked, an error message should appear stating that the account has been locked out.</li>
  <li>Take note of how many attempts it took for the lockout to occur.</li>
</ul>

<h3>Step 5: Configure Group Policy to Lockout the Account After 5 Attempts</h3>

<p>
To strengthen security, we will modify the Group Policy settings so that accounts are locked out after 5 failed login attempts.
</p>

<ul>
  <li>Log into <strong>DC-1</strong> as <code>jane_admin</code>.</li>
  <li>Open the <strong>Group Policy Management</strong> console (<code>gpmc.msc</code>).</li>
  <li>Navigate to: <code>Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy</code>.</li>
  <li>Double-click on <strong>Account lockout threshold</strong> and set it to <strong>5</strong>.</li>
  <li>Click <strong>OK</strong> and confirm the suggested values for lockout duration and reset time.</li>
  <li>Close the Group Policy Editor and run the following command in PowerShell to apply changes immediately:</li>
</ul>

<pre><code>gpupdate /force</code></pre>

<p>
After configuring this policy, any user who enters an incorrect password <strong>5 times</strong> will be locked out.
</p>

<h3>Configuring an Account Lockout Policy in Active Directory</h3>

<p>
Configuring an account lockout policy in Active Directory using Group Policy involves defining settings that control when an account is locked after multiple failed login attempts, 
how long the account remains locked, and how the lockout counter is reset. Here’s a step-by-step guide on how to do this:
</p>

<h3>Step 1: Open the Group Policy Management Console (GPMC)</h3>
<ul>
  <li>Log in to a machine with Group Policy Management Console installed (typically, a Domain Controller).</li>
  <li>Click Start, type <code>gpmc.msc</code> in the search box, then press Enter.</li>
</ul>

<h3>Step 2: Create or Edit a Group Policy Object (GPO)</h3>
<ul>
  <li>In GPMC, navigate to the <strong>Group Policy Objects</strong> section.</li>
  <li>Right-click <strong>Group Policy Objects</strong> and select <strong>New</strong> to create a new GPO, or edit an existing GPO.</li>
  <li>Give the GPO a descriptive name, such as <strong>Account Lockout Policy</strong>.</li>
</ul>

<h3>Step 3: Navigate to the Account Lockout Policy Settings</h3>
<ul>
  <li>In the Group Policy Management Editor, expand:</li>
  <ul>
    <li><code>Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy</code></li>
  </ul>
</ul>

<h3>Step 4: Configure Account Lockout Policy Settings</h3>
<ul>
  <li><strong>Account Lockout Duration:</strong> Set the time (in minutes) that an account remains locked before it is automatically unlocked.</li>
  <ul>
    <li>Double-click this setting, select <strong>Define this policy setting</strong>, and set the duration (e.g., <strong>30 minutes</strong>).</li>
  </ul>
  <li><strong>Account Lockout Threshold:</strong> The number of failed logon attempts before an account is locked.</li>
  <ul>
    <li>Double-click this setting, select <strong>Define this policy setting</strong>, and set it to <strong>3 attempts</strong>.</li>
  </ul>
  <li><strong>Reset Account Lockout Counter After:</strong> Time (in minutes) after which the failed logon attempts counter resets.</li>
  <ul>
    <li>Double-click this setting, select <strong>Define this policy setting</strong>, and set the time (e.g., <strong>15 minutes</strong>).</li>
  </ul>
</ul>

<h3>Step 5: Link the GPO to an Organizational Unit (OU)</h3>
<ul>
  <li>Once the GPO is configured, you need to link it to the appropriate Organizational Unit (OU) or domain where you want the policy to apply.</li>
  <li>In GPMC, right-click the <strong>OU or domain</strong> where you want to apply the GPO and select <strong>Link an Existing GPO</strong>.</li>
  <li>Choose the GPO you created and click <strong>OK</strong>.</li>
</ul>

<h3>Step 6: Update Group Policy</h3>
<ul>
  <li>To apply the policy immediately, open Command Prompt and run:</li>
</ul>

<pre><code>gpupdate /force</code></pre>

<h3>Step 7: Verify the Policy</h3>
<ul>
  <li>Use <code>rsop.msc</code> (Resultant Set of Policy) on a client machine to see the applied settings.</li>
  <li>Alternatively, check the policy using Group Policy Management Console.</li>
</ul>

<h3>Important Considerations</h3>
<ul>
  <li><strong>Account Lockout Threshold:</strong> Setting this too low (e.g., 1 or 2 attempts) can lead to unnecessary lockouts.</li>
  <li><strong>Account Lockout Duration:</strong> Setting this too high can be inconvenient for users but increases security.</li>
  <li><strong>Reset Account Lockout Counter After:</strong> Setting this too short could allow attackers to repeatedly attempt to log in without triggering a lockout.</li>
</ul>

<p>
By following these steps, you can successfully configure an account lockout policy in Active Directory using Group Policy.
</p>

































<br />
