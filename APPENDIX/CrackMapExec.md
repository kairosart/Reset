**CrackMapExec (CME)** is a popular post-exploitation tool used by cybersecurity professionals, penetration testers, and red teamers to assess the security of large networks, particularly Windows-based environments. It's part of the broader field of tools designed to help evaluate and strengthen the security posture of systems by simulating what an attacker might do if they gained access to the network.

## Key Features of CrackMapExec

1. **Windows Network Reconnaissance**:
    
    - CME is highly effective for enumerating information about Windows networks, such as gathering data on SMB shares, Active Directory (AD) domains, and identifying potential attack vectors.
2. **Credential Harvesting**:
    
    - It can use credentials (like NTLM hashes, Kerberos tickets, and plaintext passwords) to perform authenticated actions across the network. This feature is often used to determine the validity of credentials across multiple systems.
3. **Post-Exploitation**:
    
    - CME allows users to execute commands, run PowerShell scripts, or upload files on remote systems. This capability makes it a powerful tool for lateral movement within a compromised network.
4. **Automated Exploitation**:
    
    - The tool can automate common tasks such as spraying passwords across the network, attempting to exploit known vulnerabilities (like EternalBlue), or dumping SAM and LSASS hashes.
5. **Integration with Other Tools**:
    
    - CME integrates well with other penetration testing tools like Mimikatz, allowing for seamless transition from network enumeration to credential dumping and further exploitation.

## Installation

Visit https://www.kali.org/tools/crackmapexec/ for installing instructions on Kali Linux.

## Example Commands

- **Enumerate Shares**:
    
    `crackmapexec smb 192.168.1.0/24 --shares`
    
- **Password Spraying**:
    
    `crackmapexec smb 192.168.1.0/24 -u username -p password`
    
- **Command Execution**:
    
    `crackmapexec smb 192.168.1.10 -u admin -p password --exec 'ipconfig /all'`