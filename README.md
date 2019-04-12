Will add sub bullets for specific tools/scripts

# Possible Roles:
-	Linux
-	Windows/AD
-	Web
-	Email
-	FTP/SMB/Samba
-	DNS
-	SSH
-	MYSQL
- IR Report Author 

# Windows:
-	Reset Passwords (Rotate w/ Sheet)
-	Check/Remove Bad Users
-	Check FW Enabled/Rules
-	Disable WinRm/RDP
-	Check for Scheduled Tasks
-	Check Services
-	Monitor Procs/Subprocs
-	Pull down Sysinternals

# Linux
-	Reset Passwords (Rotate w/ Sheet)
-	Enable IPtables w/ Logging from script
-    Check sudo permissions
     -  /// Sudoer Groups only should be root + sudo
     -  cat /etc/sudoers
     -  /// Check Sudo group members
     - grep -Po '^sudo.+:\K.*$' /etc/group
-	Check/Remove Bad Users
-	Check Init Scripts
-	Check SSH Keys
-	Check Cron Jobs
-	Check PAM Modules / Backdoored /lib/security/pam_acccess.so
-	Check Kernel Modules (Rootkit)
- Monitor Procs/Subprocs (HTOP)
- Remove Aliases: unalias -a
-	Enable Pam Audit All user Commands
     - Pam Config: session    required     pam_tty_audit.so enable=*
     - ausearch -ts <some_timestamp> -m tty -i
     -   aureport --tty

# Web:
-	Check Apache Modules: Apxs

# Possible TODOs
-	Static ARP tables	

# Other Resources

- https://github.com/meirwah/awesome-incident-response
- https://github.com/BinaryDefense/artillery
- https://github.com/chrisjd20/Blue-Team-Cheat-Sheets/blob/master/BTCSwGSEnotes.pdf
- https://github.com/ucrcyber/CCDC/tree/master/blue-team
- https://github.com/marshyski/quick-secure/blob/master/quick-secure
- https://docs.google.com/presentation/d/1pPXLg3KqwSMLRCNRfows5QnVI2mLjSmll5vN2WHMFJg/edit#slide=id.g8e1a55d_0_5
- https://www.netresec.com/?page=networkminer
