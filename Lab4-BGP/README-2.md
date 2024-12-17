The network operates on iBGP, but is ready for OSPF or IS-IS protocols.   
Схема модифицировна для исключения Split Brain: RR только на Spine коммутаторах, для максимальной надежности добавлена перемычка между обоими Spine, т.к. надежность сети с моем варианте на 1 месте.   
Files:   
shema-seti_max-reliability.png - Схема сети.   
ip-plan_BGP_(+OSPF+ISIS)-max-reliability.txt - общий IP план для BGP и протколов OSPF и IS-IS.   
config_all_max-reliability.txt - конфигурации всех коммутаторов.   
show_max-reliability.txt - просмотр состояний BGP, ip маршрутов, в т.ч. при отказе линков.   


<img width="547" alt="shema-seti_max-reliability" src="https://github.com/user-attachments/assets/073e8b3c-00bd-461a-91ce-ded09b5c5c28" />

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
spine1 10.2.1.0/31 - spine2 10.2.1.1/31 

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
container-manager
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
interface Loopback2
   ip address 10.1.1.1/32
!
interface Management1
!
ip routing
!
router bgp 65001
   router-id 10.0.1.1
   timers bgp 10 30
   maximum-paths 3
   neighbor 10.2.1.11 remote-as 65001
   neighbor 10.2.1.11 bfd
   neighbor 10.2.1.11 password 7 pbiybVavL8E=
   neighbor 10.2.1.13 remote-as 65001
   neighbor 10.2.1.13 bfd
   neighbor 10.2.1.13 password 7 fG32y0nO3Zs=
   network 10.0.1.1/32
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
interface Loopback2
   ip address 10.1.1.2/32
!
interface Management1
!
ip routing
!
router bgp 65001
   router-id 10.0.1.2
   timers bgp 10 30
   maximum-paths 3
   neighbor 10.2.1.21 remote-as 65001
   neighbor 10.2.1.21 bfd
   neighbor 10.2.1.21 password 7 8Vjk+esrlzU=
   neighbor 10.2.1.23 remote-as 65001
   neighbor 10.2.1.23 bfd
   neighbor 10.2.1.23 password 7 hNgRYQupNIA=
   network 10.0.1.2/32
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
interface Loopback2
   ip address 10.1.1.3/32
!
interface Management1
!
ip routing
!
router bgp 65001
   router-id 10.0.1.3
   timers bgp 10 30
   maximum-paths 3
   neighbor 10.2.1.31 remote-as 65001
   neighbor 10.2.1.31 bfd
   neighbor 10.2.1.31 password 7 8Vjk+esrlzU=
   neighbor 10.2.1.33 remote-as 65001
   neighbor 10.2.1.33 bfd
   neighbor 10.2.1.33 password 7 hNgRYQupNIA=
   network 10.0.1.3/32
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
   description spine1-spine2
   no switchport
   ip address 10.2.1.0/31
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 De48PcrXdHk=
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
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
   maximum-paths 3
   neighbor 10.2.1.1 remote-as 65001
   neighbor 10.2.1.1 bfd
   neighbor 10.2.1.1 route-reflector-client
   neighbor 10.2.1.1 password 7 gYRYw6fZQbA=
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
   description spine2-spine1
   no switchport
   ip address 10.2.1.1/31
   bfd interval 500 min-rx 500 multiplier 3
   ip ospf network point-to-point
   ip ospf authentication message-digest
   ip ospf area 0.0.0.1
   ip ospf message-digest-key 1 md5 7 De48PcrXdHk=
   isis enable COD1
   isis circuit-type level-1
   isis network point-to-point
   isis authentication mode md5 level-1
   isis authentication key 7 G4eexUMXTFY= level-1
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
   maximum-paths 3
   neighbor 10.2.1.0 remote-as 65001
   neighbor 10.2.1.0 bfd
   neighbor 10.2.1.0 route-reflector-client
   neighbor 10.2.1.0 password 7 gYRYw6fZQbA=
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

Status iBGP, IP Route

leaf1#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.1, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.11        4  65001            160       142    0    0 00:22:33 Estab   11     11
  10.2.1.13        4  65001          94796     94770    0    0    1d23h Estab   11     11
leaf1#

leaf2#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.2, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.21        4  65001          95086     95045    0    0    1d23h Estab   11     11
  10.2.1.23        4  65001          95088     95049    0    0    1d23h Estab   11     11
leaf2#

