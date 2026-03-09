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

* Rollback commit comment maks 49 list
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

#### 9. SSH (Default Port 22), FTP, TELNET
```
set system services ssh
set system services ftp
set system services telnet
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

#### 11. Rename and Replace Parameter
* Rename Example
  ```
  edit system
  rename user "user" to "user10"
  show | compare
  commit
  ```

* Replace Example
  ```
  edit system
  replace pattern user "user" with "superuser"
  show | compare
  commit
  ```

#### 12. Pipeline
```
| match [parameter]
| no-more
| display set
```

#### 13. Backup, Restore, Load Merge Configuration
* Backup Configuration
  ```
  save "backup_config"
  ```

* Verification Backup Configuration
  ```
  run file list
  cat "backup_config"
  ```

* Restore Configuration
  ```
  load override "old_config"
  show | compare
  commit
  ```

* Load Merge Configuration
  ```
  load merge "old_config"
  show | compare
  commit
  ```

#### 14. Reset Configuration
* Reset Factory Default
  ```
  load factory-default
  show | compare
  commit
  ```

#### 15. Configure
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

#### 16. Interface Configuration
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

* OSPF Advertise Routing Single Area
  ```
  set protocols ospf area 1 interface em0
  set protocols ospf area 1 interface em1
  set protocols ospf area 1 interface lo0
  show | compare
  commit
  ```

* OSPF Advertise Routing Multi Area
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
  set interface lo0 unit 0 family inet address 1.0.0.1/32
  set interface em0 unit 0 family inet address 1.1.1.1/30
  set interface em1 unit 0 family inet address 1.1.1.2/30
  set interface em2 unit 0 family inet address s1.1.1.3/30
  ```
  
* IS-IS Interface
  ```
  set interface lo0 unit 0 family iso address 49.0001.1720.1400.4004.00
  set interface em0 unit 0 family iso
  set interface em1 unit 0 family iso
  set interface em2 unit 0 family iso
  ```
  
* IS-IS Advertise Routing Level-1
  ```
  set protocols isis interface lo0 level 2 disable (level 1 enable)
  set protocols isis interface em0 level 2 disable (level 1 enable)
  set protocols isis interface em1 level 2 disable (level 1 enable)
  set protocols isis interface em2 level 2 disable (level 1 enable)
  ```
  
* IS-IS Advertise Routing Level-2
  ```
  set protocols isis interface lo0 level 1 disable (level 2 enable)
  set protocols isis interface em0 level 1 disable (level 2 enable)
  set protocols isis interface em1 level 1 disable (level 2 enable)
  set protocols isis interface em2 level 1 disable (level 2 enable)
  ```
  
* IS-IS Advertise Routing Level-2 and Level-1
  ```
  set protocols isis interface lo0 level 2 disable (level 1 enable)
  set protocols isis interface em0 level 2 disable (level 1 enable)
  set protocols isis interface em1 level 1 disable (level 2 enable)
  set protocols isis interface em2 level 1 disable (level 2 enable)
  ```
  
* Verification
  ```
  show route protocol isis
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
set policy-options policy-statement OSPF-to-ISIS term 1
set policy-options policy-statement OSPF-to-ISIS term 1 from protocol ospf
set policy-options policy-statement OSPF-to-ISIS term 1 from protocol direct
set policy-options policy-statement OSPF-to-ISIS term 1 them accept
set protocol isis export OSPF-to-ISIS
```

* Verification
```
show route protocol isis
show route protocol ospf
```

#### 2. Redistribution IS-IS to OSPF
* Set Policy and Export
```
set policy-options policy-statement ISIS-to-OSPF term 1
set policy-options policy-statement ISIS-to-OSPF term 1 from protocol isis
set policy-options policy-statement ISIS-to-OSPF term 1 from protocol direct
set policy-options policy-statement ISIS-to-OSPF term 1 them accept
set protocol isis export ISIS-to-OSPF
```

* Verification
```
show route protocol isis
show route protocol ospf
```

## Firewall Filter

#### 1. Firewall Concept
* Discard  -> Bloking
* Reject   -> Bloking with Notification
  
#### 2. Reject Ping (ICMP)
* Set Term Firewall Filter ICMP (Reject)
  ```
  set firewall filter FILTER-PING term FILTER-TERM-1
  set firewall filter FILTER-PING term FILTER-TERM-1 from source-address 10.xx.yy.zz/24	-> Network Destination
  set firewall filter FILTER-PING term FILTER-TERM-1 from destination-address 1.1.1.1/32	-> Loopback R1
  set firewall filter FILTER-PING term FILTER-TERM-1 from protocol icmp 
  set firewall filter FILTER-PING term FILTER-TERM-1 then reject
  show | compare
  commit
  ```
  
* Set Term Firewall Filter ICMP (Accept)
  ```
  set firewall filter FILTER-PING term FILTER-DEFAULT
  set firewall filter FILTER-PING term FILTER-DEFAULT then accept
  show | compare
  commit
  ```
  
* Export Filter in Interface (Lo0, em0, etc)
  ```
  set interface lo0 unit 0 family inet filter input FILTER-TERM-1
  show | compare
  commit
  ```
  
#### 3. Reject SSH (Default Port 22)
* Set Term Firewall Filter SSH (Reject)
  ```
  set firewall filter FILTER-SSH term FILTER-TERM-SSH 
  set firewall filter FILTER-SSH term FILTER-TERM-SSH from source-address 10.xx.xx.yy/24
  set firewall filter FILTER-SSH term FILTER-TERM-SSH from protocol tcp 
  set firewall filter FILTER-SSH term FILTER-TERM-SSH from destination-port 22
  set firewall filter FILTER-SSH term FILTER-TERM-SSH then reject
  show | compare
  commit
  ```
* Set Term Firewall Filter SSH (Accept)
  ```
  set firewall filter FILTER-SSH term FILTER-DEFAULT
  set firewall filter FILTER-SSH term FILTER-DEFAULT then accept
  show | compare
  commit
  ```

## Support

* [:octocat: Follow me on GitHub](https://github.com/anggrdwjy)
* [🔔 Subscribe me on Youtube](https://www.youtube.com/@anggarda.wijaya)


#### Bugs

Please open an issue on GitHub with as much information as possible if you found a bug.
* Your Junos and Software Update
* All the logs and message outputted
* etc
