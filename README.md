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

## Basic Configuration

#### Hostname
```
set system host-name "hostname"
```

#### Root Password
```
set system root-authentication plain-text-password
"...." -> Password
commit
```

#### Interface
* Interface Configuration
```
set interfaces em0 unit 0 family ipv4 address 10.10.10.1/24
commit
```

* Verification
```
run show interfaces terse
run ping 10.10.10.x
```

#### Username and Password
```
set system login user [username] class super-user
set system login user [username] authentication plain-text-password
"...." -> Password
commit
```

#### Candidate and Active Configuration
* Candidate
```
show
```

* Active
```
run show
```

#### Commit Example
* Commit
```
commit check
commit and-quit
commit at "2026-03-26 24:00:00" -> Schedule commit time
commit confirmed -> 10minutes rollback configuration (default)
commit confirmed 1 (1 minute)
commit confirmed 20 (20 minute)
commit comment "remove xxyyzz"
show system commit
```

* default commit comment maks 49 list
```
rollback 1 -> back configuration commit 1
rollback 0 -> discard candidate configuration
```

#### Date and Time
* Set Date
```
run set date 202603060100.00 -> Operational Mode
```

* Set Timezone
```
set system timezone Asia/Jakarta -> Configuration Mode
show | compare
commit
```

* Verification
```
show system uptime
```

#### NTP
```
set system ntp server id.pool.ntp.org -> Domain
set system ntp ervr 162.159.200.123 -> IP 
show | compare
commit
```

#### SSH
```
set system services ssh
```

#### Delete, Active and Deactive Mode
* Delete Configuration
```
delete ...
```

* Disable Configuration
```
deactive ...
```

* Active Configuration
```
active ...
```

#### Rename
```
rename user "user" to "user10"
show | compare
commit
```
#### Replace
```
replace pattern user "user" with "superuser"
show | compare
commit
```

#### Pipeline
```
| match [parameter]
| no-more
| display set
```

#### Backup Configuration
* Backup Configuration
```
save "backup_config"
```

* Verification Backup Configuration
```
run file list
cat "backup_config"
```

#### Restore Configuration
* Restore Configuration
```
load override "old_config"
show | compare
commit
```

#### Load Merge Configuration
* Merge Configuration
```
load merge "old_config"
show | compare
commit
```

#### Reset Configuration
* Reset
```
load factory-default
show | compare
commit
```

#### Configure
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

#### Static Routing
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

#### OSPF

#### IS-IS

