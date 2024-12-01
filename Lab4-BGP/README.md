The network operates on iBGP, but is ready for OSPF or IS-IS protocols.  
All switches are RR for maximum network reliability.  
Files:  
shema-seti.png - Схема сети.  
ip-plan_BGP_(+OSPF+ISIS).txt - общий IP план для BGP и протколов OSPF и IS-IS.  
config_all.txt - конфигурации всех коммутаторов.  
show_bgp_bfd_route.txt - просмотр состояний BGP, BFD, ip маршрутов.  
shema-seti_in_emergency_mode.png - Схема сети при отказе 2 линков.  
show_in_emergency_mode.txt - Наличие ip связности при отказе линков через RR Leaf2.  

<img width="536" alt="shema-seti" src="https://github.com/user-attachments/assets/232511a1-779a-4d54-becb-21604e5f5157">


```
IP plan Underlay for all: BGP (OSPF & IS-IS ready)

10.0.1.0/32 - Loopback1 (Underlay)
10.1.1.0/32 - Loopback2
10.2.1.0/31 - p2p Links (Underlay)
10.3.0.0/16 - Reserv
10.7.0.0/14 - for Clients Services

BGP AS = 65001

OSPF Area = 1

IS-IS Net for COD1 = 49.0001.1000.1000.0xxx.00

IP Distribution:
Loopback1 IP Underlay (IS-IS Id):
10.0.1.1 leaf1      (1000.0000.1001)
10.0.1.2 leaf2      (1000.0000.1002)
10.0.1.3 leaf3      (1000.0000.1003)
10.0.1.101 spine1   (1000.0000.1101)
10.0.1.102 spine2   (1000.0000.1102)

Net Id fo IS-IS:                   
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

```


```
################################################################################

Configuration all switches.
The network operates on iBGP, but is ready for OSPF or IS-IS protocols.
All switches are RR for maximum network reliability.

################################################################################

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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 l0w89E/+c20=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
router bgp 65001
   router-id 10.0.1.1
   timers bgp 10 30
   bgp route-reflector preserve-attributes
   neighbor 10.2.1.11 remote-as 65001
   neighbor 10.2.1.11 bfd
   neighbor 10.2.1.11 route-reflector-client
   neighbor 10.2.1.11 password 7 pbiybVavL8E=
   neighbor 10.2.1.13 remote-as 65001
   neighbor 10.2.1.13 bfd
   neighbor 10.2.1.13 route-reflector-client
   neighbor 10.2.1.13 password 7 fG32y0nO3Zs=
   redistribute connected
!
router isis COD1 instance-id 1
   shutdown
   net 49.0001.1000.0000.1001.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.1
   shutdown
   bfd default
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
container-manager
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 l0w89E/+c20=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
router bgp 65001
   router-id 10.0.1.2
   timers bgp 10 30
   bgp route-reflector preserve-attributes
   neighbor 10.2.1.21 remote-as 65001
   neighbor 10.2.1.21 bfd
   neighbor 10.2.1.21 route-reflector-client
   neighbor 10.2.1.21 password 7 8Vjk+esrlzU=
   neighbor 10.2.1.23 remote-as 65001
   neighbor 10.2.1.23 bfd
   neighbor 10.2.1.23 route-reflector-client
   neighbor 10.2.1.23 password 7 hNgRYQupNIA=
   redistribute connected
!
router isis COD1 instance-id 1
   shutdown
   net 49.0001.1000.0000.1002.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.2
   shutdown
   bfd default
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 l0w89E/+c20=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
router bgp 65001
   router-id 10.0.1.3
   timers bgp 10 30
   bgp route-reflector preserve-attributes
   neighbor 10.2.1.31 remote-as 65001
   neighbor 10.2.1.31 bfd
   neighbor 10.2.1.31 route-reflector-client
   neighbor 10.2.1.31 password 7 8Vjk+esrlzU=
   neighbor 10.2.1.33 remote-as 65001
   neighbor 10.2.1.33 bfd
   neighbor 10.2.1.33 route-reflector-client
   neighbor 10.2.1.33 password 7 hNgRYQupNIA=
   redistribute connected
!
router isis COD1 instance-id 1
   shutdown
   net 49.0001.1000.0000.1003.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.3
   shutdown
   bfd default
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 l0w89E/+c20=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
router bgp 65001
   router-id 10.0.1.101
   timers bgp 10 30
   bgp route-reflector preserve-attributes
   neighbor 10.2.1.10 remote-as 65001
   neighbor 10.2.1.10 bfd
   neighbor 10.2.1.10 route-reflector-client
   neighbor 10.2.1.10 password 7 pbiybVavL8E=
   neighbor 10.2.1.20 remote-as 65001
   neighbor 10.2.1.20 bfd
   neighbor 10.2.1.20 route-reflector-client
   neighbor 10.2.1.20 password 7 8Vjk+esrlzU=
   neighbor 10.2.1.30 remote-as 65001
   neighbor 10.2.1.30 bfd
   neighbor 10.2.1.30 route-reflector-client
   neighbor 10.2.1.30 password 7 8Vjk+esrlzU=
   redistribute connected
!
router isis COD1 instance-id 1
   shutdown
   net 49.0001.1000.0000.1101.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.101
   shutdown
   bfd default
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 l0w89E/+c20=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 v1srhOqHBBs=
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
router bgp 65001
   router-id 10.0.1.102
   timers bgp 10 30
   bgp route-reflector preserve-attributes
   neighbor 10.2.1.12 remote-as 65001
   neighbor 10.2.1.12 bfd
   neighbor 10.2.1.12 route-reflector-client
   neighbor 10.2.1.12 password 7 fG32y0nO3Zs=
   neighbor 10.2.1.22 remote-as 65001
   neighbor 10.2.1.22 bfd
   neighbor 10.2.1.22 route-reflector-client
   neighbor 10.2.1.22 password 7 hNgRYQupNIA=
   neighbor 10.2.1.32 remote-as 65001
   neighbor 10.2.1.32 bfd
   neighbor 10.2.1.32 route-reflector-client
   neighbor 10.2.1.32 password 7 hNgRYQupNIA=
   redistribute connected
!
router isis COD1 instance-id 1
   shutdown
   net 49.0001.1000.0000.1102.00
   authentication mode md5
   authentication key 7 G4eexUMXTFY=
   !
   address-family ipv4 unicast
!
router ospf 1
   router-id 10.0.1.102
   shutdown
   bfd default
   area 0.0.0.1 default-cost 1
   max-lsa 12000
!
end
spine2#

```

