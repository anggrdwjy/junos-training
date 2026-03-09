## Overview Junos

### Introduction Junos
* Build for High Performance Network
* Unix Based (FreeBSD)

### Junos Advantages
#### Separation of Resources
* Control Engine (Core System)
* Forwarding Engine (Forwarding Data)
* Service Engine (SNMP, SSH, TELNET, etc)

#### Single Operating System (Junos)
* Routing Engine (OSPF, ISIS, BGP)
* Packet Forwarding Engine (Forwarding Data)

### Junos Devices
* Switch (EX Series, QFX Series)
* Router (ACX Series, MX Series, PTX Series)
* Firewall (SRX Series)
* Cloud Virtual (vMX, vSRX, vQFX)

### Certification Track
* Routing and Switching (JNCIA, JNCIS, JNCIP, JNCIE)
* Datacenter
* Security
* Cloud
* Automation
* Design

### 3 Mode of Junos
#### Unix Mode
* cli
  
#### Operational Mode
* ping
* traceroute
* ssh
* telnet
* show

##### Confiugration Mode
  * configure
  * edit
  * set
  * run

### Navigation Configuration (Hirarcy)
* up (back hirarcy)
* up 3 (back 3 hirarcy)
* top (back to top hirarcy)

### Candidate Configuration

### Active Configuration

## Basic Configuration

### Backup Configuration
* Backup Configuration
```
save "backup_config"
```

* Verification Backup Configuration
```
run file list
cat "backup_config"
```

### Restore Configuration
* Restore Configuration
```
load override "old_config"
show | compare
commit
```

### Load Merge Configuration
* Merge Configuration
```
load merge "old_config"
show | compare
commit
```

### Reset Configuration
* Reset
```
load factory-default
show | compare
commit
```

### Configure
* Configure
```
set ...
show | compare
commit
```

* Configure Private
```
configure private
set ...
show | compare
commit
```

* Configure Exclusive
```
configure exclusive
set ...
show | compare
commit
```

## Routing Configuration

### Static Routing
* Static Route Configuration
```
set routing-options static route [destination_network] next-hop [gateway]
show | compare
commit
```

* Routing Table Static
```
show route protocol static
```

* Delete Static Route
```
delete routing-options static
show | compare
commit
```

### OSPF

### IS-IS

