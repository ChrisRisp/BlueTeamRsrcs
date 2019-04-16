### Turn on Firewall
```netsh advfirewall set allprofiles state on```
```Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True```

### Restrict Dynamic Ports
> Restricting Ports will make it harder for attacks to set up ports
```netsh interface ipv4 set dynamicportrange protocol=tcp startport=<port> numberofports=<count>```
```netsh interface ipv4 show dynamicportrange protocol=tcp```

### Disable IPv6 
> Mainly deals with the technology allowing IPv6 machines to talk to and connect to IPv4 based machines
```
netsh interface teredo set state type=disabled # disable ipv6 host ability to connect to system
netsh interface ipv6 6to4 set state=disabled undoonstop=disabled # disables transmission of v6 packets through v4
netsh interface ipv6 isatap set state=disabled # disable transition mechanism between v6 and v4 nodes
```

### Firewall Rules
netsh advfirewall firewall show rule name="Ping"
1. Delete all rules to ensure we controll all access
```
netsh advfirewall reset (netshadvfirewall set allprofiles firewallpolicy blockinbound,blockoutbound)
netsh advfirewall set allprofiles state on
netsh advfirewall firewall delete rule name=all # delete all rules to start from scratch
```
2. Set Inital to Drop Everything
```
netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound
```

3. Set up logging
```
netsh advfirewall set allprofiles logging filename %SystemRoot%\System32\LogFiles\waterfloor.log
netsh advfirewall set allprofiles logging MaxFileSize 32676
netsh advfirewall set allprofiles logging LogDroppedConnections enable
```

4. Allow DNS
```
# Note this will allow from everywhere we want to allow just from dns server
netsh advfirwall firewall add rule name="DNS Client" dir=out protocol=udp remoteport=53 action=allow profile=any enable=yes

OR

New-NetFirewallRule -Name "DNS Clent" -DisplayName "DNS Clent" -Enabled 1 -Drection Outbound -Protocol UDP -Profile Any -action Allow -RemotePort 53

# Delete previous rule and use this after DNS is set up
netsh advfirewall add rule name="DNS Client" dir=out protocol=udp remoteport=53 action=allow profile=any enable=yes remoteip=<dcip>

OR

New-NetFirewallRule -Name "DNS Clent" -DisplayName "DNS Clent" -Enabled 1 -Drection Outbound -Protocol UDP -Profile Any -action Allow -RemotePort 53 -RemoteAddress <dcip>
```

##### Jason and Zeke
Services Active : PING, DHCP
```
netsh advfirewall firewall add rule name="Ping Outbound" profile=any dir=out action=allow protocol=icmp4:8
netsh advfirewall firewall add rule name="Ping Inbound" profile=any dir=in action=alow protocol=icmp4:8

OR

New-NetFirewallRule -Name "Ping Outbound" -DisplayName "Ping Outbound" -Enabled 1 -Direction Outbound -Action Block -Profile Any -Protocol ICMPv4
New-NetFirewallRule -Name "Ping Inbound" -DisplayName "Ping Inbound" -Enabled 1 -Direction Inbound -Action Block -Profile Any -Protocol ICMPv4
```





