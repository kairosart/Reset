You can use smbclient or another tool, but this time use [[CrackMapExec]].

Run CrackMapExec on your attacking machine:

`crackmapexec smb \<MACHINE IP\> --shares -u 0xb0b -p ''`

![[Screenshot from 2024-08-08 20-03-10.png]]

## Connect via smbclient

You can connect with smbclient without any password.

`smbclient \\\\10.10.179.59\\data -U ""`

- List the files
	`smb: \> dir`
	![[Screenshot from 2024-08-08 20-09-47.png]]

- Go to the onboarding directory an list the files.
	`smb: \> cd onboarding`
	`smb: \onboarding\> dir`
	
	![[Screenshot from 2024-08-08 20-12-32.png]]
	
	You can see there are 3 files.

- Download the files recursively.

	`mask ""`
	`recurse ON` 
	`prompt OFF`
	`mget *`



## Checking out the files

### txt file

![[Screenshot from 2024-08-08 21-01-02.png]]

In this file there's a \<USER\> tag but we don't know the user's name.

### PDF file
On one of the pdf files you'll find the following slice with an user's credentials.

![[Screenshot from 2024-08-08 20-29-00.png]]

In this file the name LILY ONEILL doesn't seem to be an user's name.
There's a password: **ResetMe123!**


**Next step:** [[AD enumeration]]