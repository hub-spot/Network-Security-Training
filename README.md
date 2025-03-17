# Network-Security-Training
Network Security Ressources

### Port 25 SMTP Testing ###

Commands:

# Banner grabbing
nc -nv 192.168.x.x 25

# Check for VRFY command (user enumeration)
nc -nv 192.168.x.x 25
HELO kali
VRFY root

# Check for EXPN command (expand email lists)
nc -nv 192.168.x.x 25
HELO kali
EXPN root

# SMTP relay testing (use caution)
telnet 192.168.x.x 25
HELO kali
MAIL FROM:<attacker@example.com>
RCPT TO:<victim@example.com>
DATA
Test mail
.
QUIT

If you see "250 OK," the server may allow email relaying, which is a security risk.

### Port 79 (Finger) - User Enumeration ###
Commands:

# Check if Finger service is running
nc -nv 192.168.x.x 79
root

# Query users
finger root@192.168.x.x
finger admin@192.168.x.x
finger user@192.168.x.x

    If users exist, they will be revealed.

### Port 110 (POP3) - Email Retrieval ###
Commands:

# Connect to POP3 service
nc -nv 192.168.x.x 110

# Try logging in with default creds
USER root
PASS toor

# List emails
LIST

If credentials are discovered elsewhere, attempt authentication.


### Port 139 (NetBIOS) & 445 (SMB) - Windows File Sharing ###
Commands:

# Check for SMB shares
smbclient -L \\\\192.168.x.x\\

# Try accessing without authentication
smbclient \\\\192.168.x.x\\sharename -N

# Enumerate SMB vulnerabilities
nmap --script=smb-vuln* -p139,445 192.168.x.x

# Check for null sessions
rpcclient -U "" 192.168.x.x

Look for Null Session Access and potential SMB exploits (e.g., EternalBlue CVE-2017-0144).

### Port 3306 MYSQL Testing ###
Commands:

# Check MySQL version
mysql -h 192.168.x.x -u root -p

# If anonymous login is enabled, try:
mysql -h 192.168.x.x -u anonymous -p

# Check for open databases
SHOW DATABASES;

If credentials are leaked elsewhere, try logging in.




### Port 5985 WS Man ###
Port 5985 (WinRM) - Windows Remote Management
Commands:

# Check if authentication is required
evil-winrm -i 192.168.x.x -u administrator -p 'password'

# Enumerate WinRM with Nmap
nmap -p5985 --script=http-winrm* 192.168.x.x

If valid Windows credentials are found, you can use Evil-WinRM to gain access.
