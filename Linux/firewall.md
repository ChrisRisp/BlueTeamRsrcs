
### Turn on Firewall

```service iptables start```

### Restrict Dynamic Ports
> Restricting Ports will make it harder for attacks to set up ports


### Disable IPv6 
> Mainly deals with the technology allowing IPv6 machines to talk to and connect to IPv4 based machines

Redhat-Based (Fedora, CentOS, RHEL)
```
sysctl -w net.ipv6.conf.all.disable_ipv6=0
sysctl -w net.ipv6.conf.default.disable_ipv6=0
sysctl -p
```

Debian-Based (Ubuntu)
```
# Fix while system is up (not persistent)
sysctl -w net.ipv6.conf.all.disable_ipv6=1 
sysctl -w net.ipv6.conf.default.disable_ipv6=1
sysctl -w net.ipv6.conf.lo.disable_ipv6=1
sysctl --system

# Create persistent variables
echo net.ipv6.conf.all.disable_ipv6=1 >> /etc/sysctl.conf
echo net.ipv6.conf.default.disable_ipv6=1 >> /etc/sysctl.conf
echo net.ipv6.conf.lo.disable_ipv6=1 >> /etc/sysctl.conf
sysctl -p
```

### Firewall Rules
1. Delete all rules to ensure we controll all access, remove extra chains, reset counters
```
iptables -F -X -Z
```
2. Set Inital to Drop Everything

```
iptables --policy INPUT DROP
iptables --policy FORWARD DROP
iptables --policy OUTPUT DROP
```

3. Set up logging
Create new file for logging
```
echo "kern.warning	/var/log/waterfloor.log" >> /etc/syslog.conf
touch /var/log/waterfloor.log
```
Start up Inbound logging
```
iptables -N LOGGING
iptables -A INPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPT-Inbound-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

Start up Outbound logging
```
iptables -N LOGGING
iptables -A OUTPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPT-Outbound-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

4. Allow DNS for CLIENTS
```

```
After Setting up DNS Use these commands to only allow traffic to dns
```

```
