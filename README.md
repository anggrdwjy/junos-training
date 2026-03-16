## Overview Junos

<p align="center">
<img src="img/juniperlogo2.png">
</p>

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
```
root@:~ # cli
root>
```
  
#### 2. Operational Mode (User Privilege)
```
root> ping
root> traceroute
root> ssh
root> telnet
root> show
```

#### 3. Confiugration Mode (Root and User Privilege)
```
root> configure
root> run
root# edit
root# set
```
## Basic Configuration

#### 1. Navigation Configuration (Hirarcy)
* up (back hirarcy)
* up 3 (back 3 hirarcy)
* top (back to top hirarcy)

#### 2. Hostname
  ```
  root# set system host-name "hostname"
  ```

#### 3. Root Password
  ```
  root# set system root-authentication plain-text-password
  "...." -> Password
  root# commit
  ```

#### 4. Username and Password
* Superuser
  ```
  root# set system login user [username] class super-user
  root# set system login user [username] authentication plain-text-password
  "...." -> Password
  root# commit
  ```

* Operator
  ```
  root# set system login user [username] class operator
  root# set system login user [username] authentication plain-text-password
  "...." -> Password
  root# commit
  ```

* Admin
  ```
  root# set system login user [username] class admin
  root# set system login user [username] authentication plain-text-password
  "...." -> Password
  root# commit
  ```

#### 5. Candidate and Active Configuration
* Candidate
  ```
  root> show
  ```

* Active
  ```
  root# run show
  ```

#### 6. Commit Example
* Commit
  ```
  root# commit check
  root# commit and-quit
  root# commit at "2026-03-26 24:00:00" -> Schedule commit time
  root# commit confirmed -> 10minutes rollback configuration (default)
  root# commit confirmed 1 (1 minute)
  root# commit confirmed 20 (20 minute)
  root# commit comment "remove xxyyzz"
  root# show system commit
  ```

* Rollback commit comment maks 49 list
  ```
  root# rollback 1 -> back configuration commit 1
  root# rollback 0 -> discard candidate configuration
  ```

#### 7. Date and Time
* Set Date
  ```
  root> run set date 202603060100.00 -> Operational Mode
  ```

* Set Timezone
  ```
  root# set system timezone Asia/Jakarta -> Configuration Mode
  root# show | compare
  root# commit
  ```

* Verification
  ```
  root# show system uptime
  ```

#### 8. NTP (Network Time Protocol)
```
root# set system ntp server id.pool.ntp.org -> Domain
root# set system ntp ervr 162.159.200.123 -> IP 
root# show | compare
root# commit
```

#### 9. SSH (Default Port 22), FTP, TELNET
```
root# set system services ssh
root# set system services ftp
root# set system services telnet
```

#### 10. Delete, Active and Deactive Mode
* Delete Configuration
  ```
  root# delete ...
  ```

* Disable Configuration
  ```
  root# deactive ...
  ```

* Active Configuration
  ```
  root# active ...
  ```

#### 11. Rename and Replace Parameter
* Rename Example
  ```
  root# edit system
  root# rename user "user" to "user10"
  root# show | compare
  root# commit
  ```

* Replace Example
  ```
  root# edit system
  root# replace pattern user "user" with "superuser"
  root# show | compare
  root# commit
  ```

#### 12. Pipeline
```
root# show | match [parameter]
root# show | no-more
root# show | display set
```

#### 13. Backup, Restore, Load Merge Configuration
* Backup Configuration
  ```
  root# save "backup_config"
  ```

* Verification Backup Configuration
  ```
  root# run file list
  root# cat "backup_config"
  ```

* Restore Configuration
  ```
  root# load override "old_config"
  root# show | compare
  root# commit
  ```

* Load Merge Configuration
  ```
  root# load merge "old_config"
  root# show | compare
  root# commit
  ```

#### 14. Reset Configuration
* Reset Factory Default
  ```
  root# load factory-default
  root# show | compare
  root# commit
  ```

#### 15. Configure
* Configure
  ```
  root# set ...
  root# show | compare
  root# commit
  ```

* Configure Private
  ```
  root# configure private
  root# set ...
  root# show | compare
  root# commit
  ```

* Configure Exclusive
  ```
  root# configure exclusive
  root# set ...
  root# show | compare
  root# commit
  ```

