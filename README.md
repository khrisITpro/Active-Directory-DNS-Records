<p align="center">
<img src="https://i.imgur.com/CtGfsq8.png" alt="osTicket logo"/>
</p>

<h1>Active Directory DNS Records</h1>
In this lab, I explore and experiment with the Domain Name System (DNS) to gain a deeper understanding of how it operates and supports network communication. Through practical exercises like like creating A-records on a domain controller's "mainframe" to Ping, observe local dns cache, and creating CNames, the aim is to enhance my knowledge of DNS functionality and its critical role in translating domain names into IP addresses.<br />

<h2>Environments and Technologies Used</h2>

- Remote Desktop
- Powershell

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022 </b> 

<h2>List of Prerequisites</h2>

- A Virtual Machine with Active Directory
- A Client VM joined to your Network 

<h2>Lab Steps</h2>
<p>
<img width="1675" height="981" alt="dns" src="https://github.com/user-attachments/assets/1c51b971-ae28-4585-992a-41a6c6c52b41" />
</p>
<p>
First, start the VMs and RDP into Client1. Open PowerShell as an administrator and attempt to ping "mainframe." This will fail because "mainframe" is not found in the local cache, the hosts file, or the DNS server.
</p>
<br />

<p>

</p><img width="1328" height="777" alt="dns2" src="https://github.com/user-attachments/assets/f20194be-cb73-4ef3-a81d-e9a4b6c4dd3f" />

<p>
Next, run the command ipconfig /displaydns. This will show that there is no entry for "mainframe" in the DNS cache.
</p>
<br />

<p>
<img width="1316" height="150" alt="dns3" src="https://github.com/user-attachments/assets/0659bf81-7ac9-4d86-8b77-459ee5ba03ce" />

</p>
<p>
After that, run the command nslookup mainframe. Once again, you'll find that no results are returned.
</p>
<br />

<p>
<img width="1328" height="798" alt="dns4" src="https://github.com/user-attachments/assets/46603c07-46b1-43a5-80d2-a0e8c72d9833" />

</p>
<p>
Next, create a DNS A-record on DC-1 for "mainframe" and point it to DC-1's private IP address. On DC-1, search for Administrative Tools and open DNS Manager. Navigate to DC-1, Forward Lookup Zones, mydomain.com, right-click inside mydomain.com and, select New Host. In the Name field, enter "mainframe", and in the IP Address field, input DC-1's private IP address. Save the record.
</p>
<br />

<p>
<img width="1662" height="437" alt="dns5" src="https://github.com/user-attachments/assets/4d6d6978-c425-48fe-a355-510020097bfd" />
</p>
<p>
Now, return to Client1 and try pinging "mainframe" in PowerShell again. This time, the ping will succeed because we created a DNS A-record on DC-1 pointing to its private IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/8nSHNtq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, go back to DC-1 and update the DNS A-record for "mainframe" to point to the IP address 8.8.8.8. After making this change, return to the Client1 VM and ping "mainframe" again to notice it still pings the old IP address. Use the command ipconfig /displaydns on Client1 to locate the entry for "mainframe". The A-record still points to 10.0.0.4, providing a more detailed view of the cached DNS entry.
</p>
<br />

<p>
<img width="1684" height="1011" alt="dns6" src="https://github.com/user-attachments/assets/277d56b7-463d-44ed-a28a-2c3e92433498" />

</p>
<p>
To ensure that the new IP address from the updated A-record is reflected, use the command ipconfig /flushdns to clear the DNS cache. The new IP address will be shown when you use ipconfig /displaydns.
</p>
<br />

<p>
<img width="1661" height="404" alt="dns7" src="https://github.com/user-attachments/assets/9b916141-70e4-4837-8058-618c1a14d0d5" />

</p>
<p>
Now, attempt to ping "mainframe" one more time from Client1. You'll notice that it successfully pings the new IP address 8.8.8.8, reflecting the updated A-record.
</p>
<br />

<p>
<img width="1912" height="1071" alt="dns8" src="https://github.com/user-attachments/assets/53900e24-e9b4-4975-bf86-2975cb11885c" />

</p>
<p>
Next, go back to the DC-1 VM and create a CNAME record that points the host "explore" to "www.google.com". To do this, navigate to the DNS Manager, right-click mydomain.com under Forward Lookup Zones, and select New Alias (CNAME). In the Alias name field, enter explore, and in the Fully Qualified Domain Name (FQDN) field, enter www.google.com. Save the record. Go back to Client 1 to ping "explore" to show that it points to the address you assigned it to. Run nslookup to show that "explore" resolves to google.com, which should indicate that the CNAME record works correctly.
</p>
<br />