```
################################################################################

Status iBGP, BFD, IP Route


leaf1#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.1, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.11        4  65001           7379      7375    0    0 20:28:15 Estab   8      8
  10.2.1.13        4  65001           7375      7374    0    0 20:27:22 Estab   8      8

leaf1#show bfd peers
VRF name: default
-----------------
DstAddr        MyDisc    YourDisc  Interface/Transport    Type          LastUp 
---------- ----------- ----------- -------------------- ------- ---------------
10.2.1.11  3051202285   888506407        Ethernet1(14)  normal  11/30/24 15:48 
10.2.1.13  2909009916  3478721614        Ethernet2(15)  normal  11/30/24 15:49 

   LastDown            LastDiag    State
-------------- ------------------- -----
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
leaf1#


leaf2#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.2, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.21        4  65001           7378      7374    0    0 20:28:05 Estab   9      9
  10.2.1.23        4  65001           7378      7374    0    0 20:27:27 Estab   9      9

leaf2#show bfd peers
VRF name: default
-----------------
DstAddr        MyDisc    YourDisc  Interface/Transport    Type          LastUp 
---------- ----------- ----------- -------------------- ------- ---------------
10.2.1.21   148557796  3284322360        Ethernet1(14)  normal  11/30/24 15:48 
10.2.1.23  4279555162   918602193        Ethernet2(15)  normal  11/30/24 15:49 

   LastDown            LastDiag    State
-------------- ------------------- -----
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
leaf2#


leaf3#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.3, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.31        4  65001           7375      7372    0    0 20:27:47 Estab   9      9
  10.2.1.33        4  65001           7375      7376    0    0 20:27:33 Estab   9      9

leaf3#show bfd peers
VRF name: default
-----------------
DstAddr        MyDisc    YourDisc  Interface/Transport    Type          LastUp 
---------- ----------- ----------- -------------------- ------- ---------------
10.2.1.31  3377572386   750347607        Ethernet1(14)  normal  11/30/24 15:49 
10.2.1.33  3846294028  1016138823        Ethernet2(15)  normal  11/30/24 15:49 

   LastDown            LastDiag    State
-------------- ------------------- -----
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
leaf3#


spine1#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.101, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.10        4  65001           7376      7380    0    0 20:28:28 Estab   6      6
  10.2.1.20        4  65001           7375      7379    0    0 20:28:14 Estab   6      6
  10.2.1.30        4  65001           7373      7376    0    0 20:27:52 Estab   6      6

spine1#show bfd peers
VRF name: default
-----------------
DstAddr        MyDisc    YourDisc  Interface/Transport    Type          LastUp 
---------- ----------- ----------- -------------------- ------- ---------------
10.2.1.10   888506407  3051202285        Ethernet1(14)  normal  11/30/24 15:48 
10.2.1.20  3284322360   148557796        Ethernet2(15)  normal  11/30/24 15:48 
10.2.1.30   750347607  3377572386        Ethernet3(16)  normal  11/30/24 15:49 

   LastDown            LastDiag    State
-------------- ------------------- -----
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
spine1#


spine2#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.102, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.12        4  65001           7375      7376    0    0 20:27:37 Estab   8      8
  10.2.1.22        4  65001           7375      7379    0    0 20:27:39 Estab   8      8
  10.2.1.32        4  65001           7376      7377    0    0 20:27:40 Estab   8      8

spine2#show bfd peers
VRF name: default
-----------------
DstAddr        MyDisc    YourDisc  Interface/Transport    Type          LastUp 
---------- ----------- ----------- -------------------- ------- ---------------
10.2.1.12  3478721614  2909009916        Ethernet1(14)  normal  11/30/24 15:49 
10.2.1.22   918602193  4279555162        Ethernet2(15)  normal  11/30/24 15:49 
10.2.1.32  1016138823  3846294028        Ethernet3(16)  normal  11/30/24 15:49 

   LastDown            LastDiag    State
-------------- ------------------- -----
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up
         NA       No Diagnostic       Up

spine2#




leaf1#show ip route
 C        10.0.1.1/32 is directly connected, Loopback1
 B I      10.0.1.2/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.3/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.101/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.13, Ethernet2
 C        10.2.1.10/31 is directly connected, Ethernet1
 C        10.2.1.12/31 is directly connected, Ethernet2
 B I      10.2.1.20/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.22/31 [200/0] via 10.2.1.13, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.13, Ethernet2
leaf1#


leaf2#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.21, Ethernet1
 C        10.0.1.2/32 is directly connected, Loopback1
 B I      10.0.1.3/32 [200/0] via 10.2.1.21, Ethernet1
 B I      10.0.1.101/32 [200/0] via 10.2.1.21, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.23, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.21, Ethernet1
 B I      10.2.1.12/31 [200/0] via 10.2.1.23, Ethernet2
 C        10.2.1.20/31 is directly connected, Ethernet1
 C        10.2.1.22/31 is directly connected, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.21, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.23, Ethernet2
leaf2#
 
 
 
leaf3#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.31, Ethernet1
 B I      10.0.1.2/32 [200/0] via 10.2.1.31, Ethernet1
 C        10.0.1.3/32 is directly connected, Loopback1
 B I      10.0.1.101/32 [200/0] via 10.2.1.31, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.31, Ethernet1
 B I      10.2.1.12/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.20/31 [200/0] via 10.2.1.31, Ethernet1
 B I      10.2.1.22/31 [200/0] via 10.2.1.33, Ethernet2
 C        10.2.1.30/31 is directly connected, Ethernet1
 C        10.2.1.32/31 is directly connected, Ethernet2
leaf3#



spine1#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.10, Ethernet1
 B I      10.0.1.2/32 [200/0] via 10.2.1.20, Ethernet2
 B I      10.0.1.3/32 [200/0] via 10.2.1.30, Ethernet3
 C        10.0.1.101/32 is directly connected, Loopback1
 B I      10.0.1.102/32 [200/0] via 10.2.1.10, Ethernet1
 C        10.2.1.10/31 is directly connected, Ethernet1
 B I      10.2.1.12/31 [200/0] via 10.2.1.10, Ethernet1
 C        10.2.1.20/31 is directly connected, Ethernet2
 B I      10.2.1.22/31 [200/0] via 10.2.1.20, Ethernet2
 C        10.2.1.30/31 is directly connected, Ethernet3
 B I      10.2.1.32/31 [200/0] via 10.2.1.30, Ethernet3
spine1#



spine2#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.12, Ethernet1
 B I      10.0.1.2/32 [200/0] via 10.2.1.22, Ethernet2
 B I      10.0.1.3/32 [200/0] via 10.2.1.32, Ethernet3
 B I      10.0.1.101/32 [200/0] via 10.2.1.12, Ethernet1
 C        10.0.1.102/32 is directly connected, Loopback1
 B I      10.2.1.10/31 [200/0] via 10.2.1.12, Ethernet1
 C        10.2.1.12/31 is directly connected, Ethernet1
 B I      10.2.1.20/31 [200/0] via 10.2.1.22, Ethernet2
 C        10.2.1.22/31 is directly connected, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.32, Ethernet3
 C        10.2.1.32/31 is directly connected, Ethernet3
spine2#


leaf1#show ip bgp detail 
BGP routing table information for VRF default
Router identifier 10.0.1.1, local AS number 65001
BGP routing table entry for 10.0.1.1/32
 Paths: 1 available
  Local
    - from - (10.0.1.1)
      Origin IGP, metric 0, localpref 0, IGP metric -, weight -, received 20:43:08 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.2/32
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.20 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:42:33 ago, valid, internal, best
      Originator: 10.0.1.2, Cluster list: 10.0.1.101
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.22 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:41:54 ago, valid, internal
      Originator: 10.0.1.2, Cluster list: 10.0.1.102
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.3/32
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.30 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:42:10 ago, valid, internal, best
      Originator: 10.0.1.3, Cluster list: 10.0.1.101
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.32 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:41:54 ago, valid, internal
      Originator: 10.0.1.3, Cluster list: 10.0.1.102
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.101/32
 Paths: 1 available
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:42:47 ago, valid, internal, best
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.102/32
 Paths: 1 available
  Local (Received from a RR-client)
    10.2.1.13 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:41:54 ago, valid, internal, best
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.10/31
 Paths: 2 available
  Local
    - from - (10.0.1.1)
      Origin IGP, metric 1, localpref 0, IGP metric -, weight -, received 20:42:49 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:42:47 ago, valid, internal
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.12/31
 Paths: 2 available
  Local
    - from - (10.0.1.1)
      Origin IGP, metric 1, localpref 0, IGP metric -, weight -, received 20:42:49 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.13 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:41:54 ago, valid, internal
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.20/31
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:42:47 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.22 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:41:54 ago, valid, internal
      Originator: 10.0.1.2, Cluster list: 10.0.1.102
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.22/31
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.13 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:41:54 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.20 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:42:33 ago, valid, internal
      Originator: 10.0.1.2, Cluster list: 10.0.1.101
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.30/31
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:42:47 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.32 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:41:54 ago, valid, internal
      Originator: 10.0.1.3, Cluster list: 10.0.1.102
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.32/31
 Paths: 2 available
  Local (Received from a RR-client)
    10.2.1.13 from 10.2.1.13 (10.0.1.102)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:41:54 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.30 from 10.2.1.11 (10.0.1.101)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:42:10 ago, valid, internal
      Originator: 10.0.1.3, Cluster list: 10.0.1.101
      Rx SAFI: Unicast
leaf1#




spine2#show ip bgp detail 
BGP routing table information for VRF default
Router identifier 10.0.1.102, local AS number 65001
BGP routing table entry for 10.0.1.1/32
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.12 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:06 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.10 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:08 ago, valid, internal
      Originator: 10.0.1.1, Cluster list: 10.0.1.2 10.0.1.101
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.10 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:09 ago, valid, internal
      Originator: 10.0.1.1, Cluster list: 10.0.1.3 10.0.1.101
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.2/32
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.22 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:08 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.20 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:06 ago, valid, internal
      Originator: 10.0.1.2, Cluster list: 10.0.1.1 10.0.1.101
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.20 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:09 ago, valid, internal
      Originator: 10.0.1.2, Cluster list: 10.0.1.3 10.0.1.101
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.3/32
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.32 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:09 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.30 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:06 ago, valid, internal
      Originator: 10.0.1.3, Cluster list: 10.0.1.1 10.0.1.101
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.30 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:08 ago, valid, internal
      Originator: 10.0.1.3, Cluster list: 10.0.1.2 10.0.1.101
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.101/32
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:06 ago, valid, internal, best
      Originator: 10.0.1.101, Cluster list: 10.0.1.1
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.21 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:08 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.2
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.31 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:09 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.3
      Rx SAFI: Unicast
BGP routing table entry for 10.0.1.102/32
 Paths: 1 available
  Local
    - from - (10.0.1.102)
      Origin IGP, metric 0, localpref 0, IGP metric -, weight -, received 20:43:39 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.10/31
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.12 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:06 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.21 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:08 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.2
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.31 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:09 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.3
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.12/31
 Paths: 2 available
  Local
    - from - (10.0.1.102)
      Origin IGP, metric 1, localpref 0, IGP metric -, weight -, received 20:43:09 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.12 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:06 ago, valid, internal
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.20/31
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.22 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:08 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:06 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.1
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.31 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:09 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.3
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.22/31
 Paths: 2 available
  Local
    - from - (10.0.1.102)
      Origin IGP, metric 1, localpref 0, IGP metric -, weight -, received 20:43:09 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.22 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:08 ago, valid, internal
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.30/31
 Paths: 3 available
  Local (Received from a RR-client)
    10.2.1.32 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:09 ago, valid, internal, best
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.11 from 10.2.1.12 (10.0.1.1)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:06 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.1
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.21 from 10.2.1.22 (10.0.1.2)
      Origin IGP, metric 0, localpref 100, IGP metric 0, weight 0, received 20:43:08 ago, valid, internal
      Originator: 10.0.1.101, Cluster list: 10.0.1.2
      Rx SAFI: Unicast
BGP routing table entry for 10.2.1.32/31
 Paths: 2 available
  Local
    - from - (10.0.1.102)
      Origin IGP, metric 1, localpref 0, IGP metric -, weight -, received 20:43:09 ago, valid, local, best, redistributed (Connected)
      Rx SAFI: Unicast
  Local (Received from a RR-client)
    10.2.1.32 from 10.2.1.32 (10.0.1.3)
      Origin IGP, metric 0, localpref 100, IGP metric 1, weight 0, received 20:43:09 ago, valid, internal
      Rx SAFI: Unicast
spine2#




leaf1#ping 10.0.1.1 source 10.0.1.1
PING 10.0.1.1 (10.0.1.1) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.1: icmp_seq=1 ttl=64 time=1.18 ms
80 bytes from 10.0.1.1: icmp_seq=2 ttl=64 time=0.301 ms
80 bytes from 10.0.1.1: icmp_seq=3 ttl=64 time=0.298 ms
80 bytes from 10.0.1.1: icmp_seq=4 ttl=64 time=0.213 ms
80 bytes from 10.0.1.1: icmp_seq=5 ttl=64 time=0.412 ms

--- 10.0.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 15ms
rtt min/avg/max/mdev = 0.213/0.481/1.184/0.357 ms, ipg/ewma 3.799/0.822 ms
leaf1#ping 10.0.1.2 source 10.0.1.1 
PING 10.0.1.2 (10.0.1.2) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.2: icmp_seq=1 ttl=63 time=51.8 ms
80 bytes from 10.0.1.2: icmp_seq=2 ttl=63 time=46.5 ms
80 bytes from 10.0.1.2: icmp_seq=3 ttl=63 time=57.0 ms
80 bytes from 10.0.1.2: icmp_seq=4 ttl=63 time=55.7 ms
80 bytes from 10.0.1.2: icmp_seq=5 ttl=63 time=53.9 ms

--- 10.0.1.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 43ms
rtt min/avg/max/mdev = 46.578/53.039/57.054/3.673 ms, pipe 5, ipg/ewma 10.978/52.593 ms
leaf1#ping 10.0.1.3 source 10.0.1.1 
PING 10.0.1.3 (10.0.1.3) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.3: icmp_seq=1 ttl=63 time=28.7 ms
80 bytes from 10.0.1.3: icmp_seq=2 ttl=63 time=21.4 ms
80 bytes from 10.0.1.3: icmp_seq=3 ttl=63 time=34.1 ms
80 bytes from 10.0.1.3: icmp_seq=4 ttl=63 time=23.4 ms
80 bytes from 10.0.1.3: icmp_seq=5 ttl=63 time=15.6 ms

--- 10.0.1.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 81ms
rtt min/avg/max/mdev = 15.643/24.714/34.171/6.330 ms, pipe 3, ipg/ewma 20.330/26.470 ms
leaf1#ping 10.0.1.101 source 10.0.1.1 
PING 10.0.1.101 (10.0.1.101) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.101: icmp_seq=1 ttl=64 time=11.0 ms
80 bytes from 10.0.1.101: icmp_seq=2 ttl=64 time=6.95 ms
80 bytes from 10.0.1.101: icmp_seq=3 ttl=64 time=12.6 ms
80 bytes from 10.0.1.101: icmp_seq=4 ttl=64 time=18.1 ms
80 bytes from 10.0.1.101: icmp_seq=5 ttl=64 time=16.5 ms

--- 10.0.1.101 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 54ms
rtt min/avg/max/mdev = 6.951/13.078/18.112/3.982 ms, pipe 2, ipg/ewma 13.634/12.336 ms
leaf1#ping 10.0.1.102 source 10.0.1.1 
PING 10.0.1.102 (10.0.1.102) from 10.0.1.1 : 72(100) bytes of data.
80 bytes from 10.0.1.102: icmp_seq=1 ttl=64 time=18.3 ms
80 bytes from 10.0.1.102: icmp_seq=2 ttl=64 time=7.44 ms
80 bytes from 10.0.1.102: icmp_seq=3 ttl=64 time=8.47 ms
80 bytes from 10.0.1.102: icmp_seq=4 ttl=64 time=7.61 ms
80 bytes from 10.0.1.102: icmp_seq=5 ttl=64 time=7.74 ms

--- 10.0.1.102 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 70ms
rtt min/avg/max/mdev = 7.445/9.928/18.367/4.234 ms, ipg/ewma 17.740/14.001 ms
leaf1#



spine2#ping 10.0.1.1 source 10.0.1.102
PING 10.0.1.1 (10.0.1.1) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.1: icmp_seq=1 ttl=64 time=10.6 ms
80 bytes from 10.0.1.1: icmp_seq=2 ttl=64 time=13.3 ms
80 bytes from 10.0.1.1: icmp_seq=3 ttl=64 time=27.5 ms
80 bytes from 10.0.1.1: icmp_seq=4 ttl=64 time=15.9 ms
80 bytes from 10.0.1.1: icmp_seq=5 ttl=64 time=13.0 ms

--- 10.0.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 63ms
rtt min/avg/max/mdev = 10.692/16.118/27.516/5.934 ms, pipe 2, ipg/ewma 15.960/13.398 ms
spine2#ping 10.0.1.2 source 10.0.1.102 
PING 10.0.1.2 (10.0.1.2) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.2: icmp_seq=1 ttl=64 time=24.5 ms
80 bytes from 10.0.1.2: icmp_seq=2 ttl=64 time=13.6 ms
80 bytes from 10.0.1.2: icmp_seq=3 ttl=64 time=6.92 ms
80 bytes from 10.0.1.2: icmp_seq=4 ttl=64 time=7.05 ms
80 bytes from 10.0.1.2: icmp_seq=5 ttl=64 time=9.92 ms

--- 10.0.1.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 82ms
rtt min/avg/max/mdev = 6.929/12.411/24.508/6.523 ms, pipe 2, ipg/ewma 20.505/18.183 ms
spine2#ping 10.0.1.3 source 10.0.1.102 
PING 10.0.1.3 (10.0.1.3) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.3: icmp_seq=1 ttl=64 time=14.9 ms
80 bytes from 10.0.1.3: icmp_seq=2 ttl=64 time=21.0 ms
80 bytes from 10.0.1.3: icmp_seq=3 ttl=64 time=8.02 ms
80 bytes from 10.0.1.3: icmp_seq=4 ttl=64 time=10.6 ms
80 bytes from 10.0.1.3: icmp_seq=5 ttl=64 time=10.9 ms

--- 10.0.1.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 66ms
rtt min/avg/max/mdev = 8.029/13.126/21.034/4.534 ms, pipe 2, ipg/ewma 16.530/13.835 ms
spine2#ping 10.0.1.101 source 10.0.1.102 
PING 10.0.1.101 (10.0.1.101) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.101: icmp_seq=1 ttl=63 time=42.2 ms
80 bytes from 10.0.1.101: icmp_seq=2 ttl=63 time=42.9 ms
80 bytes from 10.0.1.101: icmp_seq=3 ttl=63 time=39.2 ms
80 bytes from 10.0.1.101: icmp_seq=4 ttl=63 time=26.7 ms
80 bytes from 10.0.1.101: icmp_seq=5 ttl=63 time=20.1 ms

--- 10.0.1.101 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 110ms
rtt min/avg/max/mdev = 20.137/34.279/42.937/9.175 ms, pipe 3, ipg/ewma 27.701/37.582 ms
spine2#ping 10.0.1.102 source 10.0.1.102  
PING 10.0.1.102 (10.0.1.102) from 10.0.1.102 : 72(100) bytes of data.
80 bytes from 10.0.1.102: icmp_seq=1 ttl=64 time=0.628 ms
80 bytes from 10.0.1.102: icmp_seq=2 ttl=64 time=0.156 ms
80 bytes from 10.0.1.102: icmp_seq=3 ttl=64 time=0.194 ms
80 bytes from 10.0.1.102: icmp_seq=4 ttl=64 time=0.433 ms
80 bytes from 10.0.1.102: icmp_seq=5 ttl=64 time=0.164 ms

--- 10.0.1.102 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 16ms
rtt min/avg/max/mdev = 0.156/0.315/0.628/0.186 ms, ipg/ewma 4.036/0.467 ms
spine2#
```
<img width="536" alt="shema-seti_in_emergency_mode" src="https://github.com/user-attachments/assets/6fca2c21-252c-4f31-a23f-7b0d5dece827">