leaf3#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.3, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.31        4  65001          94774     94746    0    0    1d23h Estab   11     11
  10.2.1.33        4  65001          95079     95051    0    0    1d23h Estab   11     11
leaf3#

spine1#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.101, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.1         4  65001          17015     17017    0    0    1d23h Estab   8      8
  10.2.1.10        4  65001            143       161    0    0 00:22:42 Estab   1      1
  10.2.1.20        4  65001          16989     17019    0    0    1d23h Estab   1      1
  10.2.1.30        4  65001          16991     17019    0    0    1d23h Estab   1      1
spine1#


spine2#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.102, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.0         4  65001          17017     17015    0    0    1d23h Estab   8      8
  10.2.1.12        4  65001          16992     17021    0    0    1d23h Estab   1      1
  10.2.1.22        4  65001          16990     17022    0    0    1d23h Estab   1      1
  10.2.1.32        4  65001          16992     17020    0    0    1d23h Estab   1      1
spine2#

************************************************************************************************

leaf1#show ip route
 C        10.0.1.1/32 is directly connected, Loopback1
 B I      10.0.1.2/32 [200/0] via 10.2.1.11, Ethernet1
                              via 10.2.1.13, Ethernet2
 B I      10.0.1.3/32 [200/0] via 10.2.1.11, Ethernet1
                              via 10.2.1.13, Ethernet2
 B I      10.0.1.101/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.13, Ethernet2
 C        10.1.1.1/32 is directly connected, Loopback2
 B I      10.2.1.0/31 [200/0] via 10.2.1.11, Ethernet1
                              via 10.2.1.13, Ethernet2
 C        10.2.1.10/31 is directly connected, Ethernet1
 C        10.2.1.12/31 is directly connected, Ethernet2
 B I      10.2.1.20/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.22/31 [200/0] via 10.2.1.13, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.13, Ethernet2
leaf1#

leaf2#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.21, Ethernet1
                              via 10.2.1.23, Ethernet2
 C        10.0.1.2/32 is directly connected, Loopback1
 B I      10.0.1.3/32 [200/0] via 10.2.1.21, Ethernet1
                              via 10.2.1.23, Ethernet2
 B I      10.0.1.101/32 [200/0] via 10.2.1.21, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.23, Ethernet2
 C        10.1.1.2/32 is directly connected, Loopback2
 B I      10.2.1.0/31 [200/0] via 10.2.1.21, Ethernet1
                              via 10.2.1.23, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.21, Ethernet1
 B I      10.2.1.12/31 [200/0] via 10.2.1.23, Ethernet2
 C        10.2.1.20/31 is directly connected, Ethernet1
 C        10.2.1.22/31 is directly connected, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.21, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.23, Ethernet2
leaf2#

leaf3#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.31, Ethernet1
                              via 10.2.1.33, Ethernet2
 B I      10.0.1.2/32 [200/0] via 10.2.1.31, Ethernet1
                              via 10.2.1.33, Ethernet2
 C        10.0.1.3/32 is directly connected, Loopback1
 B I      10.0.1.101/32 [200/0] via 10.2.1.31, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.33, Ethernet2
 C        10.1.1.3/32 is directly connected, Loopback2
 B I      10.2.1.0/31 [200/0] via 10.2.1.31, Ethernet1
                              via 10.2.1.33, Ethernet2
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
 B I      10.0.1.102/32 [200/0] via 10.2.1.1, Ethernet4
 C        10.2.1.0/31 is directly connected, Ethernet4
 C        10.2.1.10/31 is directly connected, Ethernet1
 B I      10.2.1.12/31 [200/0] via 10.2.1.1, Ethernet4
 C        10.2.1.20/31 is directly connected, Ethernet2
 B I      10.2.1.22/31 [200/0] via 10.2.1.1, Ethernet4
 C        10.2.1.30/31 is directly connected, Ethernet3
 B I      10.2.1.32/31 [200/0] via 10.2.1.1, Ethernet4
spine1#

spine2#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.12, Ethernet1
 B I      10.0.1.2/32 [200/0] via 10.2.1.22, Ethernet2
 B I      10.0.1.3/32 [200/0] via 10.2.1.32, Ethernet3
 B I      10.0.1.101/32 [200/0] via 10.2.1.0, Ethernet4
 C        10.0.1.102/32 is directly connected, Loopback1
 C        10.2.1.0/31 is directly connected, Ethernet4
 B I      10.2.1.10/31 [200/0] via 10.2.1.0, Ethernet4
 C        10.2.1.12/31 is directly connected, Ethernet1
 B I      10.2.1.20/31 [200/0] via 10.2.1.0, Ethernet4
 C        10.2.1.22/31 is directly connected, Ethernet2
 B I      10.2.1.30/31 [200/0] via 10.2.1.0, Ethernet4
 C        10.2.1.32/31 is directly connected, Ethernet3
