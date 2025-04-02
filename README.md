# azure-network-protocols
<p align="center">
  
![istockphoto-1467163915-612x612](https://github.com/user-attachments/assets/520decba-8294-4e91-b7f9-c7ca27e2b6a1)

</p>

<h1>Network Security Groups (NSGs) and Configuring Accounts</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

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

![image](https://github.com/user-attachments/assets/5411f055-4e26-45b7-8d5b-5ea25d4bb24c)


<h3>Step 3: Attempt to Log In with Incorrect Passwords</h3>

<ul>
  <li>Log out of DC-1 or switch to a different system (Client-1).</li>
  <li>Try logging in with the chosen user account.</li>
  <li>Enter an incorrect password and attempt to log in.</li>
  <li>Repeat this process <strong>10 times</strong> to trigger an account lockout.</li>
</ul>

![image](https://github.com/user-attachments/assets/e70fb52e-e01f-4d97-ad66-7315ce225895)


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

![image](https://github.com/user-attachments/assets/e94e6288-bdf5-4275-8a77-1f72baaff89b)


![image](https://github.com/user-attachments/assets/6a920299-2f67-4ac5-b82d-4f8e14f380c9)

<pre><code>gpupdate /force</code></pre>

![image](https://github.com/user-attachments/assets/61cc155c-01b8-40ca-a72e-5aa26c5f6182)

<p>
After configuring this policy, any user who enters an incorrect password <strong>5 times</strong> will be locked out.
</p>


![image](https://github.com/user-attachments/assets/8f3f87bc-53ba-464d-9f9b-a7414428c082)


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



















<h3>Enabling and Disabling Accounts</h3>

<ul>
  <li>Search up and find the same account in active directory</li>
  <li>Right click the account and disable it. If you search it again it contains a down arrow next to the icon.</li>
  <li>Try to re-log with the disabled account. It should refuse entry.</li>
  <li>Try enabling the account, and once more try logging in. This time it should work.</li>
</ul>

![image](https://github.com/user-attachments/assets/d2a0905a-173f-4b6d-927b-e8d52b57cee2)


<h3>Observing Logs</h3>

<ul>
<li>Go to the taskbar and search eventvwr.msc, which will let you see authentication logs/</li>
<li>Expand "windows logs" and then select security. </li>
<li>Right click it and choose "find". We will see the logs of the user we just logged in with.</li>
<li>Click "Find Next" until you find the failures that occured when we tried to log in and failed.</li>
<li>You should eventually see Audit Failures, click on that.</li>

![image](https://github.com/user-attachments/assets/48a1701a-6e0e-4293-b679-5a594bc431f9)

  
<li>These are the failures we can observe when we tried to log in with a incorrect password.</li>

<p>This is the end of the lab with Active Directory. Although this scratches the surface, there are many things you can do with Active Directory. </p>
  
</ul>





























<br />
