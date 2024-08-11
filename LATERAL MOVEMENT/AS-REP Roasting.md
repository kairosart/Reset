AS-REP Roasting dumps the krbasrep5 hashes of user accounts that have Kerberos **pre-authentication disabled**, which we can then attempt to crack offline. Unlike Kerberoasting these users do not have to be service accounts the only requirement to be able to AS-REP roast a user is the user must have pre-authentication disabled.

You have credentials from the `AUTOMATE` user.

Use `GetNPUsers.py`, alternatively, we could also use Bloodhound to find them.

### Impackets script `GetNPUsers.py`

`GetNPUsers.py` can be used to query those users; it will attempt to list and get TGTs for those users that have the property 'Do not require Kerberos preauthentication' set (UF_DONT_REQUIRE_PREAUTH).

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