#### 16. Interface Configuration
* Loopback Configuration
  ```
  root# set interfaces lo0 unit 0 description
  root# set interfaces lo0 unit 0 family inet address 1.1.1.1/32
  root# commit
  ```

* Interface Configuration
  ```
  root# set interfaces em0 unit 0 description
  root# set interfaces em0 unit 0 family inet address 10.10.10.1/24
  root# commit
  ```

* Verification
  ```
  root> run show interfaces terse
  root> run show interfaces brief
  root> run ping 10.10.10.x
  ```

## Routing Configuration

#### 1. Static Routing
* Static Route Configuration
  ```
  root# set routing-options static route "destination_network" next-hop "gateway"
  root# show | compare
  root# commit
  ```

* Routing Table Static
  ```
  root# show route protocol static
  ```

* Delete Static Route
  ```
  root# delete routing-options static
  root# show | compare
  root# commit
  ```

#### 2. OSPF Routing
* Router ID
  ```
  root# set routing-options router-id 1.1.1.1
  ```

* OSPF Advertise Routing Single Area
  ```
  root# set protocols ospf area 1 interface em0
  root# set protocols ospf area 1 interface em1
  root# set protocols ospf area 1 interface lo0
  root# show | compare
  root# commit
  ```

* OSPF Advertise Routing Multi Area
  ```
  root# set routing-options router-id 2.2.2.2
  root# set protocols ospf area 1 interface em0
  root# set protocols ospf area 2 interface em1
  root# set protocols ospf area 0 interface lo0
  root# show | compare
  root# commit
  ```

* Verification
  ```
  root# show configuration routing-options
  root# show configuration protocols ospf
  root# show ospf neighbor
  root# show ospf route
  root# show route protocol ospf
  ```

* Migration Area OSPF (Rename Area)
  ```
  root# edit protocols ospf
  root# rename area 0 to area 100
  root# show | compare
  root# commit
  ```

#### 3. IS-IS Routing
* Overview IS-IS
  - First IGP  Routing Protocol
  - ISO Standard
  - ISO Address as router-id (SysID)
    
* Detail IS-IS Concept
  - AFI		    -> 49
  - Area ID		-> 0001
  - Sys ID		-> 1720.1400.4004
  - Selector	-> 00

* Convert IP to SysID
  - Step 1 : 172.14.4.4
  - Step 2 : 172.014.004.004
  - Step 3 : 1720.1400.4004
  
* Leveling IS-IS
  - Level 1 : Connection Singlearea (Intra area)
    ```
    49.0001.1720.1400.4004.00 (level 1)
    49.0001.1720.1400.4005.00
    49.0001.1720.1400.4006.00
    ```
    
  - Level 2 : Connection Multiarea (Inter area)
    ```
    49.0002.1720.1400.4004.00 (level 2)
    49.0002.1720.1400.4005.00
    49.0002.1720.1400.4006.00
    ```

* IS-IS Loopback
  ```
  root# set interface lo0 unit 0 family inet address 1.0.0.1/32
  root# set interface em0 unit 0 family inet address 1.1.1.1/30
  root# set interface em1 unit 0 family inet address 1.1.1.2/30
  root# set interface em2 unit 0 family inet address s1.1.1.3/30
  ```
  
* IS-IS Interface
  ```
  root# set interface lo0 unit 0 family iso address 49.0001.1720.1400.4004.00
  root# set interface em0 unit 0 family iso
  root# set interface em1 unit 0 family iso
  root# set interface em2 unit 0 family iso
  ```
  
* IS-IS Advertise Routing Level-1
  ```
  root# set protocols isis interface lo0 level 2 disable (level 1 enable)
  root# set protocols isis interface em0 level 2 disable (level 1 enable)
  root# set protocols isis interface em1 level 2 disable (level 1 enable)
  root# set protocols isis interface em2 level 2 disable (level 1 enable)
  ```
  
* IS-IS Advertise Routing Level-2
  ```
  root# set protocols isis interface lo0 level 1 disable (level 2 enable)
  root# set protocols isis interface em0 level 1 disable (level 2 enable)
  root# set protocols isis interface em1 level 1 disable (level 2 enable)
  root# set protocols isis interface em2 level 1 disable (level 2 enable)
  ```
  
* IS-IS Advertise Routing Level-2 and Level-1
  ```
  root# set protocols isis interface lo0 level 2 disable (level 1 enable)
  root# set protocols isis interface em0 level 2 disable (level 1 enable)
  root# set protocols isis interface em1 level 1 disable (level 2 enable)
  root# set protocols isis interface em2 level 1 disable (level 2 enable)
  ```
  
