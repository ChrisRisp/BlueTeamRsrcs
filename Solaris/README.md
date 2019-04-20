# Solaris Firewall Configuration

Solaris uses ipfilter to filter traffic so here is a basic
ipfilter configuration profile that should allow us to do most 
of what we want to during the competition.

The following are useful commands for working with ipfilter.
Note: you must have certain priviliges to enable ipfilter

```
svcadm enable network/ipfilter # enable the service
svcs -xv                       # check for errors
ipf -E                         # enable ipfilter
ipf -f firewall.conf           # enable configuration
```

