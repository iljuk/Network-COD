IP Plan + Config + show isis (all in 1)

##########################################################################################

IP plan OSPF migration -> IS-IS:

10.0.1.0/32 - Loopback1 (Underlay)
10.1.1.0/32 - Loopback2
10.2.1.0/31 - p2p Links (Underlay)
10.3.0.0/16 - Reserv
10.7.0.0/14 - for Clients Services

IS-IS Net for COD1:
49.0001.1000.1000.0xxx.00

IP:

IS-IS Id - Loopback1 IP Underlay:
1000.0000.1001 - 10.0.1.1 leaf1 
1000.0000.1002 - 10.0.1.2 leaf2
1000.0000.1003 - 10.0.1.3 leaf3
1000.0000.1101 - 10.0.1.101 spine1
1000.0000.1102 - 10.0.1.102 spine2

Net Id                    
49.0001.1000.0000.1001.00       leaf1   
49.0001.1000.0000.1002.00       leaf2   
49.0001.1000.0000.1003.00       leaf3   
49.0001.1000.0000.1101.00       spine1
49.0001.1000.0000.1102.00       spine2
                    

p2p Underlay Links:
leaf1 10.2.1.10/31 - spine1 10.2.1.11/31 
leaf1 10.2.1.12/31 - spine2 10.2.1.13/31 

leaf2 10.2.1.20/31 - spine1 10.2.1.21/31 
leaf2 10.2.1.22/31 - spine2 10.2.1.23/31 

leaf3 10.2.1.30/31 - spine1 10.2.1.31/31 
leaf3 10.2.1.32/31 - spine2 10.2.1.33/31 

Loopback2 (reserv):
10.1.1.1 leaf1
10.1.1.2 leaf2
10.1.1.3 leaf3
10.1.1.101 spine1
10.1.1.102 spine2

##########################################################################################

