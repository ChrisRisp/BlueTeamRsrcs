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
-	#### Reset Passwords (Rotate w/ Sheet)
-	#### Check/Remove Bad Users
	- List Users```Get-WmiObject -Class Win32_UserAccount```
-	#### Check FW Enabled/Rules
-	#### Disable WinRm (https://4sysops.com/wiki/disable-powershell-remoting-disable-psremoting-winrm-listener-firewall-and-localaccounttokenfilterpolicy/)
	- 0. Stop Current Session for user if any ```Disable-PSRemoting -Force```
	- 1. Stop Service Alltogether```Stop-Service WinRm -PassThruSet-Service WinRM -StartupType Disabled -PassThru```
	- 2. Check For Listener and then Delete Remote Listener
		- 1. list listeners ```dir wsman:\localhost\listener```
		- 2. ```Remove-Item -Path WSMan:\Localhost\listener\<Listener Name>```
	- 3. Disable Firewall Exceptions
		- ```Set-NetFirewallRule -DisplayName 'Windows Remote Management (HTTP-In)' -Enabled False -PassThru | Select -Property DisplayName, Profile, Enabled```
	- 4. Disable remote execution with admin access token. Remote Users will trigger UAC prompt.
		- ```Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system -Name LocalAccountTokenFilterPolicy -Value 0```
-	#### Check for Scheduled Tasks
-	#### Check Services
-	#### Monitor Procs/Subprocs
-	#### Pull down Sysinternals

# Linux
-	#### Reset Passwords (Rotate w/ Sheet)
-	#### Enable IPtables w/ Logging from script
-       #### Check sudo permissions
     	- Sanity Check permissions /etc/passwd /etc/shadow
         	- ```ls -l /etc/shadow```
          	- ```ls -l /etc/passwd```
     	-  Sudoer Groups only should be root + sudo
           	- ```cat /etc/sudoers```
     	-  Check Sudo group members
         	- ```grep -Po '^sudo.+:\K.*$' /etc/group```
-	#### Check/Remove Bad Users
        	- ```cut -d : -f1,3,4 /etc/passwd```
-	#### Check Init Scripts
-	#### Check SSH Keys
-       #### No remote Root login
          	- ```sudo sed i- s/#PermitRootLogin.*/"PermitRootLogin no"/ /etc/ssh/sshd_config; /etc/init.d/sshd restart```
-	#### Check Cron Jobs
          	- ```for user in $(cut -d : -f 1 /etc/passwd; do sudo crontab -u $user -l; done > crontab_summary.txt```
-	#### Check PAM Modules / Backdoored /lib/security/pam_acccess.so
-	#### Check Kernel Modules (Rootkit)
-       #### Monitor Procs/Subprocs (HTOP)
-       #### Remove Aliases 
          	```unalias -a```
-	#### Enable Pam Audit All user Commands
     		- ```Pam Config: session    required     pam_tty_audit.so enable=*```
      	 	- ```ausearch -ts <some_timestamp> -m tty -i```
        	- ```aureport --tty```

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
