Run nmap to find out what services and port are opened on the victim machine.

`nmap -A \<MACHINE IP\> -Pn`


![[Screenshot from 2024-08-07 19-44-09.png]]

The NMAP scan reveals that we deal with a Windows host with various open ports and services. Among them are services like:
DNS on port 53, Kerberos on port 88, Microsoft Windows RPC on ports 135, 49669 to 49702,  SMB on port 139/445, Microsoft Windows Active Directory LDAP on ports 389 and 3268, Microsoft Terminal Services on port 3389, and Microsoft HTTPAPI httpd on port 5985.

You can see the AD Domain mentioned in the info on port 3389. The domain is called **thm.corp**.

![[Screenshot from 2024-08-07 20-46-03.png]]

**Next step:** [[SMB service]]
