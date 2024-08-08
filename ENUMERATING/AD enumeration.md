While you are on SMB realize that the files here are being renamed or replaced. This means that a user accesses this share regularly. 
Since the file names are changing we can come to an assumption that someone might be accessing the contents in the `**onboarding**` directory. Let's try to make use of this to steal a `**NTLM**` hash of the user that might be making the changes within the share.

We also note that we have write access to the share.

## NTLM Theft

A tool for generating multiple types of NTLMv2 hash theft files.

ntlm_theft is an Open Source Python3 Tool that generates 21 different types of hash theft documents. These can be used for phishing when either the target allows smb traffic outside their network, or if you are already inside the internal network.  More [info](https://github.com/Greenwolf/ntlm_theft).