spine2#

*************************************************************************

leaf1#configure 
leaf1(config)#interface ethernet 2
leaf1(config-if-Et2)#shutdown 

leaf1(config-if-Et2)#show ip route
 C        10.0.1.1/32 is directly connected, Loopback1
 B I      10.0.1.2/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.3/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.101/32 [200/0] via 10.2.1.11, Ethernet1
 B I      10.0.1.102/32 [200/0] via 10.2.1.11, Ethernet1
 C        10.1.1.1/32 is directly connected, Loopback2
 B I      10.2.1.0/31 [200/0] via 10.2.1.11, Ethernet1
 C        10.2.1.10/31 is directly connected, Ethernet1
 B I      10.2.1.20/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.22/31 [200/0] via 10.2.1.11, Ethernet1
 B I      10.2.1.32/31 [200/0] via 10.2.1.11, Ethernet1

leaf1(config-if-Et2)#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.1, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.11        4  65001            199       178    0    0 00:28:35 Estab   9      9
  10.2.1.13        4  65001          94831     94805    0    0 00:00:30 Active         
leaf1(config-if-Et2)#


leaf3#configure 
leaf3(config)#interface ethernet 1
leaf3(config-if-Et1)#shutdown 

leaf3(config-if-Et1)#show ip route
 B I      10.0.1.1/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.0.1.2/32 [200/0] via 10.2.1.33, Ethernet2
 C        10.0.1.3/32 is directly connected, Loopback1
 B I      10.0.1.101/32 [200/0] via 10.2.1.33, Ethernet2
 B I      10.0.1.102/32 [200/0] via 10.2.1.33, Ethernet2
 C        10.1.1.3/32 is directly connected, Loopback2
 B I      10.2.1.0/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.10/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.20/31 [200/0] via 10.2.1.33, Ethernet2
 B I      10.2.1.22/31 [200/0] via 10.2.1.33, Ethernet2
 C        10.2.1.32/31 is directly connected, Ethernet2

leaf3(config-if-Et1)#show ip bgp summary
BGP summary information for VRF default
Router identifier 10.0.1.3, local AS number 65001
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  10.2.1.31        4  65001          94806     94779    0    0 00:00:21 Active         
  10.2.1.33        4  65001          95116     95085    0    0    1d23h Estab   9      9
leaf3(config-if-Et1)#

leaf3(config-if-Et1)#ping 10.0.1.1 source 10.0.1.3
PING 10.0.1.1 (10.0.1.1) from 10.0.1.3 : 72(100) bytes of data.
80 bytes from 10.0.1.1: icmp_seq=1 ttl=62 time=33.6 ms
80 bytes from 10.0.1.1: icmp_seq=2 ttl=62 time=29.6 ms
80 bytes from 10.0.1.1: icmp_seq=3 ttl=62 time=28.5 ms
80 bytes from 10.0.1.1: icmp_seq=4 ttl=62 time=34.1 ms
80 bytes from 10.0.1.1: icmp_seq=5 ttl=62 time=33.1 ms

--- 10.0.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 100ms
rtt min/avg/max/mdev = 28.570/31.827/34.101/2.269 ms, pipe 3, ipg/ewma 25.153/32.826 ms
leaf3(config-if-Et1)#ping 10.0.1.2 source 10.0.1.3  
PING 10.0.1.2 (10.0.1.2) from 10.0.1.3 : 72(100) bytes of data.
80 bytes from 10.0.1.2: icmp_seq=1 ttl=63 time=29.9 ms
80 bytes from 10.0.1.2: icmp_seq=2 ttl=63 time=24.6 ms
80 bytes from 10.0.1.2: icmp_seq=3 ttl=63 time=23.4 ms
80 bytes from 10.0.1.2: icmp_seq=4 ttl=63 time=27.5 ms
80 bytes from 10.0.1.2: icmp_seq=5 ttl=63 time=21.5 ms

--- 10.0.1.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 87ms
rtt min/avg/max/mdev = 21.590/25.455/29.960/2.974 ms, pipe 3, ipg/ewma 21.946/27.590 ms
leaf3(config-if-Et1)#

```
