BloodHound uses graph theory to model the relationships within an Active Directory environment. It ingests data about users, groups, computers, sessions, and ACLs (Access Control Lists) to create a graph that represents all possible attack paths.

More info about installation and execution [here](https://github.com/dirkjanm/BloodHound.py/tree/bloodhound-ce).

Since you have credentials, we can run `bloodhound` to get the Active Directory structure for this environment :

 `bloodhound-python -d thm.corp -u 'AUTOMATE' -p 'Passw0rd1' -ns MACHINE IP -c All`

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

## Analyzing the BloodHound data

- In the `Analysis` section, you can find `Pre-Built Analytics Queries`.
- Run the `Find AS-REP Roastable Users (DontReqPreAuth)` Query.
	![[Screenshot from 2024-08-13 20-21-19.png]]
	There are 3 users that do not have `Kerberos pre-authentication` enabled.

### Transitive Object Control

Transitive Object Control are objects this user can gain control of by performing ACL-only-based attacks in Active Directory. It's represented as a number. In other words, the maximum it is the number of objects the user can gain control of without needing to pivot to any other system in the network, just by manipulating objects in the directory

- Select the node for `TABATHA_BRITT` and display all `Transitively Controllable Objects`.
	![[Screenshot from 2024-08-13 20-58-38.png]]
	![[Screenshot from 2024-08-13 21-31-00.png]]
	You have:
	`GenericAll` from `TABATHA_BRITT` to `SHAWMA_BRAY`.
	
	`ForceChangePassword` from `SHAWMA_BRAY` to `CRUZZ_HALL`.
	
	`GenericWrite` from `CRUZ_HALL` to `DARLA_WINTERS`.