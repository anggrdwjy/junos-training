## Overview Junos

Junos OS (also known as Juniper Junos, Junos and JUNOS) is a FreeBSD-based network operating system used in Juniper Networks routing, switching and security devices. Junos OS was first made available on 7 July 1998. As of 2008, feature updates have been released quarterly. As of March 2024, the latest version is Junos OS 23.2R1, released on June 23, 2023.

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
* MPLS Configuration  _[Bonus]
* I-BGP Route Reflector  _[Bonus]
* MPLS L2VPN Configuration  _[Bonus]
* VPLS Configuration  _[Bonus]
* MPLS L3VPN Configuration _[Bonus]
* LACP Configuration  _[Bonus]

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
root> run
root> show
```

#### 3. Confiugration Mode (Root and User Privilege)
```
root> configure
root# edit
root# set
```
## Basic Configuration

#### 1. Navigation Configuration (Hirarcy)
```
root# up    -> back hirarcy
root# up 3  -> back 3 hirarcy
root# top   -> back to top hirarcy
```

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
root# commit at "2026-03-26 24:00:00"    -> Schedule commit time
root# commit confirmed                   -> 10minutes rollback configuration (default)
root# commit confirmed 1                 -> 1 minute
root# commit confirmed 20                -> 20 minute
root# commit comment "remove xxyyzz"
root# show system commit
```

* Rollback commit comment max 49 list
```
root# rollback 1 -> back configuration commit 1
root# rollback 0 -> discard candidate configuration
```

#### 7. Date and Time
* Set Date
```
root> run set date 202603060100.00        -> Operational Mode
```

* Set Timezone
```
root# set system timezone Asia/Jakarta    -> Configuration Mode
root# show | compare
root# commit
```

* Verification
```
root# show system uptime
```

#### 8. NTP (Network Time Protocol)
* Basic 
```
root# set system ntp server id.pool.ntp.org     -> Domain
root# set system ntp server 162.159.200.123     -> IP 
root# show | compare
root# commit
```

* Advanced
```
root# set system ntp boot-server "ntp server main"
root# set system ntp server "ntp server main" prefer
root# set system ntp server "ntp server backup"
root# set system ntp source-address "loopback"
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

* Maximum Transmission Unit (MTU)
```
root# set interfaces em0 mtu "range 1998 - 9200"
root# show | compare
root# commit
```

#### 17. SNMP (Simple Network Management Protocol)
* SNMP 
```
root# set snmp description snmpfilter
root# set snmp location "hostname"
root# set snmp contact "mail / contact center"
root# set snmp community "snmp community" authorization read-only
root# set snmp community "snmp community" clients "ip target 1"
root# set snmp community "snmp community" clients "ip target 2"
root# commit
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

* Static Route Preference
```
root# set routing-options static route "destination_network" next-hop "gateway" preference "value"
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
root# show ospf interface
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

* Advanced OSPF
```
root# set protocols ospf area "id area" interface "loopback" interface-type p2p
root# set protocols ospf area "id area" interface "loopback" metric "1 - 65000"    -> MTU
```
```
root# set protocols ospf area "id area" interface "port" interface-type p2p
root# set protocols ospf area "id area" interface "port" metric "1 - 65000"    -> MTU
root# set protocols ospf area "id area" interface "port" hello-interval 5
root# set protocols ospf area "id area" interface "port" dead-interval 15
root# set protocols ospf area "id area" interface "port" authentication md5 1 key "password"
```

* OSPF Traffic-Engineering and Reference Bandwidth
```
root# set protocols ospf traffic-engineering
root# set protocols ospf reference-bandwidth "1G/10G/100G"
```

* OSPF BFD (Bidirectional Forwarding Detection)
```
root# set protocols ospf area "id area" interface "port" bfd-liveness-detection minimum interval 300
root# set protocols ospf area "id area" interface "port" bfd-liveness-detection multiplier 3
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
root# show isis interface
root# show isis summary
root# show isis adjacency detail
root# show isis database
```

* IS-IS Advanced
```
root# set protocol isis level 1 wide-metrics-only    -> Intra Area
root# set protocol isis level 2 wide-metrics-only    -> Inter Area
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

#### Example OSPF Policy
```
root# set policy-options policy-statement "ospf-term" term 1 from route-filter "ip address /xx" exact
root# set policy-options policy-statement "ospf-term" term 1 then accept
root# set policy-options policy-statement "ospf-term" term 2 then reject
root# set protocols ospf export "ospf-term"
```

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
* Discard      -> Bloking
* Reject       -> Bloking with Notification
  
