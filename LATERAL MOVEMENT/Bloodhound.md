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
	