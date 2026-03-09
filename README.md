## Overview Junos

#### Introduction Junos
* Build for High Performance Network
* Unix Based (FreeBSD)

#### Information
* Overview Junos
* Junos Advantages
* 3 Mode of Junos
* Basic Configuration
* Routing Configuration
* Routing Policy
* Redistribution Policy
* Firewall Filter

## Junos Advantages
#### 1. Separation of Resources
* Control Engine (Core System)
* Forwarding Engine (Forwarding Data)
* Service Engine (SNMP, SSH, TELNET, etc)

#### 2. Single Operating System (Junos)
* Routing Engine (OSPF, ISIS, BGP)
* Packet Forwarding Engine (Forwarding Data)

#### 3. Junos Devices
* Switch (EX Series, QFX Series)
* Router (ACX Series, MX Series, PTX Series)
* Firewall (SRX Series)
* Cloud Virtual (vMX, vSRX, vQFX)

#### 4. Certification Track
* Routing and Switching (JNCIA, JNCIS, JNCIP, JNCIE)
* Datacenter
* Security
* Cloud
* Automation
* Design

## 3 Mode of Junos
#### 1. Unix Mode (Root Privilege)
* cli
  
#### 2. Operational Mode (User Privilege)
* ping
* traceroute
* ssh
* telnet
* show

#### 3. Confiugration Mode (Root and User Privilege)
  * configure
  * edit
  * set
  * run

## Basic Configuration

#### 1. Navigation Configuration (Hirarcy)
* up (back hirarcy)
* up 3 (back 3 hirarcy)
* top (back to top hirarcy)

#### 2. Hostname
```
set system host-name "hostname"
commit
```

#### 3. Root Password
```
set system root-authentication plain-text-password
"...." -> Password
commit
```

#### 4. Username and Password
* Superuser
```
set system login user [username] class super-user
set system login user [username] authentication plain-text-password
"...." -> Password
commit
```

* Operator
```
set system login user [username] class operator
set system login user [username] authentication plain-text-password
"...." -> Password
commit
```

* Admin
```
set system login user [username] class admin
set system login user [username] authentication plain-text-password
"...." -> Password
commit
```

#### 5. Candidate and Active Configuration
* Candidate
```
show
```

* Active
```
run show
```

#### 6. Commit Example
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

#### 7. Date and Time
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

#### 8. NTP (Network Time Protocol)
```
set system ntp server id.pool.ntp.org -> Domain
set system ntp ervr 162.159.200.123 -> IP 
show | compare
commit
```

#### 9. SSH (Default Port 22)
```
set system services ssh
```

#### 10. Delete, Active and Deactive Mode
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

#### 11. Rename
```
rename user "user" to "user10"
show | compare
commit
```
#### 12. Replace
```
replace pattern user "user" with "superuser"
show | compare
commit
```

#### 13. Pipeline
```
| match [parameter]
| no-more
| display set
```

#### 14. Backup Configuration
* Backup Configuration
```
save "backup_config"
```

* Verification Backup Configuration
```
run file list
cat "backup_config"
```

#### 15. Restore Configuration
* Restore Configuration
```
load override "old_config"
show | compare
commit
```

#### 16. Load Merge Configuration
* Merge Configuration
```
load merge "old_config"
show | compare
commit
```

#### 17. Reset Configuration
* Reset
```
load factory-default
show | compare
commit
```

#### 18. Configure
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

#### 19. Interface Configuration
* Loopback Configuration
```
set interfaces lo0 unit 0 description
set interfaces lo0 unit 0 family inet address 1.1.1.1/32
commit
```

* Interface Configuration
```
set interfaces em0 unit 0 description
set interfaces em0 unit 0 family inet address 10.10.10.1/24
commit
```

* Verification
```
run show interfaces terse
run show interfaces brief
run ping 10.10.10.x
```

## Routing Configuration

#### 1. Static Routing
* Static Route Configuration
```
set routing-options static route "destination_network" next-hop "gateway"
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

#### 2. OSPF Routing
* Router ID
```
set routing-options router-id 1.1.1.1
```

* OSPF Interface Single Area
```
set protocols ospf area 1 interface em0
set protocols ospf area 1 interface em1
set protocols ospf area 1 interface lo0
show | compare
commit
```

* OSPF Interface Multi Area
```
set routing-options router-id 2.2.2.2
set protocols ospf area 1 interface em0
set protocols ospf area 2 interface em1
set protocols ospf area 0 interface lo0
show | compare
commit
```

* Verification
```
show configuration routing-options
show configuration protocols ospf
show ospf neighbor
show ospf route
show route protocol ospf
```

* Migration Area OSPF (Rename Area)
```
edit protocols ospf
rename area 0 to area 100
show | compare
commit
```

#### 3. IS-IS