```
###########################################################################
Network emergency mode:
leaf1  if-Et2 -> shutdown 
leaf3  if-Et1 -> shutdown 


leaf1#configure 
leaf1(config)#interface ethernet 2
leaf1(config-if-Et2)#shutdown 
leaf1(config-if-Et2)#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.1, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.11        4  65001           7707      7699    0    0 21:21:44 Estab   8      8
  10.2.1.13        4  65001           7696      7697    0    0 00:00:04 Active         
leaf1(config-if-Et2)#show ip route
 C        10.0.1.1/32 is directly connected, Loopback1
 B I      10.0.1.2/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.3/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.101/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.11, Ethernet1
 C        10.2.1.10/31 is directly connected, Ethernet1
 B I      10.2.1.20/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.22/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.11, Ethernet1
leaf1(config-if-Et2)#


leaf3#configure 
leaf3(config)#interface ethernet 1
leaf3(config-if-Et1)#shutdown 
leaf3(config-if-Et1)#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.3, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.31        4  65001           7694      7692    0    0 00:00:18 Active         
  10.2.1.33        4  65001           7703      7699    0    0 21:21:00 Estab   8      8
leaf3(config-if-Et1)#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.0.1.2/32 [200/0] via 10.2.1.33, Ethernet2
 C        10.0.1.3/32 is directly connected, Loopback1
 B I      10.0.1.101/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.0.1.102/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.20/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.22/31 [200/0] via 10.2.1.33, Ethernet2
 C        10.2.1.32/31 is directly connected, Ethernet2
leaf3(config-if-Et1)#


leaf2#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.2, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.21        4  65001           7726      7720    0    0 21:25:08 Estab   4      4
  10.2.1.23        4  65001           7726      7720    0    0 21:24:30 Estab   4      4
leaf2#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.21, Ethernet1
 C        10.0.1.2/32 is directly connected, Loopback1
 B I      10.0.1.3/32 [200/0] via 10.2.1.23, Ethernet2
 B I      10.0.1.101/32 [200/0] via 10.2.1.21, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.23, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.21, Ethernet1
 C        10.2.1.20/31 is directly connected, Ethernet1
 C        10.2.1.22/31 is directly connected, Ethernet2
 B I      10.2.1.32/31 [200/0] via 10.2.1.23, Ethernet2
leaf2#
```
