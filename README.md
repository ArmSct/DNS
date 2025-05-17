
![firefox_t9yt3TE2Df](https://github.com/user-attachments/assets/92520792-713b-4976-bfcb-d2fd8bf3dbbe)

# DNS

This lab demonstrates how to create and manage DNS records in a Windows Server Active Directory environment hosted on Microsoft Azure. The lab cover creating A-records, observing DNS caching behavior, and configuring CNAME records. A domain controller (DC-1) and client (Client-1) virtual machine are pre-configured for this lab.

## Technologies Utilized
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

## Actions and Observations

### A-Record
Login in to the client as an admin. Open the command prompt and attempt to ping "mainframe". Observe how the host "mainframe" cannot be found. No DNS record exists for that name.
![mstsc_LyVuqIEO7i](https://github.com/user-attachments/assets/b658551a-6cb8-47ad-b079-4ef5e2db8a05)

Switch to DC-1 and open the DNS Management Console. Create a new A-record named mainframe and point it to DC-1’s private IP address.

![mstsc_1SRaHZRxzc](https://github.com/user-attachments/assets/6022203a-20c4-4a6a-ae35-9bb95594dbf8)


After the record is created, return to Client-1 and try pinging mainframe again. You should now observe that the ping succeeds, confirming that the A-record is functioning and being resolved correctly by the client.
![mstsc_DO3DZA3r4C](https://github.com/user-attachments/assets/c7dbe73a-ec41-468f-b123-8076cc7a3cfa)

### Local DNS Cache

On DC-1, modify the existing A-record for mainframe to now point to a different IP address, 8.8.8.8 (Google’s public DNS server).

![mstsc_DILB9tvUWx](https://github.com/user-attachments/assets/8bd210f5-4e97-4b2a-9306-ad6cf4d4f0d9)

Return to Client-1 and ping mainframe again. Despite the change, the system will still resolve the hostname to the old IP address. This occurs due to the client’s local DNS cache. The original IP address will still be  cached locally. To clear the cache, run ipconfig /flushdns. Once cleared, use ipconfig /displaydns again to confirm that the DNS cache is empty.

![mstsc_Oq0yYbmBuY](https://github.com/user-attachments/assets/911d701c-26b8-4b06-8bf6-2338ccd54a59)

Now ping mainframe once more. You should observe that the hostname now resolves to the updated IP address (8.8.8.8), indicating that the cache has been properly refreshed.

![mstsc_YTWG0g1OP2](https://github.com/user-attachments/assets/1de35d9e-4a1f-4ebe-bed7-6c8e3cfc0b07)

### CNAME Record

On DC-1 go to the  DNS Management Console and create a CNAME (Canonical Name) record. Set the alias search to point to www.google.com.

![mstsc_whcfpcug2j](https://github.com/user-attachments/assets/bccc56fd-a0ce-4166-8ec2-b2e832241488)

On Client-1, ping search to test the CNAME resolution. Then run nslookup search to confirm it points to www.google.com, verifying the alias is working. The result will show that search is a CNAME for www.google.com, demonstrating how DNS aliases work within the domain environment.

![mstsc_REHRhkUBBJ](https://github.com/user-attachments/assets/278e03f1-56b2-4347-b256-67d5cc7a235a)