#### 2. Reject Ping (ICMP)
* Set Term Firewall Filter ICMP (Reject)
```
root# set firewall filter FILTER-PING term FILTER-TERM-1
root# set firewall filter FILTER-PING term FILTER-TERM-1 from source-address 10.xx.yy.zz/24     -> Network Destination
root# set firewall filter FILTER-PING term FILTER-TERM-1 from destination-address 1.1.1.1/32    -> Loopback R1
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

## MPLS (Multi Protocol Labeling Switching)
#### 1. MPLS Interface
* MPLS Configuration
```
root# set protocols mpls no-propagate-ttl
root# set protocols mpls interface lo0
root# set protocols mpls interface em0
root# set protocols mpls interface em1
```

* Verification
```
root> show route forwarding-table family mpls
root> show mpls interface brief
root> show mpls lsp brief
```

#### 2. MPLS LDP (Label Distribution Protocol)
* LDP Configuration
```
root# set protocols ldp interface lo0
root# set protocols ldp interface em0
root# set protocols ldp interface em1
root# set protocols ldp session "ip neighbor 1" authentication-key "password"
root# set protocols ldp session "ip neighbor 2" authentication-key "password"
```

* Verification
```
root> show ldp interface
root> show ldp neighbor
root> show ldp session
```

#### 3. LLDP (Link Layer Discovery Protocol)
* LLDP
```
root# set protocols lldp interface em0
root# set protocols lldp interface em1
```

* Verification
```
root> show lldp neighbor
root> show lldp detail
```

#### 4. RSVP (Resource Reservation Protocol)
* RSVP
```
root# set protocols rsvp interface lo0
root# set protocols rsvp interface em0
root# set protocols rsvp interface em1
```

* Verification
```
root> show rsvp interface
```

## I-BGP Route Reflector

#### 1. BGP Route Reflector (Master)
* Router RR
```
root# set routing-options graceful-restart
root# set routing-options router-id [IP_LOOPBACK]
root# set routing-options autonomous-system [AS-NUMBER]
root# set protocols bgp log-updown
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] type internal
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] local-address [IP_LOOPBACK]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] family inet-vpn unicast  <- VPNV4
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] authentication-key [PASSWORD]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] cluster [CLUSTER_ID]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] peer-as [AS_NUMBER_PEERING]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_A] description [DESCRIPTION]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_B] description [DESCRIPTION]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_C] description [DESCRIPTION]
root# set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_D] description [DESCRIPTION]
root# show | compare
root# commit
```

* Verification
```
root> show bgp summary 
root> show bgp neighbor
```

#### 2. BGP Router Client (Client)
* Router Client
```
root# set routing-options graceful-restart
root# set routing-options router-id [IP_LOOPBACK]
root# set routing-options autonomous-system [AS-NUMBER]
root# set protocols bgp log-updown
root# set protocols bgp group [BGP_GROUP] type internal
root# set protocols bgp group [BGP_GROUP] local-address [IP_LOOPBACK]
root# set protocols bgp group [BGP_GROUP] family inet-vpn unicast  <- VPNV4
root# set protocols bgp group [BGP_GROUP] authentication-key [PASSWORD]
root# set protocols bgp group [BGP_GROUP] cluster [CLUSTER_ID]
root# set protocols bgp group [BGP_GROUP] peer-as [AS_NUMBER_PEERING]
root# set protocols bgp group [BGP_GROUP] neighbor [IP_ROUTE_REFLECTOR] description [DESCRIPTION]
root# show | compare
root# commit
```

* Verification
```
root> show bgp summary 
root> show bgp neighbor
```

## MPLS L2VPN Configuration

#### 1. Far End
* L2VPN
```
root# set protocols l2circuit neighbor "near-end loopback" interface ge-0/0/1.10 virtual-circuit-id 10
root# set protocols l2circuit neighbor "near-end loopback" interface ge-0/0/1.10 description L2VPN
root# set protocols l2circuit neighbor "near-end loopback" interface ge-0/0/1.10 no-control-word
root# set protocols l2circuit neighbor "near-end loopback" interface ge-0/0/1.10 ignore-encapsulation-mismatch
```

* Service Instance
```
root# set interfaces ge-0/0/1 flexible-vlan-tagging
root# set interfaces ge-0/0/1 mtu 1900
root# set interfaces ge-0/0/1 encapsulation flexible-ethernet-services
root# set interfaces ge-0/0/1 unit 10 description L2VPN
root# set interfaces ge-0/0/1 unit 10 encapsulation vlan-ccc
root# set interfaces ge-0/0/1 unit 10 vlan-id 10
```

* Verification
```
root# run show l2circuit connections
```

#### 2. Near End
* L2VPN
```
root# set protocols l2circuit neighbor "far-end loopback" interface ge-0/0/1.10 virtual-circuit-id 10
root# set protocols l2circuit neighbor "far-end loopback" interface ge-0/0/1.10 description L2VPN
root# set protocols l2circuit neighbor "far-end loopback" interface ge-0/0/1.10 no-control-word
root# set protocols l2circuit neighbor "far-end loopback" interface ge-0/0/1.10 ignore-encapsulation-mismatch
```

* Service Instance
```
root# set interfaces ge-0/0/1 flexible-vlan-tagging
root# set interfaces ge-0/0/1 mtu 1900
root# set interfaces ge-0/0/1 encapsulation flexible-ethernet-services
root# set interfaces ge-0/0/1 unit 10 description L2VPN
root# set interfaces ge-0/0/1 unit 10 encapsulation vlan-ccc
root# set interfaces ge-0/0/1 unit 10 vlan-id 10
```

* Verification
```
root# run show l2circuit connections
```

## VPLS Configuration

#### Far End
* VPLS
```
root# set routing-instances "to Near-end" instance-type vpls 
root# set routing-instances "to Near-end" interface ge-0/0/4.0 
root# set routing-instances "to Near-end" protocols vpls encapsulation-type ethernet 
root# set routing-instances "to Near-end" protocols vpls no-control-word 
root# set routing-instances "to Near-end" protocols vpls no-tunnel-services 
root# set routing-instances "to Near-end" protocols vpls vpls-id 234 
root# set routing-instances "to Near-end" protocols vpls mtu 1500 
root# set routing-instances "to Near-end" protocols vpls ignore-mtu-mismatch 
root# set routing-instances "to Near-end" protocols vpls ignore-encapsulation-mismatch 
root# set routing-instances "to Near-end" protocols vpls neighbor "to Near-end Loopback" switchover-delay 5000 
root# set routing-instances "to Near-end" protocols vpls neighbor "to Near-end Loopback" revert-time 60 
root# set routing-instances "to Near-end" protocols vpls neighbor "to Near-end Loopback" backup-neighbor "to Backup-HO Loopback" standby 
```

* Service Instance
```
root# set interfaces ge-0/0/4 unit 0 description "to Near-end"  
root# set interfaces ge-0/0/4 mtu 1500 
root# set interfaces ge-0/0/4 encapsulation ethernet-vpls 
```

* Verification
```
root> show vpls connections
root> show vpls mac-table brief
root> show route forwarding-table family vpls
```

#### Near End
* VPLS
```
root# set routing-instances "to Far-end" instance-type vpls 
root# set routing-instances "to Far-end" interface ge-0/0/4.0 
root# set routing-instances "to Far-end" protocols vpls encapsulation-type ethernet 
root# set routing-instances "to Far-end" protocols vpls no-control-word 
root# set routing-instances "to Far-end" protocols vpls no-tunnel-services 
root# set routing-instances "to Far-end" protocols vpls vpls-id 234 
root# set routing-instances "to Far-end" protocols vpls mtu 1500 
root# set routing-instances "to Far-end" protocols vpls ignore-mtu-mismatch 
root# set routing-instances "to Far-end" protocols vpls ignore-encapsulation-mismatch 
root# set routing-instances "to Far-end" protocols vpls neighbor "to Far-end Loopback" switchover-delay 5000 
root# set routing-instances "to Far-end" protocols vpls neighbor "to Far-end Loopback" revert-time 60 
root# set routing-instances "to Far-end" protocols vpls neighbor "to Far-end Loopback" backup-neighbor "to Backup-HO Loopback" standby 
```

* Service Instance
```
root# set interfaces ge-0/0/4 unit 0 description "to Far-end" 
root# set interfaces ge-0/0/4 mtu 1500 
root# set interfaces ge-0/0/4 encapsulation ethernet-vpls 
```

* Verification
```
root> show vpls connections
root> show vpls mac-table brief
root> show route forwarding-table family vpls
```

## MPLS L3VPN Configuration

## LACP Configuration

## Support

* [:octocat: Follow me on GitHub](https://github.com/anggrdwjy)
* [🔔 Subscribe me on Youtube](https://www.youtube.com/@anggarda.wijaya)


#### Bugs

Please open an issue on GitHub with as much information as possible if you found a bug.
* Your Junos and Software Update
* All the logs and message outputted
* etc
