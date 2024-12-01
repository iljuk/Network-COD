The network operates on iBGP, but is ready for OSPF or IS-IS protocols.  
All switches are RR for maximum network reliability.  
Files:  
shema-seti.png - Схема сети.  
ip-plan_BGP_(+OSPF+ISIS).txt - общий IP план для BGP и протколов OSPF и IS-IS.  
config_all.txt - конфигурации всех коммутаторов.  
show_bgp_bfd_route.txt - просмотр состояний BGP, BFD, ip маршрутов.  
shema-seti_in_emergency_mode.png - Схема стеи при отказе 2 линков.  
show_in_emergency_mode.txt - Наличие ip связности при отказе линков через RR Leaf2.  

'''
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


'''