leaf1#show running-config 
! Command: show running-config
! device: leaf1 (vEOS-lab, EOS-4.29.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname leaf1
!
spanning-tree mode mstp
!
interface Ethernet1
   description leaf1-spine1
   no switchport
   ip address 10.2.1.10/31
   bfd static neighbor 10.2.1.11
   bfd interval 500 min-rx 500 multiplier 5
   bfd echo
   bfd authentication mode disabled  
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet2
   description leaf1-spine2
   no switchport
   ip address 10.2.1.12/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet3
   shutdown
!
interface Ethernet4
   shutdown
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.1/32
   ip ospf area 0.0.0.1
   isis enable COD1
!
interface Management1
!
ip routing
!
router isis COD1 instance-id 1
   net 49.0001.1000.0000.1001.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.1
   shutdown
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
leaf1#

leaf2#show running-config
! Command: show running-config
! device: leaf2 (vEOS-lab, EOS-4.29.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname leaf2
!
spanning-tree mode mstp
!
interface Ethernet1
   description leaf2-spine1
   no switchport
   ip address 10.2.1.20/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet2
   description leaf2-spine2
   no switchport
   ip address 10.2.1.22/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet3
   shutdown
!
interface Ethernet4
   shutdown
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.2/32
   ip ospf area 0.0.0.1
   isis enable COD1
!
interface Management1
!
ip routing
!
router isis COD1 instance-id 1
   net 49.0001.1000.0000.1002.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.2
   shutdown
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
leaf2#


leaf3#show running-config
! Command: show running-config
! device: leaf3 (vEOS-lab, EOS-4.29.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname leaf3
!
spanning-tree mode mstp
!
interface Ethernet1
   description leaf3-spine1
   no switchport
   ip address 10.2.1.30/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet2
   description leaf3-spine2
   no switchport
   ip address 10.2.1.32/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet3
   shutdown
!
interface Ethernet4
   shutdown
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.3/32
   ip ospf area 0.0.0.1
   isis enable COD1
!
interface Management1
!
ip routing
!
router isis COD1 instance-id 1
   net 49.0001.1000.0000.1003.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.3
   shutdown
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
leaf3#


spine1#show running-config
! Command: show running-config
! device: spine1 (vEOS-lab, EOS-4.29.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname spine1
!
spanning-tree mode mstp
!
interface Ethernet1
   description spine1-leaf1
   no switchport
   ip address 10.2.1.11/31
   bfd static neighbor 10.2.1.10
   bfd interval 500 min-rx 500 multiplier 5
   bfd echo
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet2
   description spine1-leaf2
   no switchport
   ip address 10.2.1.21/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet3
   description spine1-leaf3
   no switchport
   ip address 10.2.1.31/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet4
   shutdown
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.101/32
   ip ospf area 0.0.0.1
   isis enable COD1
!
interface Management1
!
ip routing
!
router isis COD1 instance-id 1
   net 49.0001.1000.0000.1101.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.101
   shutdown
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
spine1#


spine2#show running-config
! Command: show running-config
! device: spine2 (vEOS-lab, EOS-4.29.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model ribd
!
hostname spine2
!
spanning-tree mode mstp
!
interface Ethernet1
   description spine2-leaf1
   no switchport
   ip address 10.2.1.13/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet2
   description spine2-leaf2
   no switchport
   ip address 10.2.1.23/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet3
   description spine2-leaf3
   no switchport
   ip address 10.2.1.33/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.1
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
!
interface Ethernet4
   shutdown
!
interface Ethernet5
   shutdown
!
interface Ethernet6
   shutdown
!
interface Ethernet7
   shutdown
!
interface Ethernet8
   shutdown
!
interface Loopback1
   ip address 10.0.1.102/32
   ip ospf area 0.0.0.1
   isis enable COD1
!
interface Management1
!
ip routing
!
router isis COD1 instance-id 1
   net 49.0001.1000.0000.1102.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.102
   shutdown
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
spine2#

#######################################################################################################


******************************************************************************************************************

leaf1#show isis summary
 
IS-IS Instance: COD1 VRF: default
  Instance ID: 1
  System ID: 1000.0000.1001, administratively enabled
  Router ID: IPv4: 10.0.1.1
  Hostname: leaf1
  Multi Topology disabled, not attached
  IPv4 Preference: Level 1: 115, Level 2: 115
  IPv6 Preference: Level 1: 115, Level 2: 115
  IS-Type: Level 1 and 2, Number active interfaces: 3
  Routes IPv4 only
  LSP size maximum: Level 1: 1492, Level 2: 1492
                            Max wait(s) Initial wait(ms) Hold interval(ms)
  LSP Generation Interval:     5              50               50
  SPF Interval:                2            1000             1000
  Current SPF hold interval(ms): Level 1: 1000, Level 2: 1000
  Last Level 1 SPF run 45 seconds ago
  Last Level 2 SPF run 3:10 minutes ago
  CSNP generation interval: 10 seconds
  Dynamic Flooding: Disabled
  Authentication mode: Level 1: MD5, Level 2: MD5
  Graceful Restart: Disabled, Graceful Restart Helper: Enabled
  Area addresses: 49.0001
  level 1: number DIS interfaces: 0, LSDB size: 5
    Area Leader: None
    Overload Bit is not set. 
  level 2: number DIS interfaces: 0, LSDB size: 1
    Area Leader: None
    Overload Bit is not set. 
  Redistributed Level 1 routes: 0 limit: Not Configured
  Redistributed Level 2 routes: 0 limit: Not Configured
leaf1#


leaf2#show isis summary
 
IS-IS Instance: COD1 VRF: default
  Instance ID: 1
  System ID: 1000.0000.1002, administratively enabled
  Router ID: IPv4: 10.0.1.2
  Hostname: leaf2
  Multi Topology disabled, not attached
  IPv4 Preference: Level 1: 115, Level 2: 115
  IPv6 Preference: Level 1: 115, Level 2: 115
  IS-Type: Level 1 and 2, Number active interfaces: 3
  Routes IPv4 only
  LSP size maximum: Level 1: 1492, Level 2: 1492
                            Max wait(s) Initial wait(ms) Hold interval(ms)
  LSP Generation Interval:     5              50               50
  SPF Interval:                2            1000             1000
  Current SPF hold interval(ms): Level 1: 1000, Level 2: 1000
  Last Level 1 SPF run 1:29 minutes ago
  Last Level 2 SPF run 36 seconds ago
  CSNP generation interval: 10 seconds
  Dynamic Flooding: Disabled
  Authentication mode: Level 1: MD5, Level 2: MD5
  Graceful Restart: Disabled, Graceful Restart Helper: Enabled
  Area addresses: 49.0001
  level 1: number DIS interfaces: 0, LSDB size: 5
    Area Leader: None
    Overload Bit is not set. 
  level 2: number DIS interfaces: 0, LSDB size: 1
    Area Leader: None
    Overload Bit is not set. 
  Redistributed Level 1 routes: 0 limit: Not Configured
  Redistributed Level 2 routes: 0 limit: Not Configured
leaf2#

leaf3#show isis summary
 
IS-IS Instance: COD1 VRF: default
  Instance ID: 1
  System ID: 1000.0000.1003, administratively enabled
  Router ID: IPv4: 10.0.1.3
  Hostname: leaf3
  Multi Topology disabled, not attached
  IPv4 Preference: Level 1: 115, Level 2: 115
  IPv6 Preference: Level 1: 115, Level 2: 115
  IS-Type: Level 1 and 2, Number active interfaces: 3
  Routes IPv4 only
  LSP size maximum: Level 1: 1492, Level 2: 1492
                            Max wait(s) Initial wait(ms) Hold interval(ms)
  LSP Generation Interval:     5              50               50
  SPF Interval:                2            1000             1000
  Current SPF hold interval(ms): Level 1: 1000, Level 2: 1000
  Last Level 1 SPF run 1:31 minutes ago
  Last Level 2 SPF run 4:42 minutes ago
  CSNP generation interval: 10 seconds
  Dynamic Flooding: Disabled
  Authentication mode: Level 1: MD5, Level 2: MD5
  Graceful Restart: Disabled, Graceful Restart Helper: Enabled
  Area addresses: 49.0001
  level 1: number DIS interfaces: 0, LSDB size: 5
    Area Leader: None
    Overload Bit is not set. 
  level 2: number DIS interfaces: 0, LSDB size: 1
    Area Leader: None
    Overload Bit is not set. 
  Redistributed Level 1 routes: 0 limit: Not Configured
  Redistributed Level 2 routes: 0 limit: Not Configured
leaf3#

spine1#show isis summary
 
IS-IS Instance: COD1 VRF: default
  Instance ID: 1
  System ID: 1000.0000.1101, administratively enabled
  Router ID: IPv4: 10.0.1.101
  Hostname: spine1
  Multi Topology disabled, not attached
  IPv4 Preference: Level 1: 115, Level 2: 115
  IPv6 Preference: Level 1: 115, Level 2: 115
  IS-Type: Level 1 and 2, Number active interfaces: 4
  Routes IPv4 only
  LSP size maximum: Level 1: 1492, Level 2: 1492
                            Max wait(s) Initial wait(ms) Hold interval(ms)
  LSP Generation Interval:     5              50               50
  SPF Interval:                2            1000             1000
  Current SPF hold interval(ms): Level 1: 1000, Level 2: 1000
  Last Level 1 SPF run 1:33 minutes ago
  Last Level 2 SPF run 3:48 minutes ago
  CSNP generation interval: 10 seconds
  Dynamic Flooding: Disabled
  Authentication mode: Level 1: MD5, Level 2: MD5
  Graceful Restart: Disabled, Graceful Restart Helper: Enabled
  Area addresses: 49.0001
  level 1: number DIS interfaces: 0, LSDB size: 5
    Area Leader: None
    Overload Bit is not set. 
  level 2: number DIS interfaces: 0, LSDB size: 1
    Area Leader: None
    Overload Bit is not set. 
  Redistributed Level 1 routes: 0 limit: Not Configured
  Redistributed Level 2 routes: 0 limit: Not Configured
spine1#

spine2#show isis summary
 
IS-IS Instance: COD1 VRF: default
  Instance ID: 1
  System ID: 1000.0000.1102, administratively enabled
  Router ID: IPv4: 10.0.1.102
  Hostname: spine2
  Multi Topology disabled, not attached
  IPv4 Preference: Level 1: 115, Level 2: 115
  IPv6 Preference: Level 1: 115, Level 2: 115
  IS-Type: Level 1 and 2, Number active interfaces: 4
  Routes IPv4 only
  LSP size maximum: Level 1: 1492, Level 2: 1492
                            Max wait(s) Initial wait(ms) Hold interval(ms)
  LSP Generation Interval:     5              50               50
  SPF Interval:                2            1000             1000
  Current SPF hold interval(ms): Level 1: 1000, Level 2: 1000
  Last Level 1 SPF run 1:35 minutes ago
  Last Level 2 SPF run 6:19 minutes ago
  CSNP generation interval: 10 seconds
  Dynamic Flooding: Disabled
  Authentication mode: Level 1: MD5, Level 2: MD5
  Graceful Restart: Disabled, Graceful Restart Helper: Enabled
  Area addresses: 49.0001
  level 1: number DIS interfaces: 0, LSDB size: 5
    Area Leader: None
    Overload Bit is not set. 
  level 2: number DIS interfaces: 0, LSDB size: 1
    Area Leader: None
    Overload Bit is not set. 
  Redistributed Level 1 routes: 0 limit: Not Configured
  Redistributed Level 2 routes: 0 limit: Not Configured
spine2#

******************************************************************************************************************

leaf1#show isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
COD1      default  spine1           L1   Ethernet1          P2P               UP    25          0E                  
COD1      default  spine2           L1   Ethernet2          P2P               UP    26          0D                  
leaf1#

leaf2#show isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
COD1      default  spine1           L1   Ethernet1          P2P               UP    28          0F                  
COD1      default  spine2           L1   Ethernet2          P2P               UP    24          0E                  
leaf2#

leaf3#show isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
COD1      default  spine1           L1   Ethernet1          P2P               UP    26          10                  
COD1      default  spine2           L1   Ethernet2          P2P               UP    27          0F                  
leaf3#

spine1#show isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
COD1      default  leaf1            L1   Ethernet1          P2P               UP    28          0E                  
COD1      default  leaf2            L1   Ethernet2          P2P               UP    23          0E                  
COD1      default  leaf3            L1   Ethernet3          P2P               UP    23          0D                  
spine1#

spine2#show isis neighbors 
 
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id          
COD1      default  leaf1            L1   Ethernet1          P2P               UP    26          0F                  
COD1      default  leaf2            L1   Ethernet2          P2P               UP    23          0F                  
COD1      default  leaf3            L1   Ethernet3          P2P               UP    30          0E                  
spine2#

******************************************************************************************************************

leaf1#show isis database 

IS-IS Instance: COD1 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  61321   784    146 L2 <>
    leaf2.00-00                  10  43406   929    146 L2 <>
    leaf3.00-00                   7  46476   643    146 L2 <>
    spine1.00-00                 29  55842   793    171 L2 <>
    spine2.00-00                  9  38793   644    171 L2 <>
  IS-IS Level 2 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  10  14967   784    188 L2 <>
leaf1#

IS-IS Instance: COD1 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  61321   768    146 L2 <>
    leaf2.00-00                  10  43406   913    146 L2 <>
    leaf3.00-00                   7  46476   627    146 L2 <>
    spine1.00-00                 29  55842   777    171 L2 <>
    spine2.00-00                  9  38793   628    171 L2 <>
  IS-IS Level 2 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf2.00-00                  18  49933   966    188 L2 <>
leaf2#

leaf3#show isis database 

IS-IS Instance: COD1 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  61321   766    146 L2 <>
    leaf2.00-00                  10  43406   910    146 L2 <>
    leaf3.00-00                   7  46476   625    146 L2 <>
    spine1.00-00                 29  55842   775    171 L2 <>
    spine2.00-00                  9  38793   626    171 L2 <>
  IS-IS Level 2 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf3.00-00                  12  13229   719    188 L2 <>
leaf3#

spine1#show isis database 

IS-IS Instance: COD1 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  61321   764    146 L2 <>
    leaf2.00-00                  10  43406   909    146 L2 <>
    leaf3.00-00                   7  46476   623    146 L2 <>
    spine1.00-00                 29  55842   773    171 L2 <>
    spine2.00-00                  9  38793   624    171 L2 <>
  IS-IS Level 2 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    spine1.00-00                 13  17972   773    189 L2 <>
spine1#

spine2#show isis database 

IS-IS Instance: COD1 VRF: default
  IS-IS Level 1 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    leaf1.00-00                  16  61321   762    146 L2 <>
    leaf2.00-00                  10  43406   906    146 L2 <>
    leaf3.00-00                   7  46476   621    146 L2 <>
    spine1.00-00                 29  55842   771    171 L2 <>
    spine2.00-00                  9  38793   622    171 L2 <>
  IS-IS Level 2 Link State Database
    LSPID                   Seq Num  Cksum  Life Length IS Flags
    spine2.00-00                 15  29742   622    189 L2 <>
spine2#

******************************************************************************************************************

leaf1#show ip route 

 C        10.0.1.1/32 is directly connected, Loopback1
 I L1     10.0.1.2/32 [115/30] via 10.2.1.11, Ethernet1
                               via 10.2.1.13, Ethernet2
 I L1     10.0.1.3/32 [115/30] via 10.2.1.11, Ethernet1
                               via 10.2.1.13, Ethernet2
 I L1     10.0.1.101/32 [115/20] via 10.2.1.11, Ethernet1
 I L1     10.0.1.102/32 [115/20] via 10.2.1.13, Ethernet2
 C        10.2.1.10/31 is directly connected, Ethernet1
 C        10.2.1.12/31 is directly connected, Ethernet2
 I L1     10.2.1.20/31 [115/20] via 10.2.1.11, Ethernet1
 I L1     10.2.1.22/31 [115/20] via 10.2.1.13, Ethernet2
 I L1     10.2.1.30/31 [115/20] via 10.2.1.11, Ethernet1
 I L1     10.2.1.32/31 [115/20] via 10.2.1.13, Ethernet2

leaf1#

leaf2#show ip route 

 I L1     10.0.1.1/32 [115/30] via 10.2.1.21, Ethernet1
                               via 10.2.1.23, Ethernet2
 C        10.0.1.2/32 is directly connected, Loopback1
 I L1     10.0.1.3/32 [115/30] via 10.2.1.21, Ethernet1
                               via 10.2.1.23, Ethernet2
 I L1     10.0.1.101/32 [115/20] via 10.2.1.21, Ethernet1
 I L1     10.0.1.102/32 [115/20] via 10.2.1.23, Ethernet2
 I L1     10.2.1.10/31 [115/20] via 10.2.1.21, Ethernet1
 I L1     10.2.1.12/31 [115/20] via 10.2.1.23, Ethernet2
 C        10.2.1.20/31 is directly connected, Ethernet1
 C        10.2.1.22/31 is directly connected, Ethernet2
 I L1     10.2.1.30/31 [115/20] via 10.2.1.21, Ethernet1
 I L1     10.2.1.32/31 [115/20] via 10.2.1.23, Ethernet2

leaf2#

leaf3#show ip route 

 I L1     10.0.1.1/32 [115/30] via 10.2.1.31, Ethernet1
                               via 10.2.1.33, Ethernet2
 I L1     10.0.1.2/32 [115/30] via 10.2.1.31, Ethernet1
                               via 10.2.1.33, Ethernet2
 C        10.0.1.3/32 is directly connected, Loopback1
 I L1     10.0.1.101/32 [115/20] via 10.2.1.31, Ethernet1
 I L1     10.0.1.102/32 [115/20] via 10.2.1.33, Ethernet2
 I L1     10.2.1.10/31 [115/20] via 10.2.1.31, Ethernet1
 I L1     10.2.1.12/31 [115/20] via 10.2.1.33, Ethernet2
 I L1     10.2.1.20/31 [115/20] via 10.2.1.31, Ethernet1
 I L1     10.2.1.22/31 [115/20] via 10.2.1.33, Ethernet2
 C        10.2.1.30/31 is directly connected, Ethernet1
 C        10.2.1.32/31 is directly connected, Ethernet2

leaf3#

spine1#show ip route 

 I L1     10.0.1.1/32 [115/20] via 10.2.1.10, Ethernet1
 I L1     10.0.1.2/32 [115/20] via 10.2.1.20, Ethernet2
 I L1     10.0.1.3/32 [115/20] via 10.2.1.30, Ethernet3
 C        10.0.1.101/32 is directly connected, Loopback1
 I L1     10.0.1.102/32 [115/30] via 10.2.1.10, Ethernet1
                                 via 10.2.1.20, Ethernet2
                                 via 10.2.1.30, Ethernet3
 C        10.2.1.10/31 is directly connected, Ethernet1
 I L1     10.2.1.12/31 [115/20] via 10.2.1.10, Ethernet1
 C        10.2.1.20/31 is directly connected, Ethernet2
 I L1     10.2.1.22/31 [115/20] via 10.2.1.20, Ethernet2
 C        10.2.1.30/31 is directly connected, Ethernet3
 I L1     10.2.1.32/31 [115/20] via 10.2.1.30, Ethernet3

spine1#

spine2#show ip route 

 I L1     10.0.1.1/32 [115/20] via 10.2.1.12, Ethernet1
 I L1     10.0.1.2/32 [115/20] via 10.2.1.22, Ethernet2
 I L1     10.0.1.3/32 [115/20] via 10.2.1.32, Ethernet3
 I L1     10.0.1.101/32 [115/30] via 10.2.1.12, Ethernet1
                                 via 10.2.1.22, Ethernet2
                                 via 10.2.1.32, Ethernet3
 C        10.0.1.102/32 is directly connected, Loopback1
 I L1     10.2.1.10/31 [115/20] via 10.2.1.12, Ethernet1
 C        10.2.1.12/31 is directly connected, Ethernet1
 I L1     10.2.1.20/31 [115/20] via 10.2.1.22, Ethernet2
 C        10.2.1.22/31 is directly connected, Ethernet2
 I L1     10.2.1.30/31 [115/20] via 10.2.1.32, Ethernet3
 C        10.2.1.32/31 is directly connected, Ethernet3

spine2#

******************************************************************************************************************

leaf1#ping 10.0.1.1 source 10.0.1.1
PING 10.0.1.1 (10.0.1.1) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.1: icmp_seq=1 ttl=64 time=0.730 ms
80 bytes from 10.0.1.1: icmp_seq=2 ttl=64 time=0.157 ms
80 bytes from 10.0.1.1: icmp_seq=3 ttl=64 time=0.156 ms
80 bytes from 10.0.1.1: icmp_seq=4 ttl=64 time=0.155 ms
80 bytes from 10.0.1.1: icmp_seq=5 ttl=64 time=0.199 ms

--- 10.0.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 17ms
rtt min/avg/max/mdev = 0.155/0.279/0.730/0.226 ms, ipg/ewma 4.354/0.498 ms
leaf1#ping 10.0.1.2 source 10.0.1.1 
PING 10.0.1.2 (10.0.1.2) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.2: icmp_seq=1 ttl=63 time=59.6 ms
80 bytes from 10.0.1.2: icmp_seq=2 ttl=63 time=47.8 ms
80 bytes from 10.0.1.2: icmp_seq=3 ttl=63 time=44.4 ms
80 bytes from 10.0.1.2: icmp_seq=4 ttl=63 time=39.4 ms
80 bytes from 10.0.1.2: icmp_seq=5 ttl=63 time=37.2 ms

--- 10.0.1.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 56ms
rtt min/avg/max/mdev = 37.287/45.745/59.692/7.898 ms, pipe 5, ipg/ewma 14.177/52.226 ms
leaf1#ping 10.0.1.3 source 10.0.1.1 
PING 10.0.1.3 (10.0.1.3) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.3: icmp_seq=1 ttl=63 time=41.0 ms
80 bytes from 10.0.1.3: icmp_seq=2 ttl=63 time=40.6 ms
80 bytes from 10.0.1.3: icmp_seq=3 ttl=63 time=43.5 ms
80 bytes from 10.0.1.3: icmp_seq=4 ttl=63 time=19.5 ms
80 bytes from 10.0.1.3: icmp_seq=5 ttl=63 time=16.2 ms

--- 10.0.1.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 107ms
rtt min/avg/max/mdev = 16.267/32.217/43.553/11.764 ms, pipe 3, ipg/ewma 26.830/35.801 ms
leaf1#ping 10.0.1.101 source 10.0.1.1 
PING 10.0.1.101 (10.0.1.101) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.101: icmp_seq=1 ttl=64 time=16.8 ms
80 bytes from 10.0.1.101: icmp_seq=2 ttl=64 time=13.9 ms
80 bytes from 10.0.1.101: icmp_seq=3 ttl=64 time=9.71 ms
80 bytes from 10.0.1.101: icmp_seq=4 ttl=64 time=9.37 ms
80 bytes from 10.0.1.101: icmp_seq=5 ttl=64 time=10.8 ms

--- 10.0.1.101 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 65ms
rtt min/avg/max/mdev = 9.379/12.133/16.855/2.855 ms, pipe 2, ipg/ewma 16.499/14.351 ms
leaf1#ping 10.0.1.102 source 10.0.1.1  
PING 10.0.1.102 (10.0.1.102) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.102: icmp_seq=1 ttl=64 time=9.91 ms
80 bytes from 10.0.1.102: icmp_seq=2 ttl=64 time=8.12 ms
80 bytes from 10.0.1.102: icmp_seq=3 ttl=64 time=7.74 ms
80 bytes from 10.0.1.102: icmp_seq=4 ttl=64 time=8.37 ms
80 bytes from 10.0.1.102: icmp_seq=5 ttl=64 time=11.2 ms

--- 10.0.1.102 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 45ms
rtt min/avg/max/mdev = 7.745/9.087/11.280/1.323 ms, ipg/ewma 11.408/9.557 ms
leaf1#

******************************************************************************************************************

spine2#ping 10.0.1.1 source 10.0.1.1
bind: Cannot assign requested address
spine2#ping 10.0.1.1 source 10.0.1.102
PING 10.0.1.1 (10.0.1.1) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.1: icmp_seq=1 ttl=64 time=18.0 ms
80 bytes from 10.0.1.1: icmp_seq=2 ttl=64 time=14.1 ms
80 bytes from 10.0.1.1: icmp_seq=3 ttl=64 time=12.8 ms
80 bytes from 10.0.1.1: icmp_seq=4 ttl=64 time=20.7 ms
80 bytes from 10.0.1.1: icmp_seq=5 ttl=64 time=32.3 ms

--- 10.0.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 68ms
rtt min/avg/max/mdev = 12.868/19.653/32.354/6.941 ms, pipe 2, ipg/ewma 17.147/19.339 ms
spine2#ping 10.0.1.2 source 10.0.1.102 
PING 10.0.1.2 (10.0.1.2) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.2: icmp_seq=1 ttl=64 time=11.9 ms
80 bytes from 10.0.1.2: icmp_seq=2 ttl=64 time=23.4 ms
80 bytes from 10.0.1.2: icmp_seq=3 ttl=64 time=18.0 ms

--- 10.0.1.2 ping statistics ---
5 packets transmitted, 3 received, 40% packet loss, time 63ms
rtt min/avg/max/mdev = 11.922/17.813/23.471/4.719 ms, pipe 2, ipg/ewma 15.820/13.950 ms
spine2#ping 10.0.1.3 source 10.0.1.102 
PING 10.0.1.3 (10.0.1.3) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.3: icmp_seq=1 ttl=64 time=9.52 ms
80 bytes from 10.0.1.3: icmp_seq=2 ttl=64 time=9.66 ms
80 bytes from 10.0.1.3: icmp_seq=3 ttl=64 time=11.2 ms
80 bytes from 10.0.1.3: icmp_seq=4 ttl=64 time=11.2 ms
80 bytes from 10.0.1.3: icmp_seq=5 ttl=64 time=9.73 ms

--- 10.0.1.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 51ms
rtt min/avg/max/mdev = 9.521/10.275/11.239/0.787 ms, pipe 2, ipg/ewma 12.941/9.909 ms
spine2#ping 10.0.1.101 source 10.0.1.102 
PING 10.0.1.101 (10.0.1.101) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.101: icmp_seq=1 ttl=63 time=51.7 ms
80 bytes from 10.0.1.101: icmp_seq=2 ttl=63 time=64.3 ms
80 bytes from 10.0.1.101: icmp_seq=3 ttl=63 time=66.4 ms
80 bytes from 10.0.1.101: icmp_seq=4 ttl=63 time=67.9 ms
80 bytes from 10.0.1.101: icmp_seq=5 ttl=63 time=49.4 ms

--- 10.0.1.101 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 98ms
rtt min/avg/max/mdev = 49.415/59.992/67.958/7.822 ms, pipe 4, ipg/ewma 24.688/55.672 ms
spine2#ping 10.0.1.102 source 10.0.1.102 
PING 10.0.1.102 (10.0.1.102) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.102: icmp_seq=1 ttl=64 time=1.62 ms
80 bytes from 10.0.1.102: icmp_seq=2 ttl=64 time=0.308 ms
80 bytes from 10.0.1.102: icmp_seq=3 ttl=64 time=0.918 ms
80 bytes from 10.0.1.102: icmp_seq=4 ttl=64 time=0.263 ms
80 bytes from 10.0.1.102: icmp_seq=5 ttl=64 time=0.355 ms

--- 10.0.1.102 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 32ms
rtt min/avg/max/mdev = 0.263/0.693/1.624/0.523 ms, ipg/ewma 8.094/1.139 ms
spine2#



