Will add sub bullets for specific tools/scripts
# Box Assignment
- JC - martha, kelsi
- Johnathan - taylor, darbus
- Ryan - chad, jason
- Chris - troy, zeke
- Ben - sharpay, gabriella

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
- #### Reset Passwords (Rotate w/ Sheet)
- #### Check/Remove Bad Users
	- List Users ```Get-WmiObject -Class Win32_UserAccount```
	- Remove Local Users ```Remove-LocalUser -Name "<name>"```
- #### Check FW Enabled/Rules
	- Turn on ```netsh advfirewall set allprofiles state on```
	- Remove all Rules at Start ```netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound```
	- Start Logging ```netsh advfirewall set allprofiles logging filename %SystemRoot%\System32\LogFiles\Firewall\pfirewall.log```
- #### Disable WinRm (https://4sysops.com/wiki/disable-powershell-remoting-disable-psremoting-winrm-listener-firewall-and-localaccounttokenfilterpolicy/)
	 0. Stop Current Session for user if any ```Disable-PSRemoting -Force```
	 1. Stop Service Alltogether```Stop-Service WinRm -PassThruSet-Service WinRM -StartupType Disabled -PassThru```
	 2. Check For Listener and then Delete Remote Listener
		1. list listeners ```dir wsman:\localhost\listener```
		2. ```Remove-Item -Path WSMan:\Localhost\listener\<Listener Name>```
	 3. Disable Firewall Exceptions
		```Set-NetFirewallRule -DisplayName 'Windows Remote Management (HTTP-In)' -Enabled False -PassThru | Select -Property DisplayName, Profile, Enabled```
	 4. Disable remote execution with admin access token. Remote Users will trigger UAC prompt.
		```Set-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system -Name LocalAccountTokenFilterPolicy -Value 0```
- #### Check for Scheduled Tasks
	- View Running Tasks ```Get-ScheduledTask | ? State -eq Running```
	- Disable/Enable Tasks ```[Enable|Disable]-ScheduledTask -TaskName "<name>"```
- #### Check Services
- #### Monitor Procs/Subprocs
- #### Pull down Sysinternals

# Linux
- #### Reset Passwords (Rotate w/ Sheet)
- #### Enable IPtables w/ Logging from script
    - sudo iptables-restore < /etc/iptables.firewall.rules
- #### Check sudo permissions
    - Sanity Check permissions /etc/passwd /etc/shadow
        - ```ls -l /etc/shadow```
        - ```ls -l /etc/passwd```
    - Sudoer Groups only should be root + sudo
        - ```cat /etc/sudoers```
    -  Check Sudo group members
        - ```grep -Po '^sudo.+:\K.*$' /etc/group```
- #### Check/Remove Bad Users
    - ```cut -d : -f1,3,4 /etc/passwd```
- #### Check Init Scripts
- #### Check SSH Keys
- #### No remote Root login
    - ```sudo sed i- s/#PermitRootLogin.*/"PermitRootLogin no"/ /etc/ssh/sshd_config; /etc/init.d/sshd restart```
- #### Check Cron Jobs
    - ```for user in $(cut -d : -f 1 /etc/passwd; do sudo crontab -u $user -l; done > crontab_summary.txt```
	- ```sudo touch /etc/cron.d/cron.allow```, then add users as necessary
	- ```rm -f /etc/cron.deny```
- #### Check At Jobs
    - ```atq # check for at jobs```
    - ```touch /etc/at.d/at.allow```, then add root only
    - ```rm -f /etc/at.deny```
		
- #### Check PAM Modules / Backdoored /lib/security/pam_access.so
- #### Check Kernel Modules (Rootkit)
- #### Monitor Procs/Subprocs (HTOP)
- #### Remove Aliases 
    - ```unalias -a```
- #### Enable Pam Audit All user Commands
    - ```Pam Config: session    required     pam_tty_audit.so enable=*```
    - ```ausearch -ts <some_timestamp> -m tty -i```
    - ```aureport --tty```

# Solaris
- #### Reset Passwords (Rotate w/ Sheet)
- #### Enable ipfilter with proper configuration
- #### Check privileges
	- ```ppriv <pid/user>```
    - Sanity Check permissions /etc/passwd /etc/shadow
        - ```ls -l /etc/shadow```
        - ```ls -l /etc/passwd```
- #### Check/Remove Bad Users
    - ```cut -d : -f1,3,4 /etc/passwd```
- #### Check Init Scripts
- #### Secure SSH Configuration
- #### Check Cron Jobs
- #### Check At Jobs
- #### Check Kernel Modules (Rootkit)
	-	```modinfo; modunload -i 0```
- #### Monitor Procs/Subprocs (prstat)
- #### Remove Aliases
- #### Enable Pam Audit All user Commands

# Web:
-	Check Apache Modules: Apxs

# Possible TODOs
-	Static ARP tables
-       nope
-	Delete script artifacts

# Other Resources

- https://github.com/meirwah/awesome-incident-response
https://alexlevinson.wordpress.com/2017/05/09/know-your-opponent-my-ccdc-toolbox/
- https://github.com/BinaryDefense/artillery
- https://github.com/chrisjd20/Blue-Team-Cheat-Sheets/blob/master/BTCSwGSEnotes.pdf
- https://github.com/ucrcyber/CCDC/tree/master/blue-team
- https://github.com/marshyski/quick-secure/blob/master/quick-secure
- https://docs.google.com/presentation/d/1pPXLg3KqwSMLRCNRfows5QnVI2mLjSmll5vN2WHMFJg/edit#slide=id.g8e1a55d_0_5
- https://www.netresec.com/?page=networkminer
- http://www.cheat-sheets.org/saved-copy/Solaris_quickref.pdf
- http://www.tablespace.net/quicksheet/solaris-quicksheet.pdf