* Verification
  ```
  root# show route protocol isis
  ```

## Routing Policy

#### Default Routing Policy

  A. OSPF
  - Import Policy : Accept All Route from OSPF
  - Export Policy : Export only to OSPF

  B. IS-IS
  - Import Policy : Accept All Route from IS-IS
  - Export Policy : Export only to IS-IS

  C. RIP
  - Import Policy : Accept All Route from RIP
  - Export Policy : Discard all export

  D. BGP
  - Import Policy : Accept All Route from BGP
  - Export Policy : Export to all active BGP neighbor

## Redistribution Policy

#### 1. Redistribution OSPF to IS-IS
* Set Policy and Export
```
root# set policy-options policy-statement OSPF-to-ISIS term 1
root# set policy-options policy-statement OSPF-to-ISIS term 1 from protocol ospf
root# set policy-options policy-statement OSPF-to-ISIS term 1 from protocol direct
root# set policy-options policy-statement OSPF-to-ISIS term 1 them accept
root# set protocol isis export OSPF-to-ISIS
```

* Verification
```
root# show route protocol isis
root# show route protocol ospf
```

#### 2. Redistribution IS-IS to OSPF
* Set Policy and Export
```
root# set policy-options policy-statement ISIS-to-OSPF term 1
root# set policy-options policy-statement ISIS-to-OSPF term 1 from protocol isis
root# set policy-options policy-statement ISIS-to-OSPF term 1 from protocol direct
root# set policy-options policy-statement ISIS-to-OSPF term 1 them accept
root# set protocol isis export ISIS-to-OSPF
```

* Verification
```
root# show route protocol isis
root# show route protocol ospf
```

## Firewall Filter

#### 1. Firewall Concept
* Discard  -> Bloking
* Reject   -> Bloking with Notification
  
#### 2. Reject Ping (ICMP)
* Set Term Firewall Filter ICMP (Reject)
  ```
  root# set firewall filter FILTER-PING term FILTER-TERM-1
  root# set firewall filter FILTER-PING term FILTER-TERM-1 from source-address 10.xx.yy.zz/24	-> Network Destination
  root# set firewall filter FILTER-PING term FILTER-TERM-1 from destination-address 1.1.1.1/32	-> Loopback R1
  root# set firewall filter FILTER-PING term FILTER-TERM-1 from protocol icmp 
  root# set firewall filter FILTER-PING term FILTER-TERM-1 then reject
  root# show | compare
  root# commit
  ```
  
* Set Term Firewall Filter ICMP (Accept)
  ```
  root# set firewall filter FILTER-PING term FILTER-DEFAULT
  root# set firewall filter FILTER-PING term FILTER-DEFAULT then accept
  root# show | compare
  root# commit
  ```
  
* Export Filter in Interface (Lo0, em0, etc)
  ```
  root# set interface lo0 unit 0 family inet filter input FILTER-TERM-1
  root# show | compare
  root# commit
  ```
  
#### 3. Reject SSH (Default Port 22)
* Set Term Firewall Filter SSH (Reject)
  ```
  root# set firewall filter FILTER-SSH term FILTER-TERM-SSH 
  root# set firewall filter FILTER-SSH term FILTER-TERM-SSH from source-address 10.xx.xx.yy/24
  root# set firewall filter FILTER-SSH term FILTER-TERM-SSH from protocol tcp 
  root# set firewall filter FILTER-SSH term FILTER-TERM-SSH from destination-port 22
  root# set firewall filter FILTER-SSH term FILTER-TERM-SSH then reject
  root# show | compare
  root# commit
  ```
* Set Term Firewall Filter SSH (Accept)
  ```
  root# set firewall filter FILTER-SSH term FILTER-DEFAULT
  root# set firewall filter FILTER-SSH term FILTER-DEFAULT then accept
  root# show | compare
  root# commit
  ```

## Support

* [:octocat: Follow me on GitHub](https://github.com/anggrdwjy)
* [🔔 Subscribe me on Youtube](https://www.youtube.com/@anggarda.wijaya)


#### Bugs

Please open an issue on GitHub with as much information as possible if you found a bug.
* Your Junos and Software Update
* All the logs and message outputted
* etc
