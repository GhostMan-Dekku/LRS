# LRS

***
*** CONNECT TO 172.27.188.1:22
*** date 29/01/2020
*** time 10:55:52
***
                                                          
[SSH] Server Version Cisco-1.25
[SSH] Logged in (keyboard-interactive)
 

+-------------------------------------------------------------------------------------------------------------------+
|                                                                                                                   |
|  UNAUTHORIZED ACCESS TO THIS NETWORK DEVICE IS PROHIBITED. You must have explicit permission to access            |
|  or  configure this device. All activities performed on this device maybe logged, and violations of this          |
|  policy may result in disciplinary action, and may be reported to law enforcement. There is no right to privacy   |
|  on this device.                                                                                                  |
|                                                                                                                   |
|  ACCESO NO AUTORIZADO A ESTE EQUIPO ESTA PROHIBIDO. Usted debe estar autorizado para accesar a configurar         |
|  este equipo. Todas las actividades realizadas en este equipo seran monitoreadas y las violaciones a estas        |
|  politicas seran causantes de acciones disciplinarias y reportadas a la autoridad. No hay derecho de              |
|  privacidad en este equipo.                                                                                       |
|                                                                                                                   |
+-------------------------------------------------------------------------------------------------------------------+



MX-LRS1-S951.group.wan#ter len 0
MX-LRS1-S951.group.wan#sh run
Building configuration...

Current configuration : 41109 bytes
!
! Last configuration change at 13:20:34 CDT Fri Jan 17 2020 by g-mx-tla1-networking
! NVRAM config last updated at 03:53:22 CDT Sun Jan 12 2020 by g-mx-tla1-Networking
!
version 16.9
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service call-home
no platform punt-keepalive disable-kernel-core
!
hostname MX-LRS1-S951.group.wan
!
!
vrf definition Mgmt-vrf
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
 exit-address-family
!
enable secret 5 $1$FqUq$awqOMHEsYYNxtVjTSqG6T1
!
no aaa new-model
boot system switch all flash:packages.conf
clock timezone CDT -6 0
clock summer-time CDT recurring 1 Sun Mar 2:00 last Sun Oct 2:00
switch 1 provision c9500-16x
switch 2 provision c9500-16x
!
!
!
!
stackwise-virtual
 domain 13
call-home
 ! If contact email address in call-home is configured as sch-smart-licensing@cisco.com
 ! the email address configured in Cisco Smart License Portal will be used as contact email address to send SCH notifications.
 contact-email-addr sch-smart-licensing@cisco.com
 profile "CiscoTAC-1"
  active
  destination transport-method http
  no destination transport-method email
no ip source-route
ip routing
!
!
!
!
!
no ip domain lookup
ip dhcp smart-relay
!
!
!
login on-success log
!
!
!
!
!
!
!
!
flow record NET-FLOW-input
 description IPv4 NetFlow
 match ipv4 source address
 match ipv4 destination address
 match transport source-port
 match transport destination-port
 match ipv4 protocol
 match interface input
 match ipv4 tos
 match flow direction
 collect interface output
 collect counter bytes long
 collect counter packets long
 collect transport tcp flags
 collect timestamp absolute first
 collect timestamp absolute last
!
!
flow record NET-FLOW-output
 description IPv4 NetFlow
 match ipv4 source address
 match ipv4 destination address
 match transport source-port
 match transport destination-port
 match ipv4 protocol
 match interface input
 match ipv4 tos
 match flow direction
 collect interface output
 collect counter bytes long
 collect counter packets long
 collect transport tcp flags
 collect timestamp absolute first
 collect timestamp absolute last
!
!
flow exporter Server-Solar-WIND
 description Export to  Server-Solar-WIND
 destination 172.27.180.129
 source Vlan1102
 transport udp 2055
 template data timeout 60
!
!
flow monitor flow_mon_input
 description IPv4 NETFLOW ingress exports
 exporter Server-Solar-WIND
 cache timeout active 60
 record NET-FLOW-input
!
!
flow monitor flow_mon_output
 description IPv4 NETFLOW egress exports
 exporter Server-Solar-WIND
 cache timeout active 60
 record NET-FLOW-output
!
!
crypto pki trustpoint TP-self-signed-1914330892
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-1914330892
 revocation-check none
 rsakeypair TP-self-signed-1914330892
!
crypto pki trustpoint SLA-TrustPoint
 enrollment pkcs12
 revocation-check crl
!
!
crypto pki certificate chain TP-self-signed-1914330892
 certificate self-signed 01
  30820330 30820218 A0030201 02020101 300D0609 2A864886 F70D0101 05050030 
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274 
  69666963 6174652D 31393134 33333038 3932301E 170D3139 31303035 32323136 
  32365A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649 
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D31 39313433 
  33303839 32308201 22300D06 092A8648 86F70D01 01010500 0382010F 00308201 
  0A028201 01009F48 240B3B49 BE907864 9AB18E30 F135C0DE E97E0146 C0A326A3 
  3913A340 AA806B48 71BA8F62 35687928 6BB93EBE 6C2FA103 C62C396A C4DC1413 
  A3FA6464 286DBB99 9C7F9165 39DA6D90 F747EEE1 1D2C05A9 80EE484D A6B93BAA 
  71F72666 29CC491A D6363915 8E3F6425 89E7E44F 9B75E5FA 6EC70EDB 960E011B 
  66C7BA72 866A6AF9 A441A04F 09460C5A 4D9D76FF ECBE4256 DF1D47A9 28153BAD 
  FC822A47 35112247 035C264E 358A53E8 CA8CDBD0 1CBE420B DB3E3A1F 45D5F53D 
  E07757BE FF01913A 1E7D98A1 34D92F56 70CE5A5F 48781F67 CCD162D0 0193F3F5 
  3D852D68 66FCE364 2CE8DDDB 219CFE28 0207A093 F7CC51C7 1EF7508E CF698E5C 
  92B54157 D08F0203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 
  301F0603 551D2304 18301680 14325328 7976BAE7 0964302A 15E6A85F 2BA87432 
  AA301D06 03551D0E 04160414 32532879 76BAE709 64302A15 E6A85F2B A87432AA 
  300D0609 2A864886 F70D0101 05050003 82010100 27555AA5 7F1DE835 BAE31E41 
  CF312D8F AAB32800 45D719C9 954AF6F8 ECDDFDE9 1546B0D5 1887D210 AD1571E6 
  431F8144 0B31B2AF F53EBF5C 2F65FA1D 2A0EB705 DDBF579D E3705C69 62B45F1B 
  90723D9D 58A103EC 6440982E 46A5CDE6 2EA733BC 09753482 427CAE36 8CF06BF1 
  5925E2DE 1FB225C3 683C2FC4 55F0D181 CC4C5181 68248830 041A7887 4FB9FC27 
  E11F8320 9AB2C176 6F2BAF74 CDF40467 BD51B24A 7C962BF8 B311DD09 39D6FC49 
  82A21078 DFAB26D6 9E7EECCB FB4FED87 680C509F 044F34D6 5D8F7AC4 020767D1 
  B1BCE8B7 EDBB2350 76FBA031 66FCD152 01A54EB3 71BB326D BD656B7E FD97472E 
  F0BDB96F ACE8C958 A183137F 8395641E 10F9E6C3
  	quit
crypto pki certificate chain SLA-TrustPoint
 certificate ca 01
  30820321 30820209 A0030201 02020101 300D0609 2A864886 F70D0101 0B050030 
  32310E30 0C060355 040A1305 43697363 6F312030 1E060355 04031317 43697363 
  6F204C69 63656E73 696E6720 526F6F74 20434130 1E170D31 33303533 30313934 
  3834375A 170D3338 30353330 31393438 34375A30 32310E30 0C060355 040A1305 
  43697363 6F312030 1E060355 04031317 43697363 6F204C69 63656E73 696E6720 
  526F6F74 20434130 82012230 0D06092A 864886F7 0D010101 05000382 010F0030 
  82010A02 82010100 A6BCBD96 131E05F7 145EA72C 2CD686E6 17222EA1 F1EFF64D 
  CBB4C798 212AA147 C655D8D7 9471380D 8711441E 1AAF071A 9CAE6388 8A38E520 
  1C394D78 462EF239 C659F715 B98C0A59 5BBB5CBD 0CFEBEA3 700A8BF7 D8F256EE 
  4AA4E80D DB6FD1C9 60B1FD18 FFC69C96 6FA68957 A2617DE7 104FDC5F EA2956AC 
  7390A3EB 2B5436AD C847A2C5 DAB553EB 69A9A535 58E9F3E3 C0BD23CF 58BD7188 
  68E69491 20F320E7 948E71D7 AE3BCC84 F10684C7 4BC8E00F 539BA42B 42C68BB7 
  C7479096 B4CB2D62 EA2F505D C7B062A4 6811D95B E8250FC4 5D5D5FB8 8F27D191 
  C55F0D76 61F9A4CD 3D992327 A8BB03BD 4E6D7069 7CBADF8B DF5F4368 95135E44 
  DFC7C6CF 04DD7FD1 02030100 01A34230 40300E06 03551D0F 0101FF04 04030201 
  06300F06 03551D13 0101FF04 05300301 01FF301D 0603551D 0E041604 1449DC85 
  4B3D31E5 1B3E6A17 606AF333 3D3B4C73 E8300D06 092A8648 86F70D01 010B0500 
  03820101 00507F24 D3932A66 86025D9F E838AE5C 6D4DF6B0 49631C78 240DA905 
  604EDCDE FF4FED2B 77FC460E CD636FDB DD44681E 3A5673AB 9093D3B1 6C9E3D8B 
  D98987BF E40CBD9E 1AECA0C2 2189BB5C 8FA85686 CD98B646 5575B146 8DFC66A8 
  467A3DF4 4D565700 6ADF0F0D CF835015 3C04FF7C 21E878AC 11BA9CD2 55A9232C 
  7CA7B7E6 C1AF74F6 152E99B7 B1FCF9BB E973DE7F 5BDDEB86 C71E3B49 1765308B 
  5FB0DA06 B92AFE7F 494E8A9E 07B85737 F3A58BE1 1A48A229 C37C1E69 39F08678 
  80DDCD16 D6BACECA EEBC7CF9 8428787B 35202CDC 60E4616A B623CDBD 230E3AFB 
  418616A9 4093E049 4D10AB75 27E86F73 932E35B5 8862FDAE 0275156F 719BB2F0 
  D697DF7F 28
  	quit
!
license boot level network-advantage addon dna-advantage
!
!
diagnostic bootup level minimal
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1-1000 priority 4096
!
!
!
!
!
object-group network ServersVoice 
 host 172.27.188.25
 host 172.27.137.8
 host 172.27.157.18
 host 172.27.188.26
 host 172.27.188.27
!
object-group service TCP_Voz 
 tcp eq www
 tcp eq 443
 tcp eq 3128
 tcp eq 2000
 tcp eq 8080
 tcp eq 5060
 tcp eq domain
!
object-group service UDP_Voz 
 udp eq tftp
 udp eq 5060
 udp eq domain
 udp range 16384 32767
 udp eq ntp
!
object-group network VLANsVoice 
 172.27.158.128 255.255.255.128
 172.27.190.0 255.255.255.0
 172.27.137.128 255.255.255.128
 172.27.168.0 255.255.255.0
 172.27.150.128 255.255.255.128
 172.27.136.128 255.255.255.128
 172.27.156.128 255.255.255.128
 172.27.148.128 255.255.255.128
 172.27.154.128 255.255.255.128
 172.27.143.0 255.255.255.0
 172.27.173.0 255.255.255.0
!
object-group network VlansIpComunicator 
 172.27.157.128 255.255.255.128
 172.27.136.0 255.255.255.128
 172.27.166.0 255.255.254.0
 172.27.150.0 255.255.255.128
 172.27.148.0 255.255.255.128
 172.27.156.0 255.255.255.128
 172.27.146.0 255.255.255.128
 172.27.186.0 255.255.254.0
 172.27.154.0 255.255.255.128
 172.27.139.0 255.255.255.0
 172.27.174.0 255.255.254.0
!
!
username skm-admin privilege 15 secret 5 $1$zc87$apqduateB9JZNyhSMFLkZ0
username g-mx-tla1-Networking privilege 15 secret 5 $1$jNB6$XjqOWy/5kwZFyIpDUYHh91
username g-mx-tla1-Noctsmx privilege 15 secret 5 $1$yxjO$JiITC8C2WD601TJEaxHNR1
username jvazquez privilege 5 secret 5 $1$Eo1f$TRdfV1iGfctD4Q9AH90gz0
!
redundancy
 mode sso
!
!
!
!
!
transceiver type all
 monitoring
!
vlan 1100,1102,1203 
!
lldp run
!
class-map match-any system-cpp-police-topology-control
  description Topology control
class-map match-any system-cpp-police-sw-forward
  description Sw forwarding, L2 LVX data, LOGGING
class-map match-any system-cpp-default
  description Inter FED, EWLC control, EWLC data
class-map match-any system-cpp-police-sys-data
  description Learning cache ovfl, High Rate App, Exception, EGR Exception, NFL SAMPLED DATA, RPF Failed
class-map type access-control match-all CLASS_35MBPS
class-map match-any system-cpp-police-punt-webauth
  description Punt Webauth
class-map match-any system-cpp-police-l2lvx-control
  description L2 LVX control packets
class-map match-any system-cpp-police-forus
  description Forus Address resolution and Forus traffic
class-map match-any system-cpp-police-multicast-end-station
  description MCAST END STATION
class-map match-any system-cpp-police-high-rate-app
  description High Rate Applications 
class-map match-any system-cpp-police-multicast
  description Transit Traffic and MCAST Data
class-map match-any system-cpp-police-l2-control
  description L2 control
class-map match-any system-cpp-police-dot1x-auth
  description DOT1X Auth
class-map match-any system-cpp-police-data
  description ICMP redirect, ICMP_GEN and BROADCAST
class-map type access-control match-all cos3
class-map type access-control match-all cos2
class-map match-any system-cpp-police-stackwise-virt-control
  description Stackwise Virtual
class-map match-any system-cpp-police-control-low-priority
  description General punt
class-map match-any non-client-nrt-class
class-map match-any system-cpp-police-routing-control
  description Routing control and Low Latency
class-map match-any system-cpp-police-protocol-snooping
  description Protocol snooping
class-map match-any system-cpp-police-dhcp-snooping
  description DHCP snooping
class-map match-any system-cpp-police-system-critical
  description System Critical and Gold Pkt
!
policy-map system-cpp-policy
 class system-cpp-police-control-low-priority
policy-map type access-control POLICY_35MBPS
!
! 
!
!
!
!
!
!
!
!
!
interface Port-channel1
 switchport trunk native vlan 2
 switchport mode trunk
!
interface Port-channel3
 description Netbackup_2-2
 switchport access vlan 802
!
interface Port-channel10
 description Port-CH TunnelDRP
 switchport access vlan 403
!
interface Port-channel14
 switchport trunk native vlan 4
 switchport mode trunk
!
interface Port-channel16
 switchport trunk native vlan 4
 switchport mode trunk
!
interface Port-channel20
 switchport trunk native vlan 802
 switchport trunk allowed vlan 802
 switchport mode trunk
!
interface Port-channel21
 switchport trunk native vlan 802
 switchport mode trunk
 speed 1000
 duplex full
!
interface Tunnel403
 bandwidth 5120
 ip address 172.31.254.1 255.255.255.0
 tunnel source 172.27.189.129
 tunnel destination 172.27.157.2
!
interface GigabitEthernet0/0
 vrf forwarding Mgmt-vrf
 no ip address
 speed 1000
 negotiation auto
!
interface TenGigabitEthernet1/0/1
 description Link to ACCESS-SWITCH001
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/2
 description Link to ACCESS-SWITCH011 Gi5/0/52
 switchport trunk native vlan 2
 switchport mode trunk
 channel-group 1 mode active
!
interface TenGigabitEthernet1/0/3
 description Link to ACCESS-SWITCH031 Gi
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/4
 description Link to ACCESS-SWITCH051 Gi
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/5
 description Link to ACCESS-SWITCH071 Gi
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/6
 description Link to ACCESS-SWITCH081 Gi
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/7
 description Link to ACCESS-SWITCH101 Gi
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/8
 description Link to ACCESS-SWITCH151 Gi
 switchport trunk native vlan 4
 switchport mode trunk
!
interface TenGigabitEthernet1/0/9
 switchport trunk native vlan 4
 switchport mode trunk
 channel-group 16 mode active
!
interface TenGigabitEthernet1/0/10
 description M075-MediaSerBackup_1
 switchport access vlan 1102
!
interface TenGigabitEthernet1/0/11
 description ESX03
 switchport trunk allowed vlan 3,101,102,401,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet1/0/12
 description Nva_Consola_Respaldos
 switchport access vlan 1102
!
interface TenGigabitEthernet1/0/13
 description ESX4
 switchport trunk allowed vlan 3,101,102,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet1/0/14
 description Link to ACCESS-SWITCH931 Te1/1/8
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/0/15
 stackwise-virtual link 1
 description -Stack Virtual-
 !
 interface TenGigabitEthernet1/0/16
 stackwise-virtual link 1
 description -Stack Virtual-
 !
 interface TenGigabitEthernet1/1/1
 description Netbackup_2-2
 switchport access vlan 802
 channel-group 3 mode active
!
interface TenGigabitEthernet1/1/2
 description CONSOLA DE RESPALDOS LRS
 switchport access vlan 1102
!
interface TenGigabitEthernet1/1/3
 description ESX5
 switchport access vlan 802
 switchport trunk allowed vlan 3,101,102,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet1/1/4
 description MX-LRS1-M050
 switchport access vlan 802
!
interface TenGigabitEthernet1/1/5
 description NetBackup_1-1
 switchport access vlan 802
!
interface TenGigabitEthernet1/1/6
 description Link to ACCESS-SWITCH111
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet1/1/7
!
interface TenGigabitEthernet1/1/8
!
interface FortyGigabitEthernet1/1/1
!
interface FortyGigabitEthernet1/1/2
!
interface TenGigabitEthernet2/0/1
 description Link to ACCESS-SWITCH021
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/2
 description Link to ACCESS-SWITCH011
 switchport trunk native vlan 2
 switchport mode trunk
 shutdown
 channel-group 1 mode active
!
interface TenGigabitEthernet2/0/3
 description Link to ACCESS-SWITCH041
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/4
 description Link to ACCESS-SWITCH061
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/5
 description Link to ACCESS-SWITCH081
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/6
 description Link to ACCESS-SWITCH101
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/7
 description Link to ACCESS-SWITCH141
 switchport trunk native vlan 4
 switchport mode trunk
 channel-group 14 mode active
!
interface TenGigabitEthernet2/0/8
 description PowerConnect_1
 switchport trunk native vlan 3
 switchport trunk allowed vlan 3,401,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet2/0/9
 switchport trunk native vlan 4
 switchport mode trunk
 channel-group 16 mode active
!
interface TenGigabitEthernet2/0/10
 description M075-MediaSerBackup_2
 switchport access vlan 1102
!
interface TenGigabitEthernet2/0/11
 description ESX03
 switchport access vlan 1102
 switchport trunk allowed vlan 3,101,102,401,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet2/0/12
 description Tunnel DRP_2
 switchport access vlan 403
!
interface TenGigabitEthernet2/0/13
 description ESX4_02
 switchport trunk allowed vlan 3,101,102,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet2/0/14
 description Link to ACCESS-SWITCH931 Te2/1/8
 switchport trunk native vlan 3
 switchport mode trunk
!
interface TenGigabitEthernet2/0/15
 stackwise-virtual link 1
 !
 interface TenGigabitEthernet2/0/16
 stackwise-virtual link 1
 !
 interface TenGigabitEthernet2/1/1
 description CONSOLA DE RESPALDOS LRS
 switchport access vlan 1102
!
interface TenGigabitEthernet2/1/2
 switchport access vlan 802
 channel-group 3 mode active
!
interface TenGigabitEthernet2/1/3
 description ESX5_02
 switchport access vlan 102
 switchport trunk allowed vlan 3,101,102,406-408,411,801,802,1102
 switchport mode trunk
!
interface TenGigabitEthernet2/1/4
 description MX-LRS1-M050
 switchport access vlan 802
!
interface TenGigabitEthernet2/1/5
 description NetBackup_1-2
 switchport access vlan 411
!
interface TenGigabitEthernet2/1/6
 description Link to ACCESS-SWITCH141
 switchport trunk native vlan 4
 channel-group 14 mode active
!
interface TenGigabitEthernet2/1/7
!
interface TenGigabitEthernet2/1/8
!
interface FortyGigabitEthernet2/1/1
!
interface FortyGigabitEthernet2/1/2
!
interface Vlan1
 no ip address
!
interface Vlan2
 ip address 172.27.172.1 255.255.255.192
 no ip redirects
 shutdown
!
interface Vlan3
 ip address 172.27.188.129 255.255.255.192
 ip helper-address 172.27.188.20
 no ip redirects
!
interface Vlan4
 ip address 172.27.188.193 255.255.255.192
 ip helper-address 172.27.188.20
 no ip redirects
!
interface Vlan77
 no ip address
!
interface Vlan80
 no ip address
!
interface Vlan101
 ip address 172.27.186.1 255.255.254.0
 ip helper-address 172.27.188.20
 no ip redirects
!
interface Vlan102
 ip address 172.27.128.1 255.255.252.0
 ip helper-address 172.27.188.20
 no ip redirects
!
interface Vlan103
 ip address 172.27.189.1 255.255.255.192
 no ip redirects
!
interface Vlan301
 ip address 172.27.190.1 255.255.255.0
 no ip redirects
 ip access-group VOZ_IN in
 ip access-group VOZ_OUT out
!
interface Vlan401
 ip address 172.27.185.1 255.255.255.192
 no ip redirects
!
interface Vlan403
 ip address 172.27.184.129 255.255.255.128
 no ip redirects
!
interface Vlan405
 no ip address
 no ip redirects
!
interface Vlan406
 ip address 172.27.134.1 255.255.255.0
 no ip redirects
!
interface Vlan411
 ip address 172.27.181.193 255.255.255.224
 no ip redirects
!
interface Vlan412
 ip address 172.27.181.225 255.255.255.224
 ip helper-address 172.27.188.20
 no ip redirects
!
interface Vlan413
 ip address 172.27.189.65 255.255.255.192
!
interface Vlan777
 ip address 10.10.10.129 255.255.255.0
!
interface Vlan801
 ip address 172.27.188.1 255.255.255.192
 no ip redirects
 shutdown
!
interface Vlan802
 ip address 172.27.188.65 255.255.255.192
 no ip redirects
!
interface Vlan803
 ip address 172.27.172.65 255.255.255.224
 shutdown
!
interface Vlan901
 description Transit_To_Core
 ip address 172.27.189.129 255.255.255.192
 no ip redirects
 ip ospf 1 area 0.0.0.0
!
interface Vlan1102
 ip address 172.27.188.1 255.255.255.192
!
interface Vlan1103
 no ip address
!
router ospf 1
 router-id 172.27.189.129
!
ip forward-protocol nd
ip http server
ip http authentication local
ip http secure-server
ip route 0.0.0.0 0.0.0.0 172.27.189.134
ip route 5.39.123.4 255.255.255.255 172.27.189.139
ip route 5.39.123.20 255.255.255.255 172.27.189.139
ip route 10.203.0.75 255.255.255.255 172.27.185.2
ip route 10.203.27.223 255.255.255.255 172.27.185.2
ip route 10.203.27.224 255.255.255.255 172.27.185.2
ip route 10.203.28.120 255.255.255.255 172.27.185.2
ip route 10.203.28.121 255.255.255.255 172.27.185.2
ip route 10.203.28.185 255.255.255.255 172.27.185.2
ip route 10.203.28.186 255.255.255.255 172.27.185.2
ip route 10.203.28.187 255.255.255.255 172.27.185.2
ip route 10.203.28.188 255.255.255.255 172.27.185.2
ip route 10.203.28.189 255.255.255.255 172.27.185.2
ip route 10.203.28.190 255.255.255.255 172.27.185.2
ip route 10.203.28.191 255.255.255.255 172.27.185.2
ip route 10.203.28.192 255.255.255.255 172.27.185.2
ip route 10.203.28.193 255.255.255.255 172.27.185.2
ip route 10.203.28.194 255.255.255.255 172.27.185.2
ip route 10.204.62.253 255.255.255.255 172.27.185.2
ip route 10.208.220.23 255.255.255.255 172.27.185.2
ip route 10.208.220.59 255.255.255.255 172.27.185.2
ip route 10.208.220.200 255.255.255.255 172.27.185.2
ip route 10.208.220.201 255.255.255.255 172.27.185.2
ip route 10.208.220.202 255.255.255.255 172.27.185.2
ip route 10.208.220.203 255.255.255.255 172.27.185.2
ip route 10.208.221.139 255.255.255.255 172.27.185.2
ip route 10.208.221.140 255.255.255.255 172.27.185.2
ip route 10.208.221.141 255.255.255.255 172.27.185.2
ip route 10.208.221.142 255.255.255.255 172.27.185.2
ip route 10.208.221.143 255.255.255.255 172.27.185.2
ip route 10.208.221.144 255.255.255.255 172.27.185.2
ip route 10.208.221.145 255.255.255.255 172.27.185.2
ip route 10.208.222.26 255.255.255.255 172.27.185.2
ip route 10.208.222.27 255.255.255.255 172.27.185.2
ip route 10.208.222.42 255.255.255.255 172.27.185.2
ip route 10.208.222.43 255.255.255.255 172.27.185.2
ip route 10.208.222.44 255.255.255.255 172.27.185.2
ip route 10.208.222.45 255.255.255.255 172.27.185.2
ip route 10.216.122.0 255.255.255.0 172.27.185.2
ip route 13.65.246.180 255.255.255.255 172.27.189.139
ip route 13.107.6.152 255.255.255.254 172.27.189.139
ip route 13.107.18.10 255.255.255.254 172.27.189.139
ip route 13.107.128.0 255.255.252.0 172.27.189.139
ip route 13.107.136.0 255.255.252.0 172.27.189.139
ip route 23.103.160.0 255.255.240.0 172.27.189.139
ip route 40.96.0.0 255.248.0.0 172.27.189.139
ip route 40.104.0.0 255.254.0.0 172.27.189.139
ip route 40.108.128.0 255.255.128.0 172.27.189.139
ip route 40.126.0.0 255.254.0.0 172.27.189.139
ip route 46.163.100.199 255.255.255.255 172.27.189.139
ip route 50.16.174.234 255.255.255.255 172.27.189.139
ip route 50.198.142.120 255.255.255.255 172.27.189.139
ip route 52.51.248.160 255.255.255.255 172.27.189.139
ip route 52.96.0.0 255.252.0.0 172.27.189.139
ip route 52.104.0.0 255.252.0.0 172.27.189.139
ip route 52.210.38.190 255.255.255.255 172.27.189.139
ip route 78.40.77.0 255.255.255.0 172.27.189.139
ip route 78.134.106.154 255.255.255.255 172.27.189.139
ip route 78.134.106.157 255.255.255.255 172.27.189.139
ip route 79.136.9.30 255.255.255.255 172.27.189.139
ip route 80.91.62.0 255.255.255.0 172.27.189.139
ip route 80.146.172.224 255.255.255.224 172.27.189.139
ip route 80.154.27.217 255.255.255.255 172.27.189.139
ip route 85.17.153.135 255.255.255.255 172.27.189.139
ip route 85.37.200.240 255.255.255.255 172.27.189.139
ip route 87.241.18.131 255.255.255.255 172.27.189.139
ip route 88.149.216.170 255.255.255.255 172.27.189.139
ip route 88.198.198.10 255.255.255.255 172.27.189.139
ip route 88.198.198.11 255.255.255.255 172.27.189.139
ip route 94.254.44.179 255.255.255.255 172.27.189.139
ip route 94.254.44.181 255.255.255.255 172.27.189.139
ip route 104.146.128.0 255.255.128.0 172.27.189.139
ip route 131.253.33.215 255.255.255.255 172.27.189.139
ip route 132.245.0.0 255.255.0.0 172.27.189.139
ip route 150.171.32.0 255.255.252.0 172.27.189.139
ip route 150.171.40.0 255.255.252.0 172.27.189.139
ip route 159.63.108.74 255.255.255.255 172.27.189.139
ip route 169.57.28.250 255.255.255.255 172.27.189.139
ip route 169.57.62.200 255.255.255.255 172.27.189.139
ip route 172.16.0.154 255.255.255.255 172.27.189.139
ip route 172.20.197.0 255.255.255.0 172.27.189.139
ip route 172.27.135.0 255.255.255.0 172.27.189.138 120
ip route 172.27.136.0 255.255.255.0 172.27.189.138 120
ip route 172.27.137.0 255.255.255.0 172.27.189.138 120
ip route 172.27.138.0 255.255.255.0 172.27.189.138 120
ip route 172.27.139.0 255.255.255.0 172.27.189.138 120
ip route 172.27.140.0 255.255.255.0 172.27.189.138 120
ip route 172.27.141.0 255.255.255.0 172.27.189.138 120
ip route 172.27.142.0 255.255.255.0 172.27.189.138 120
ip route 172.27.143.0 255.255.255.0 172.27.189.138 120
ip route 172.27.144.0 255.255.255.0 172.27.189.137 120
ip route 172.27.145.0 255.255.255.0 172.27.189.138 120
ip route 172.27.146.0 255.255.255.0 172.27.189.138 120
ip route 172.27.147.0 255.255.255.0 172.27.189.138 120
ip route 172.27.148.0 255.255.255.0 172.27.189.138 120
ip route 172.27.149.0 255.255.255.0 172.27.189.138 120
ip route 172.27.150.0 255.255.255.0 172.27.189.138 120
ip route 172.27.151.0 255.255.255.0 172.27.189.137 120
ip route 172.27.152.0 255.255.255.0 172.27.189.137 120
ip route 172.27.153.0 255.255.255.0 172.27.189.138 120
ip route 172.27.154.0 255.255.255.0 172.27.189.138 120
ip route 172.27.155.0 255.255.255.0 172.27.189.138 120
ip route 172.27.156.0 255.255.255.0 172.27.189.138 120
ip route 172.27.157.0 255.255.255.0 172.27.189.137 120
ip route 172.27.158.0 255.255.255.0 172.27.189.137 120
ip route 172.27.159.0 255.255.255.0 172.27.189.137 120
ip route 172.27.160.0 255.255.255.0 172.27.189.137 120
ip route 172.27.161.0 255.255.255.0 172.27.189.137 120
ip route 172.27.162.0 255.255.255.0 172.27.189.137 120
ip route 172.27.163.0 255.255.255.240 172.27.189.137 120
ip route 172.27.163.16 255.255.255.240 172.27.189.138 120
ip route 172.27.163.32 255.255.255.240 172.27.189.138 120
ip route 172.27.163.48 255.255.255.240 172.27.189.137 120
ip route 172.27.163.64 255.255.255.240 172.27.189.137 120
ip route 172.27.163.80 255.255.255.240 172.27.189.138 120
ip route 172.27.163.96 255.255.255.240 172.27.189.138 120
ip route 172.27.163.112 255.255.255.240 172.27.189.138 120
ip route 172.27.163.128 255.255.255.240 172.27.189.138 120
ip route 172.27.163.144 255.255.255.240 172.27.189.138 120
ip route 172.27.163.160 255.255.255.240 172.27.189.137 120
ip route 172.27.163.176 255.255.255.240 172.27.189.138 120
ip route 172.27.163.192 255.255.255.240 172.27.189.138 120
ip route 172.27.163.208 255.255.255.240 172.27.189.138 120
ip route 172.27.163.224 255.255.255.240 172.27.189.138 120
ip route 172.27.163.240 255.255.255.240 172.27.189.138 120
ip route 172.27.164.0 255.255.255.240 172.27.189.137 120
ip route 172.27.165.0 255.255.255.224 172.27.189.137 120
ip route 172.27.165.32 255.255.255.224 172.27.189.137 120
ip route 172.27.166.0 255.255.255.0 172.27.189.138 120
ip route 172.27.167.0 255.255.255.0 172.27.189.138 120
ip route 172.27.168.0 255.255.255.0 172.27.189.138 120
ip route 172.27.169.0 255.255.255.0 172.27.189.138 120
ip route 172.27.170.0 255.255.255.252 172.27.189.138 120
ip route 172.27.170.4 255.255.255.252 172.27.189.138 120
ip route 172.27.170.8 255.255.255.252 172.27.189.138 120
ip route 172.27.170.12 255.255.255.252 172.27.189.138 120
ip route 172.27.170.28 255.255.255.252 172.27.189.138 120
ip route 172.27.170.32 255.255.255.252 172.27.189.138 120
ip route 172.27.170.36 255.255.255.252 172.27.189.138 120
ip route 172.27.170.40 255.255.255.252 172.27.189.138 120
ip route 172.27.170.44 255.255.255.252 172.27.189.138 120
ip route 172.27.170.48 255.255.255.252 172.27.189.138 120
ip route 172.27.170.52 255.255.255.252 172.27.189.138 120
ip route 172.27.170.60 255.255.255.252 172.27.189.138 120
ip route 172.27.170.64 255.255.255.252 172.27.189.138 120
ip route 172.27.170.68 255.255.255.252 172.27.189.138 120
ip route 172.27.170.72 255.255.255.252 172.27.189.138 120
ip route 172.27.170.76 255.255.255.252 172.27.189.138 120
ip route 172.27.170.84 255.255.255.252 172.27.189.138 120
ip route 172.27.170.92 255.255.255.252 172.27.189.138 120
ip route 172.27.170.96 255.255.255.252 172.27.189.138 120
ip route 172.27.170.100 255.255.255.252 172.27.189.138 120
ip route 172.27.170.104 255.255.255.252 172.27.189.138 120
ip route 172.27.170.116 255.255.255.252 172.27.189.138 120
ip route 172.27.170.120 255.255.255.252 172.27.189.138 120
ip route 172.27.170.136 255.255.255.252 172.27.189.138 120
ip route 172.27.170.140 255.255.255.252 172.27.189.138 120
ip route 172.27.170.144 255.255.255.252 172.27.189.138 120
ip route 172.27.170.148 255.255.255.252 172.27.189.138 120
ip route 172.27.170.152 255.255.255.252 172.27.189.138 120
ip route 172.27.170.156 255.255.255.252 172.27.189.138 120
ip route 172.27.170.168 255.255.255.252 172.27.189.138 120
ip route 172.27.170.172 255.255.255.252 172.27.189.138 120
ip route 172.27.170.184 255.255.255.252 172.27.189.138 120
ip route 172.27.170.188 255.255.255.252 172.27.189.138 120
ip route 172.27.170.192 255.255.255.252 172.27.189.138 120
ip route 172.27.170.196 255.255.255.252 172.27.189.138 120
ip route 172.27.170.200 255.255.255.252 172.27.189.138 120
ip route 172.27.172.0 255.255.252.0 172.27.189.137 120
ip route 172.27.177.0 255.255.255.0 172.27.189.137 120
ip route 172.27.178.0 255.255.255.0 172.27.189.137 120
ip route 172.27.179.0 255.255.255.128 172.27.189.137 120
ip route 172.27.179.128 255.255.255.128 172.27.189.137 120
ip route 172.27.180.0 255.255.255.128 172.27.189.137 120
ip route 172.27.180.128 255.255.255.224 172.27.189.137 120
ip route 172.27.180.160 255.255.255.224 172.27.189.137 120
ip route 172.27.184.24 255.255.255.248 172.27.185.2
ip route 173.230.135.175 255.255.255.255 172.27.189.139
ip route 184.68.145.182 255.255.255.255 172.27.189.139
ip route 184.72.42.7 255.255.255.255 172.27.189.139
ip route 184.72.220.33 255.255.255.255 172.27.189.139
ip route 187.189.18.157 255.255.255.255 172.27.189.139
ip route 191.234.140.0 255.255.252.0 172.27.189.139
ip route 192.145.232.32 255.255.255.255 172.27.189.139
ip route 192.145.232.36 255.255.255.252 172.27.189.139
ip route 192.145.232.38 255.255.255.255 172.27.189.139
ip route 194.39.131.178 255.255.255.255 172.27.189.139
ip route 194.117.106.129 255.255.255.255 172.27.189.139
ip route 195.145.232.0 255.255.255.192 172.27.189.139
ip route 195.145.232.32 255.255.255.255 172.27.189.139
ip route 195.145.232.36 255.255.255.252 172.27.189.139
ip route 198.50.162.21 255.255.255.255 172.27.189.139
ip route 200.53.1.48 255.255.255.255 172.27.189.139
ip route 200.67.139.251 255.255.255.255 172.27.189.139
ip route 200.76.5.147 255.255.255.255 172.27.189.134
ip route 200.76.152.227 255.255.255.255 172.27.189.139
ip route 200.76.152.240 255.255.255.255 172.27.189.139
ip route 200.76.152.243 255.255.255.255 172.27.189.139
ip route 200.188.11.157 255.255.255.255 172.27.189.139
ip route 201.149.255.24 255.255.255.255 172.27.189.139
ip route 201.149.255.57 255.255.255.255 172.27.189.139
ip route 201.151.239.195 255.255.255.255 172.27.189.134
ip route 204.79.197.215 255.255.255.255 172.27.189.139
ip route 206.18.172.0 255.255.255.0 172.27.189.139
ip route 206.51.26.33 255.255.255.255 172.27.189.134
ip route 207.44.167.210 255.255.255.255 172.27.189.139
ip route 210.173.216.40 255.255.255.255 172.27.189.139
ip route 212.49.145.8 255.255.255.255 172.27.189.139
ip route 212.49.145.11 255.255.255.255 172.27.189.134
ip route 212.49.145.39 255.255.255.255 172.27.189.139
ip route 212.49.145.70 255.255.255.255 172.27.189.139
ip route 212.63.73.203 255.255.255.255 172.27.189.139
ip route 212.185.180.160 255.255.255.240 172.27.189.139
ip route 217.58.138.0 255.255.255.255 172.27.189.139
!
!
!
ip access-list extended ACL_35MBPS
 permit ip host 172.27.188.86 host 172.27.157.47
ip access-list extended Guest
 permit tcp 172.27.181.224 0.0.0.31 any eq www
 permit tcp 172.27.181.224 0.0.0.31 any eq 443
 permit tcp 172.27.181.224 0.0.0.31 any eq pop2
 permit tcp 172.27.181.224 0.0.0.31 any eq pop3
 permit tcp 172.27.181.224 0.0.0.31 any eq smtp
 permit tcp 172.27.181.224 0.0.0.31 any eq echo
ip access-list extended SNMP
 permit ip host 172.27.188.13 any
 permit ip any host 172.27.188.13
 permit ip host 172.27.188.34 any
 permit ip any host 172.27.188.34
ip access-list extended UCS_ControlAccess
 permit tcp 172.27.0.0 0.0.255.255 host 172.27.188.220
ip access-list extended VOZ_IN
 permit ip 172.27.190.0 0.0.0.255 any log
 permit ip any any
 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group VLANsVoice
 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group ServersVoice
 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group VlansIpComunicator
 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group VLANsVoice
 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group ServersVoice
 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group VlansIpComunicator
ip access-list extended VOZ_OUT
 permit object-group TCP_Voz object-group VLANsVoice 172.27.190.0 0.0.0.255
 permit object-group TCP_Voz object-group ServersVoice 172.27.190.0 0.0.0.255
 permit object-group TCP_Voz object-group VlansIpComunicator 172.27.190.0 0.0.0.255
 permit object-group UDP_Voz object-group VLANsVoice 172.27.190.0 0.0.0.255
 permit object-group UDP_Voz object-group ServersVoice 172.27.190.0 0.0.0.255
 permit object-group UDP_Voz object-group VlansIpComunicator 172.27.190.0 0.0.0.255
 permit ip any 172.27.190.0 0.0.0.255 log
ip access-list extended cos2
 permit udp host 172.27.188.13 eq snmp any
 permit udp any eq ntp any
 permit udp any eq time any
 permit tcp any eq 37 any
 permit udp host 172.27.188.15 eq domain any
 permit udp host 172.27.188.16 eq domain any
 permit tcp host 172.27.188.15 eq domain any
 permit tcp host 172.27.188.16 eq domain any
 permit tcp host 172.27.188.16 eq 67 any
 permit tcp host 172.27.188.16 eq 68 any
 permit udp host 172.27.188.16 eq bootpc any
 permit udp host 172.27.188.16 eq bootps any
 permit udp host 172.27.188.16 eq 445 any
 permit udp host 172.27.188.15 eq 445 any
 permit tcp host 172.27.188.15 eq 445 any
 permit tcp host 172.27.188.16 eq 445 any
 permit tcp any eq 389 any
 permit udp any eq 389 any
 permit udp any eq nameserver any
 permit tcp host 172.27.188.19 eq 81 any
 permit udp host 172.27.188.19 eq 81 any
 permit udp host 172.27.188.20 eq 3101 any
 permit tcp host 172.27.188.20 eq 3101 any
 permit tcp any eq 3101 any
 permit udp any eq 3101 any
 permit udp host 172.27.188.17 eq 135 any
 permit udp host 172.27.188.17 eq 136 any
 permit udp host 172.27.188.17 eq netbios-ns any
 permit udp host 172.27.188.17 eq netbios-dgm any
 permit udp host 172.27.188.17 eq netbios-ss any
 permit tcp host 172.27.188.17 eq msrpc any
 permit tcp host 172.27.188.17 eq 136 any
 permit tcp host 172.27.188.17 eq 137 any
 permit tcp host 172.27.188.17 eq 138 any
 permit tcp host 172.27.188.17 eq 139 any
 permit tcp any eq www any
 permit tcp any eq 443 any
 permit tcp any eq 8080 any
 permit tcp any eq 8088 any
 permit tcp host 172.27.188.21 range 1500 1520 any
 permit tcp host 172.27.188.22 range 1500 1520 any
 permit udp host 172.27.188.22 range 1500 1520 any
 permit udp host 172.27.188.21 range 1500 1520 any
 permit udp host 172.27.188.21 eq 1580 any
 permit tcp host 172.27.188.21 eq 1580 any
 permit tcp host 172.27.188.22 eq 1580 any
 permit udp host 172.27.188.22 eq 1580 any
ip access-list extended cos3
 permit tcp host 172.27.188.22 eq smtp any
 permit tcp host 172.27.188.22 eq pop3 any
 permit udp host 172.27.188.22 eq 110 any
 permit udp host 172.27.188.22 eq 25 any
 permit udp host 172.27.188.24 eq 21 any
 permit udp host 172.27.188.24 eq 80 any
 permit tcp host 172.27.188.24 eq www any
 permit tcp host 172.27.188.24 eq ftp any
 permit tcp host 172.27.188.23 eq www any
 permit udp host 172.27.188.23 eq 80 any
 permit udp host 172.27.188.23 eq 443 any
 permit tcp host 172.27.188.23 eq 443 any
 permit tcp host 172.27.188.17 eq 1433 any
 permit udp host 172.27.188.17 eq 1433 any
 permit udp host 172.27.188.17 eq 1434 any
 permit tcp host 172.27.188.17 eq 1434 any
 permit tcp any range 1128 1129 any
 permit udp any range 1128 1129 any
 permit tcp any range 1520 1529 any
 permit udp any range 1520 1529 any
 permit tcp any range 3200 3299 any
 permit udp any range 3200 3299 any
 permit tcp any range 3300 3399 any
 permit udp any range 3300 3399 any
 permit tcp any range 3600 3699 any
 permit udp any range 3600 3699 any
 permit tcp any range 3900 3999 any
 permit udp any range 3900 3999 any
 permit udp any range 4238 4241 any
 permit tcp any range 4238 4241 any
 permit tcp any range 4700 4799 any
 permit tcp any range 4800 4899 any
 permit udp any range 4700 4799 any
 permit udp any range 4800 4899 any
 permit udp any range 8000 8099 any
 permit tcp any range 8000 8099 any
 permit tcp any range 21212 21213 any
 permit udp any range 21212 21213 any
 permit udp any range 44300 44399 any
 permit tcp any range 44300 44399 any
 permit tcp any range 44400 44499 any
 permit udp any range 44400 44499 any
 permit udp any range 50000 50099 any
 permit tcp any range 50000 50099 any
 permit tcp any eq smtp any
 permit udp any eq 25 any
 permit udp any eq 515 any
 permit tcp any eq lpd any
ip access-list extended vty
 permit ip 172.27.186.0 0.0.1.255 any log
 permit ip 172.27.174.1 0.0.0.254 any
 permit ip 172.27.136.9 0.0.0.2 any
 permit ip 172.27.188.128 0.0.0.63 any log
 permit ip host 172.27.188.45 any log
 permit ip 172.27.131.0 0.0.0.255 any log
 permit ip host 172.27.188.34 any log
 permit ip host 172.27.166.52 any log
!
route-map Guest permit 10 
 match ip address Guest
 set ip next-hop 172.27.181.227
!
snmp-server group ReadOnlyLAN v3 auth read Read access snmp-list
snmp-server view Read iso included
snmp-server community skm-rotelecomIT RO 10
snmp-server community SMURFITKAPPARO RO
snmp-server enable traps snmp authentication linkdown linkup coldstart warmstart
snmp-server enable traps call-home message-send-fail server-fail
snmp-server enable traps tty
snmp-server enable traps vtp
snmp-server enable traps vlancreate
snmp-server enable traps vlandelete
snmp-server enable traps port-security
snmp-server enable traps license
snmp-server enable traps cpu threshold
snmp-server enable traps stackwise
snmp-server enable traps fru-ctrl
snmp-server enable traps flash insertion
snmp-server enable traps flash removal
snmp-server enable traps energywise
snmp-server enable traps power-ethernet police
snmp-server enable traps entity
snmp-server enable traps envmon fan shutdown supply temperature status
snmp-server enable traps ipsla
snmp-server enable traps config-copy
snmp-server enable traps config
snmp-server enable traps config-ctid
snmp-server enable traps event-manager
snmp-server enable traps bridge newroot topologychange
snmp-server enable traps stpx inconsistency root-inconsistency loop-inconsistency
snmp-server enable traps syslog
snmp-server enable traps auth-framework sec-violation
snmp-server enable traps ike policy add
snmp-server enable traps ike policy delete
snmp-server enable traps ike tunnel start
snmp-server enable traps ike tunnel stop
snmp-server enable traps ipsec cryptomap add
snmp-server enable traps ipsec cryptomap delete
snmp-server enable traps ipsec cryptomap attach
snmp-server enable traps ipsec cryptomap detach
snmp-server enable traps ipsec tunnel start
snmp-server enable traps ipsec tunnel stop
snmp-server enable traps ipsec too-many-sas
snmp-server enable traps vlan-membership
snmp-server enable traps errdisable
snmp-server enable traps transceiver all
snmp-server enable traps bulkstat collection transfer
snmp-server enable traps mac-notification change move threshold
snmp-server host 172.27.158.17 version 2c ise_comunity 
snmp-server host 172.27.188.194 version 2c ise_comunity 
!
!
control-plane
 service-policy input system-cpp-policy
!
banner motd ^C
+-------------------------------------------------------------------------------------------------------------------+
|                                                                                                                   |
|  UNAUTHORIZED ACCESS TO THIS NETWORK DEVICE IS PROHIBITED. You must have explicit permission to access            |
|  or  configure this device. All activities performed on this device maybe logged, and violations of this          |
|  policy may result in disciplinary action, and may be reported to law enforcement. There is no right to privacy   |
|  on this device.                                                                                                  |
|                                                                                                                   |
|  ACCESO NO AUTORIZADO A ESTE EQUIPO ESTA PROHIBIDO. Usted debe estar autorizado para accesar a configurar         |
|  este equipo. Todas las actividades realizadas en este equipo seran monitoreadas y las violaciones a estas        |
|  politicas seran causantes de acciones disciplinarias y reportadas a la autoridad. No hay derecho de              |
|  privacidad en este equipo.                                                                                       |
|                                                                                                                   |
+-------------------------------------------------------------------------------------------------------------------+
^C
!
line con 0
 exec-timeout 5 0
 login local
 stopbits 1
line vty 0 4
 exec-timeout 15 0
 logging synchronous
 login local
 transport input ssh
line vty 5 15
 exec-timeout 15 0
 logging synchronous
 login local
 transport input ssh
!
ntp server 172.27.188.1 prefer
ntp server 172.27.157.2
!
!
!
!
!
!
end

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh ip int brief
Interface              IP-Address      OK? Method Status                Protocol
Vlan1                  unassigned      YES NVRAM  up                    up      
Vlan2                  172.27.172.1    YES NVRAM  administratively down down    
Vlan3                  172.27.188.129  YES NVRAM  up                    up      
Vlan4                  172.27.188.193  YES NVRAM  up                    up      
Vlan77                 unassigned      YES unset  down                  down    
Vlan80                 unassigned      YES unset  down                  down    
Vlan101                172.27.186.1    YES NVRAM  up                    up      
Vlan102                172.27.128.1    YES manual up                    up      
Vlan103                172.27.189.1    YES NVRAM  up                    up      
Vlan301                172.27.190.1    YES NVRAM  up                    up      
Vlan401                172.27.185.1    YES NVRAM  up                    up      
Vlan403                172.27.184.129  YES NVRAM  up                    up      
Vlan405                unassigned      YES unset  up                    up      
Vlan406                172.27.134.1    YES NVRAM  up                    up      
Vlan411                172.27.181.193  YES NVRAM  up                    up      
Vlan412                172.27.181.225  YES NVRAM  up                    up      
Vlan413                172.27.189.65   YES NVRAM  up                    up      
Vlan777                10.10.10.129    YES NVRAM  up                    up      
Vlan801                172.27.188.1    YES manual administratively down down    
Vlan802                172.27.188.65   YES NVRAM  up                    up      
Vlan803                172.27.172.65   YES NVRAM  administratively down down    
Vlan901                172.27.189.129  YES NVRAM  up                    up      
Vlan1102               172.27.188.1    YES manual up                    up      
Vlan1103               unassigned      YES unset  down                  down    
GigabitEthernet0/0     unassigned      YES NVRAM  down                  down    
Te1/0/1                unassigned      YES unset  up                    up      
Te1/0/2                unassigned      YES unset  up                    up      
Te1/0/3                unassigned      YES unset  up                    up      
Te1/0/4                unassigned      YES unset  up                    up      
Te1/0/5                unassigned      YES unset  up                    up      
Te1/0/6                unassigned      YES unset  up                    up      
Te1/0/7                unassigned      YES unset  up                    up      
Te1/0/8                unassigned      YES unset  up                    up      
Te1/0/9                unassigned      YES unset  up                    up      
Te1/0/10               unassigned      YES unset  up                    up      
Te1/0/11               unassigned      YES unset  down                  down    
Te1/0/12               unassigned      YES unset  up                    up      
Te1/0/13               unassigned      YES unset  up                    up      
Te1/0/14               unassigned      YES unset  up                    up      
Te1/0/15               unassigned      YES unset  up                    up      
Te1/0/16               unassigned      YES unset  up                    up      
Te1/1/1                unassigned      YES unset  up                    up      
Te1/1/2                unassigned      YES unset  up                    up      
Te1/1/3                unassigned      YES unset  up                    up      
Te1/1/4                unassigned      YES unset  down                  down    
Te1/1/5                unassigned      YES unset  up                    up      
Te1/1/6                unassigned      YES unset  up                    up      
Te1/1/7                unassigned      YES unset  down                  down    
Te1/1/8                unassigned      YES unset  down                  down    
Fo1/1/1                unassigned      YES unset  down                  down    
Fo1/1/2                unassigned      YES unset  down                  down    
Te2/0/1                unassigned      YES unset  up                    up      
Te2/0/2                unassigned      YES unset  administratively down down    
Te2/0/3                unassigned      YES unset  up                    up      
Te2/0/4                unassigned      YES unset  up                    up      
Te2/0/5                unassigned      YES unset  up                    up      
Te2/0/6                unassigned      YES unset  up                    up      
Te2/0/7                unassigned      YES unset  up                    up      
Te2/0/8                unassigned      YES unset  up                    up      
Te2/0/9                unassigned      YES unset  up                    up      
Te2/0/10               unassigned      YES unset  up                    up      
Te2/0/11               unassigned      YES unset  up                    up      
Te2/0/12               unassigned      YES unset  down                  down    
Te2/0/13               unassigned      YES unset  up                    up      
Te2/0/14               unassigned      YES unset  up                    up      
Te2/0/15               unassigned      YES unset  up                    up      
Te2/0/16               unassigned      YES unset  up                    up      
Te2/1/1                unassigned      YES unset  up                    up      
Te2/1/2                unassigned      YES unset  up                    up      
Te2/1/3                unassigned      YES unset  up                    up      
Te2/1/4                unassigned      YES unset  down                  down    
Te2/1/5                unassigned      YES unset  up                    up      
Te2/1/6                unassigned      YES unset  up                    up      
Te2/1/7                unassigned      YES unset  down                  down    
Te2/1/8                unassigned      YES unset  down                  down    
Fo2/1/1                unassigned      YES unset  down                  down    
Fo2/1/2                unassigned      YES unset  down                  down    
Port-channel1          unassigned      YES unset  up                    up      
Port-channel3          unassigned      YES unset  up                    up      
Port-channel10         unassigned      YES unset  down                  down    
Port-channel14         unassigned      YES unset  up                    up      
Port-channel16         unassigned      YES unset  up                    up      
Port-channel20         unassigned      YES unset  down                  down    
Port-channel21         unassigned      YES unset  down                  down    
Tunnel403              172.31.254.1    YES NVRAM  up                    up      
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh cdp nei
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone, 
                  D - Remote, C - CVTA, M - Two-port Mac Relay 

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
MX-LRS1-SW-MDF-ESXMNMGT1
                 Ten 2/0/8         159               R    PCT7024   Gi1/0/24
MX-LRS1-S931.group.wan.group.wan
                 Ten 2/0/14        157              S I   C9300-24P Ten 2/1/8
MX-LRS1-S931.group.wan.group.wan
                 Ten 1/0/14        167              S I   C9300-24P Ten 1/1/8
MX-LRS1-S091.group.wan.group.wan
                 Ten 2/0/5         129              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S081.group.wan.group.wan
                 Ten 1/0/6         140              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S031.group.wan.group.wan
                 Ten 1/0/3         176              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S121.group.wan.group.wan
                 Ten 2/0/6         179              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S021.group.wan.group.wan
                 Ten 2/0/1         164              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S101.group.wan.group.wan
                 Ten 1/0/7         124              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S011.group.wan.group.wan
                 Ten 1/0/2         155              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S001.group.wan.group.wan
                 Ten 1/0/1         131              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S111.group.wan.group.wan
                 Ten 1/1/6         135              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S071.group.wan.group.wan
                 Ten 1/0/5         171              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S161.group.wan.group.wan
                 Ten 2/0/9         154              S I   WS-C2960X Gig 1/0/26
MX-LRS1-S161.group.wan.group.wan
                 Ten 1/0/9         178              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S061.group.wan.group.wan
                 Ten 2/0/4         158              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S141.group.wan.group.wan
                 Ten 2/1/6         171              S I   WS-C2960X Gig 2/0/52
MX-LRS1-S141.group.wan.group.wan
                 Ten 2/0/7         175              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S051.group.wan.group.wan
                 Ten 1/0/4         167              S I   WS-C2960X Gig 1/0/49
MX-LRS1-S151.group.wan.group.wan
                 Ten 1/0/8         149              S I   WS-C2960X Gig 1/0/25
MX-LRS1-S041.group.wan.group.wan
                 Ten 2/0/3         159              S I   WS-C2960X Gig 1/0/49

Total cdp entries displayed : 21
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh cdp nei det
-------------------------
Device ID: MX-LRS1-SW-MDF-ESXMNMGT1
Entry address(es): 
  IP address: 172.27.188.167
Platform: PCT7024,  Capabilities: Router 
Interface: TenGigabitEthernet2/0/8,  Port ID (outgoing port): Gi1/0/24
Holdtime : 159 sec

Version :
4.2.0.4

advertisement version: 2

-------------------------
Device ID: MX-LRS1-S931.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.130
Platform: cisco C9300-24P,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/14,  Port ID (outgoing port): TenGigabitEthernet2/1/8
Holdtime : 157 sec

Version :
Cisco IOS Software [Fuji], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 16.9.3, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Wed 20-Mar-19 08:02 by mcpre

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.130

-------------------------
Device ID: MX-LRS1-S931.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.130
Platform: cisco C9300-24P,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/14,  Port ID (outgoing port): TenGigabitEthernet1/1/8
Holdtime : 167 sec

Version :
Cisco IOS Software [Fuji], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 16.9.3, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Wed 20-Mar-19 08:02 by mcpre

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.130

-------------------------
Device ID: MX-LRS1-S091.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.146
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/5,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 129 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E127BDC80FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.146

-------------------------
Device ID: MX-LRS1-S081.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.145
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/6,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 140 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E1211BE80FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.145

-------------------------
Device ID: MX-LRS1-S031.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.139
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/3,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 176 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E12114380FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.139

-------------------------
Device ID: MX-LRS1-S121.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.173
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/6,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 179 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E12DB7600FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.173

-------------------------
Device ID: MX-LRS1-S021.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.138
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/1,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 164 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000501CB047CD80FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.138

-------------------------
Device ID: MX-LRS1-S101.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.147
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/7,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 124 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E12DB4F00FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.147

-------------------------
Device ID: MX-LRS1-S011.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.133
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/2,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 155 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF01022501000000000000B4A8B9DA7A80FF0000
VTP Management Domain: ''
Native VLAN: 2
Duplex: full
Management address(es): 
  IP address: 172.27.188.133

-------------------------
Device ID: MX-LRS1-S001.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.132
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/1,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 131 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E120F1480FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.132

-------------------------
Device ID: MX-LRS1-S111.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.148
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/1/6,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 135 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E12DB5A00FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.148

-------------------------
Device ID: MX-LRS1-S071.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.144
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/5,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 170 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E12DB3A00FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.144

-------------------------
Device ID: MX-LRS1-S161.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.215
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/9,  Port ID (outgoing port): GigabitEthernet1/0/26
Holdtime : 154 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF01022510000000000000247E12104100FF0000
VTP Management Domain: ''
Native VLAN: 4
Duplex: full
Management address(es): 
  IP address: 172.27.188.215

-------------------------
Device ID: MX-LRS1-S161.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.215
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/9,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 178 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF01022510000000000000247E12104100FF0000
VTP Management Domain: ''
Native VLAN: 4
Duplex: full
Management address(es): 
  IP address: 172.27.188.215

-------------------------
Device ID: MX-LRS1-S061.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.142
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/4,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 158 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E1211C600FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.142

-------------------------
Device ID: MX-LRS1-S141.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.203
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/1/6,  Port ID (outgoing port): GigabitEthernet2/0/52
Holdtime : 171 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF0102250E0000000000000041D24F8880FF0000
VTP Management Domain: ''
Native VLAN: 4
Duplex: full
Management address(es): 
  IP address: 172.27.188.203

-------------------------
Device ID: MX-LRS1-S141.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.203
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/7,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 175 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF0102250E0000000000000041D24F8880FF0000
VTP Management Domain: ''
Native VLAN: 4
Duplex: full
Management address(es): 
  IP address: 172.27.188.203

-------------------------
Device ID: MX-LRS1-S051.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.141
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/4,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 167 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF0000000000005006AB33A200FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.141

-------------------------
Device ID: MX-LRS1-S151.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.204
Platform: cisco WS-C2960X-24PS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet1/0/8,  Port ID (outgoing port): GigabitEthernet1/0/25
Holdtime : 149 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.0(2)EX3, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 11-Sep-13 02:04 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF00000000000000CCFC9EF080FF0000
VTP Management Domain: ''
Native VLAN: 4
Duplex: full
Management address(es): 
  IP address: 172.27.188.204

-------------------------
Device ID: MX-LRS1-S041.group.wan.group.wan
Entry address(es): 
  IP address: 172.27.188.140
Platform: cisco WS-C2960X-48FPS-L,  Capabilities: Switch IGMP 
Interface: TenGigabitEthernet2/0/3,  Port ID (outgoing port): GigabitEthernet1/0/49
Holdtime : 159 sec

Version :
Cisco IOS Software, C2960X Software (C2960X-UNIVERSALK9-M), Version 15.2(2)E7, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2017 by Cisco Systems, Inc.
Compiled Wed 12-Jul-17 13:06 by prod_rel_team

advertisement version: 2
Protocol Hello:  OUI=0x00000C, Protocol ID=0x0112; payload len=27, value=00000000FFFFFFFF010221FF000000000000247E1205A880FF0000
VTP Management Domain: ''
Native VLAN: 3
Duplex: full
Management address(es): 
  IP address: 172.27.188.140


Total cdp entries displayed : 21
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Te1/0/11, Te1/1/7, Te1/1/8, Te2/0/2, Te2/1/7, Te2/1/8
2    NetMgmt                          active    
3    NetMgmt_2                        active    
4    NetMgmt_3                        active    
60   DMZ                              active    
101  Users                            active    
102  Printers                         active    
103  Printers_02                      active    
301  VoIP                             active    
401  Special_01_Services              active    
402  Special_02_Kodak                 active    
403  Special_03_Tunneling             active    Te2/0/12
404  Special_04_Priv.DELL             active    
405  Special_05_Antigua               active    
406  Special_06_SAP-Antiguo           active    
407  Special_07_vControl              active    
408  Special_08_vPacket               active    
409  Special_09_Telcel-Fibras-L2      active    
410  Special_10_Priv.Respaldos        active    
411  Special_11_VMware-Backup         active    Te2/1/5
412  Special_12_Cisco_ISE_GUEST       active    
413  Special_13_CCTV                  active    
777  emergencia                       active    
801  Local_servers_01                 active    
802  Local_servers_02                 active    Te1/1/4, Te1/1/5, Te2/1/4, Po3
803  Local_Servers_03_HQP             active    
901  Transit_To_Core                  active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
1100 VLAN1100                         active    
1102 VLAN1102                         active    Te1/0/10, Te1/0/12, Te1/1/2, Te2/0/10, Te2/1/1
1203 VLAN1203                         active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0   
2    enet  100002     1500  -      -      -        -    -        0      0   
3    enet  100003     1500  -      -      -        -    -        0      0   
4    enet  100004     1500  -      -      -        -    -        0      0   
60   enet  100060     1500  -      -      -        -    -        0      0   
101  enet  100101     1500  -      -      -        -    -        0      0   
102  enet  100102     1500  -      -      -        -    -        0      0   
103  enet  100103     1500  -      -      -        -    -        0      0   
301  enet  100301     1500  -      -      -        -    -        0      0   
401  enet  100401     1500  -      -      -        -    -        0      0   
402  enet  100402     1500  -      -      -        -    -        0      0   
403  enet  100403     1500  -      -      -        -    -        0      0   
404  enet  100404     1500  -      -      -        -    -        0      0   
405  enet  100405     1500  -      -      -        -    -        0      0   
406  enet  100406     1500  -      -      -        -    -        0      0   
407  enet  100407     1500  -      -      -        -    -        0      0   
408  enet  100408     1500  -      -      -        -    -        0      0   
409  enet  100409     1500  -      -      -        -    -        0      0   
410  enet  100410     1500  -      -      -        -    -        0      0   
411  enet  100411     1500  -      -      -        -    -        0      0   
412  enet  100412     1500  -      -      -        -    -        0      0   
413  enet  100413     1500  -      -      -        -    -        0      0   
777  enet  100777     1500  -      -      -        -    -        0      0   
801  enet  100801     1500  -      -      -        -    -        0      0   
802  enet  100802     1500  -      -      -        -    -        0      0   
803  enet  100803     1500  -      -      -        -    -        0      0   
901  enet  100901     1500  -      -      -        -    -        0      0   
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   
1100 enet  101100     1500  -      -      -        -    -        0      0   
1102 enet  101102     1500  -      -      -        -    -        0      0   
1203 enet  101203     1500  -      -      -        -    -        0      0   

Remote SPAN VLANs
------------------------------------------------------------------------------


Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh vlan summary
Number of existing VLANs               : 34
 Number of existing VTP VLANs          : 31
 Number of existing extended VLANS     : 3

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh ip arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.10.10.129            -   00b1.e35a.cb7b  ARPA   Vlan777
Internet  172.27.128.1            -   00b1.e35a.cb4d  ARPA   Vlan102
Internet  172.27.130.11           0   Incomplete      ARPA   
Internet  172.27.130.14          30   0030.56a2.e726  ARPA   Vlan102
Internet  172.27.130.21           1   d89e.f320.1857  ARPA   Vlan102
Internet  172.27.130.25           0   0021.b707.2b00  ARPA   Vlan102
Internet  172.27.130.26           0   Incomplete      ARPA   
Internet  172.27.130.34           0   Incomplete      ARPA   
Internet  172.27.130.35           0   Incomplete      ARPA   
Internet  172.27.130.36           0   0025.b31e.0420  ARPA   Vlan102
Internet  172.27.130.39         243   0021.b767.e0bb  ARPA   Vlan102
Internet  172.27.130.40           0   0021.b707.cb47  ARPA   Vlan102
Internet  172.27.130.43           8   0016.6c73.8685  ARPA   Vlan102
Internet  172.27.130.46           0   Incomplete      ARPA   
Internet  172.27.130.47          57   0012.93f0.3881  ARPA   Vlan102
Internet  172.27.130.49           0   0050.56ad.3dda  ARPA   Vlan102
Internet  172.27.130.50           0   Incomplete      ARPA   
Internet  172.27.130.51           0   0021.b747.3a41  ARPA   Vlan102
Internet  172.27.130.52           0   Incomplete      ARPA   
Internet  172.27.130.53           2   0021.b72d.30fb  ARPA   Vlan102
Internet  172.27.130.54           0   Incomplete      ARPA   
Internet  172.27.130.55         145   0026.7341.3e8f  ARPA   Vlan102
Internet  172.27.130.56          44   0026.7341.3e85  ARPA   Vlan102
Internet  172.27.130.57         168   0026.7341.3f3c  ARPA   Vlan102
Internet  172.27.130.58         144   0021.b767.a06c  ARPA   Vlan102
Internet  172.27.130.59          36   0026.73aa.52e3  ARPA   Vlan102
Internet  172.27.130.60          21   0021.b7ab.2ee3  ARPA   Vlan102
Internet  172.27.130.61           0   Incomplete      ARPA   
Internet  172.27.130.62           0   Incomplete      ARPA   
Internet  172.27.130.63           0   0021.b747.7a94  ARPA   Vlan102
Internet  172.27.130.64           2   0021.b707.6bd6  ARPA   Vlan102
Internet  172.27.130.65           8   0021.b7ab.2e33  ARPA   Vlan102
Internet  172.27.130.66         148   0021.b767.603d  ARPA   Vlan102
Internet  172.27.130.67          21   0021.b767.a091  ARPA   Vlan102
Internet  172.27.130.68           0   Incomplete      ARPA   
Internet  172.27.130.69         250   0021.b7ab.eed5  ARPA   Vlan102
Internet  172.27.130.70           9   0021.b767.d071  ARPA   Vlan102
Internet  172.27.130.71         237   0021.b767.a08e  ARPA   Vlan102
Internet  172.27.130.72           0   0021.b747.dac2  ARPA   Vlan102
Internet  172.27.130.73           0   Incomplete      ARPA   
Internet  172.27.130.74           0   0021.b747.51bc  ARPA   Vlan102
Internet  172.27.130.75           0   0021.b747.bab8  ARPA   Vlan102
Internet  172.27.130.76           0   0021.b747.913a  ARPA   Vlan102
Internet  172.27.130.77          12   0021.b7ab.b255  ARPA   Vlan102
Internet  172.27.130.78         152   0026.7341.69b7  ARPA   Vlan102
Internet  172.27.130.79          21   0021.b7ab.ee43  ARPA   Vlan102
Internet  172.27.130.80           0   Incomplete      ARPA   
Internet  172.27.130.81          21   0021.b767.a0c9  ARPA   Vlan102
Internet  172.27.130.82          63   0021.b7ab.1e56  ARPA   Vlan102
Internet  172.27.130.84         215   0026.7341.258a  ARPA   Vlan102
Internet  172.27.130.85           0   0021.b747.5abf  ARPA   Vlan102
Internet  172.27.130.86          28   0000.749c.47bf  ARPA   Vlan102
Internet  172.27.130.87          77   0026.739e.59e2  ARPA   Vlan102
Internet  172.27.130.88           0   Incomplete      ARPA   
Internet  172.27.130.90          30   0030.56a2.e6e8  ARPA   Vlan102
Internet  172.27.130.91         243   0030.56a2.e79d  ARPA   Vlan102
Internet  172.27.130.92           2   0021.b747.dac0  ARPA   Vlan102
Internet  172.27.130.95           0   Incomplete      ARPA   
Internet  172.27.130.97          22   0002.d3f0.0000  ARPA   Vlan102
Internet  172.27.130.98         165   0021.b7ab.1e3c  ARPA   Vlan102
Internet  172.27.130.100          0   Incomplete      ARPA   
Internet  172.27.130.101          3   b0e8.9280.e52b  ARPA   Vlan102
Internet  172.27.130.102          0   Incomplete      ARPA   
Internet  172.27.130.103        164   0026.73ae.87f1  ARPA   Vlan102
Internet  172.27.131.0            0   d89e.f31b.eba7  ARPA   Vlan102
Internet  172.27.131.2          174   ec1d.8b2b.d14b  ARPA   Vlan102
Internet  172.27.131.4            1   0050.56ad.28ee  ARPA   Vlan102
Internet  172.27.131.5            2   0050.56ad.6110  ARPA   Vlan102
Internet  172.27.131.6            0   0050.56ad.6787  ARPA   Vlan102
Internet  172.27.131.7            0   a44c.c870.bfee  ARPA   Vlan102
Internet  172.27.131.9            1   0050.56ad.2708  ARPA   Vlan102
Internet  172.27.131.10         152   a44c.c880.2799  ARPA   Vlan102
Internet  172.27.131.11           1   d89e.f31b.d8c3  ARPA   Vlan102
Internet  172.27.131.12           0   d094.66fe.a9b7  ARPA   Vlan102
Internet  172.27.131.14         165   ec1d.8b2a.f109  ARPA   Vlan102
Internet  172.27.131.17           1   0050.56ad.6179  ARPA   Vlan102
Internet  172.27.131.22          12   0008.32aa.fde6  ARPA   Vlan102
Internet  172.27.131.23         174   ec1d.8b2b.d2b2  ARPA   Vlan102
Internet  172.27.131.24          10   0008.32aa.c0bd  ARPA   Vlan102
Internet  172.27.131.28           0   d89e.f31f.af02  ARPA   Vlan102
Internet  172.27.131.29           1   d89e.f320.0828  ARPA   Vlan102
Internet  172.27.131.80           0   Incomplete      ARPA   
Internet  172.27.131.163          0   Incomplete      ARPA   
Internet  172.27.134.1            -   00b1.e35a.cb72  ARPA   Vlan406
Internet  172.27.134.39           0   Incomplete      ARPA   
Internet  172.27.181.193          -   00b1.e35a.cb6e  ARPA   Vlan411
Internet  172.27.181.225          -   00b1.e35a.cb5b  ARPA   Vlan412
Internet  172.27.184.129          -   00b1.e35a.cb6f  ARPA   Vlan403
Internet  172.27.184.187          0   Incomplete      ARPA   
Internet  172.27.185.1            -   00b1.e35a.cb6f  ARPA   Vlan401
Internet  172.27.185.2            0   Incomplete      ARPA   
Internet  172.27.185.3           57   90b1.1c05.12ea  ARPA   Vlan401
Internet  172.27.185.7            0   000c.be02.da58  ARPA   Vlan401
Internet  172.27.185.9          155   0002.9904.1764  ARPA   Vlan401
Internet  172.27.185.11         167   90ac.3f06.6c6b  ARPA   Vlan401
Internet  172.27.185.12          23   90ac.3f06.6c67  ARPA   Vlan401
Internet  172.27.185.13          51   90ac.3f06.6dcb  ARPA   Vlan401
Internet  172.27.185.14           6   90ac.3f06.6e81  ARPA   Vlan401
Internet  172.27.185.16           1   90ac.3f06.6c59  ARPA   Vlan401
Internet  172.27.185.17         125   90ac.3f06.6e8b  ARPA   Vlan401
Internet  172.27.185.18         157   90ac.3f06.6c4f  ARPA   Vlan401
Internet  172.27.185.19          15   90ac.3f06.6ea6  ARPA   Vlan401
Internet  172.27.185.21           0   90ac.3f06.6c56  ARPA   Vlan401
Internet  172.27.185.22           5   0017.fc3e.03db  ARPA   Vlan401
Internet  172.27.185.27          59   0030.56a1.5bbf  ARPA   Vlan401
Internet  172.27.185.28          46   d89e.f31f.da92  ARPA   Vlan401
Internet  172.27.186.1            -   00b1.e35a.cb41  ARPA   Vlan101
Internet  172.27.186.3            2   0017.fc3e.03ce  ARPA   Vlan101
Internet  172.27.186.4            2   0017.fc10.cc51  ARPA   Vlan101
Internet  172.27.186.5            0   0017.fc10.cc4e  ARPA   Vlan101
Internet  172.27.186.6            2   0017.fc10.cc0b  ARPA   Vlan101
Internet  172.27.186.8           52   0017.fc21.caa5  ARPA   Vlan101
Internet  172.27.186.11           5   0017.fc10.cc0a  ARPA   Vlan101
Internet  172.27.186.15          96   1065.30f4.5e7a  ARPA   Vlan101
Internet  172.27.186.19           0   d89e.f31b.ea9d  ARPA   Vlan101
Internet  172.27.186.21           0   d89e.f31b.de50  ARPA   Vlan101
Internet  172.27.186.24         161   0023.15e6.5541  ARPA   Vlan101
Internet  172.27.186.32           0   0050.56ad.1b0d  ARPA   Vlan101
Internet  172.27.186.37          94   d89e.f31b.dd9e  ARPA   Vlan101
Internet  172.27.186.42           0   d89e.f31b.fb15  ARPA   Vlan101
Internet  172.27.186.47           0   d425.8beb.ece5  ARPA   Vlan101
Internet  172.27.186.49         151   2446.c84c.54cd  ARPA   Vlan101
Internet  172.27.186.56          72   fca6.2178.e159  ARPA   Vlan101
Internet  172.27.186.57           0   d425.8beb.ebf5  ARPA   Vlan101
Internet  172.27.186.58         125   d077.1488.bcde  ARPA   Vlan101
Internet  172.27.186.65         116   d425.8bea.4f20  ARPA   Vlan101
Internet  172.27.186.66           0   4c34.8836.9174  ARPA   Vlan101
Internet  172.27.186.67          56   d425.8beb.ec31  ARPA   Vlan101
Internet  172.27.186.70         191   0087.0108.05ea  ARPA   Vlan101
Internet  172.27.186.71           0   d425.8bea.4944  ARPA   Vlan101
Internet  172.27.186.73           0   d89e.f31b.d38a  ARPA   Vlan101
Internet  172.27.186.75           0   d425.8beb.ec59  ARPA   Vlan101
Internet  172.27.186.76           0   2047.47d1.9507  ARPA   Vlan101
Internet  172.27.186.78           0   0021.6afd.4b8e  ARPA   Vlan101
Internet  172.27.186.80           0   d89e.f31b.dfdb  ARPA   Vlan101
Internet  172.27.186.82           0   d46a.6ad9.8e85  ARPA   Vlan101
Internet  172.27.186.84           0   f859.7144.a41c  ARPA   Vlan101
Internet  172.27.186.91           0   0050.56ad.26cb  ARPA   Vlan101
Internet  172.27.186.92         122   8840.3bd6.7da7  ARPA   Vlan101
Internet  172.27.186.93           0   a44c.c870.5e76  ARPA   Vlan101
Internet  172.27.186.95           0   d425.8beb.eb41  ARPA   Vlan101
Internet  172.27.186.96         178   0023.15f4.c6e9  ARPA   Vlan101
Internet  172.27.186.97         162   d425.8bea.499e  ARPA   Vlan101
Internet  172.27.186.101          0   0050.56ad.70d4  ARPA   Vlan101
Internet  172.27.186.102          0   d425.8beb.eb5f  ARPA   Vlan101
Internet  172.27.186.103          0   0050.56ad.52ea  ARPA   Vlan101
Internet  172.27.186.104          0   0050.56ad.64d1  ARPA   Vlan101
Internet  172.27.186.107        171   d425.8beb.ecef  ARPA   Vlan101
Internet  172.27.186.108        267   d463.c6ee.4459  ARPA   Vlan101
Internet  172.27.186.110          0   d89e.f31f.db0e  ARPA   Vlan101
Internet  172.27.186.111          0   a44c.c880.2cc3  ARPA   Vlan101
Internet  172.27.186.113          0   0023.15f6.c9f3  ARPA   Vlan101
Internet  172.27.186.114          0   d89e.f31b.f3d4  ARPA   Vlan101
Internet  172.27.186.116          0   8000.0b81.7261  ARPA   Vlan101
Internet  172.27.186.117         29   d425.8beb.ec63  ARPA   Vlan101
Internet  172.27.186.118          0   60f6.772a.f2ec  ARPA   Vlan101
Internet  172.27.186.119          0   d425.8beb.ea74  ARPA   Vlan101
Internet  172.27.186.120          0   d425.8beb.ecea  ARPA   Vlan101
Internet  172.27.186.123         20   d89e.f31b.df6f  ARPA   Vlan101
Internet  172.27.186.124        107   8ceb.c642.ee58  ARPA   Vlan101
Internet  172.27.186.128          0   d89e.f31b.f598  ARPA   Vlan101
Internet  172.27.186.129          0   0021.6afd.b59c  ARPA   Vlan101
Internet  172.27.186.131          0   4c34.8836.9192  ARPA   Vlan101
Internet  172.27.186.132          0   d89e.f321.fdd6  ARPA   Vlan101
Internet  172.27.186.133          0   d89e.f31b.effa  ARPA   Vlan101
Internet  172.27.186.134          0   0021.b767.9072  ARPA   Vlan101
Internet  172.27.186.135         51   d425.8beb.ea8d  ARPA   Vlan101
Internet  172.27.186.136          0   0023.15e6.7c0b  ARPA   Vlan101
Internet  172.27.186.137          0   1065.30f6.1b1a  ARPA   Vlan101
Internet  172.27.186.139          0   d425.8bea.2e9b  ARPA   Vlan101
Internet  172.27.186.143         99   7ca1.7773.8478  ARPA   Vlan101
Internet  172.27.186.144          0   14ab.c547.15e4  ARPA   Vlan101
Internet  172.27.186.145          0   0050.56ad.24d5  ARPA   Vlan101
Internet  172.27.186.146          4   a44c.c870.5e48  ARPA   Vlan101
Internet  172.27.186.147          0   a44c.c870.bff7  ARPA   Vlan101
Internet  172.27.186.148         87   8a7a.c762.10df  ARPA   Vlan101
Internet  172.27.186.149          0   60f6.772a.f341  ARPA   Vlan101
Internet  172.27.186.150        150   60f6.775f.392b  ARPA   Vlan101
Internet  172.27.186.151          0   d425.8bea.4ea3  ARPA   Vlan101
Internet  172.27.186.152        225   ac07.5f75.ef15  ARPA   Vlan101
Internet  172.27.186.154          0   d89e.f31b.ef6e  ARPA   Vlan101
Internet  172.27.186.155          0   d425.8bea.4976  ARPA   Vlan101
Internet  172.27.186.156          0   0050.56ad.0853  ARPA   Vlan101
Internet  172.27.186.157          0   d425.8beb.ec3b  ARPA   Vlan101
Internet  172.27.186.158          0   1065.30f5.c1f5  ARPA   Vlan101
Internet  172.27.186.159         39   0021.6afd.40f3  ARPA   Vlan101
Internet  172.27.186.160         32   04b1.6702.6a1d  ARPA   Vlan101
Internet  172.27.186.161          0   d89e.f31b.e9f9  ARPA   Vlan101
Internet  172.27.186.162          0   d89e.f31b.f27c  ARPA   Vlan101
Internet  172.27.186.166          0   0023.15f4.c61c  ARPA   Vlan101
Internet  172.27.186.169          0   6c2b.59d9.5c37  ARPA   Vlan101
Internet  172.27.186.170        158   d425.8bea.36a7  ARPA   Vlan101
Internet  172.27.186.172          0   d89e.f31b.de36  ARPA   Vlan101
Internet  172.27.186.173          0   1002.b5b5.7368  ARPA   Vlan101
Internet  172.27.186.174          0   0023.15e6.7ac1  ARPA   Vlan101
Internet  172.27.186.175          0   d89e.f31f.bf56  ARPA   Vlan101
Internet  172.27.186.176         41   58d9.c3a7.c5e4  ARPA   Vlan101
Internet  172.27.186.177         54   d425.8bea.38f0  ARPA   Vlan101
Internet  172.27.186.178         40   601d.91c3.289a  ARPA   Vlan101
Internet  172.27.186.179          0   8019.34e2.efe1  ARPA   Vlan101
Internet  172.27.186.180          6   acbd.7043.d264  ARPA   Vlan101
Internet  172.27.186.185        233   0087.0160.f9b6  ARPA   Vlan101
Internet  172.27.186.186         29   dcbf.e91c.4b00  ARPA   Vlan101
Internet  172.27.186.187          0   a434.d972.015a  ARPA   Vlan101
Internet  172.27.186.189          0   a434.d972.06c8  ARPA   Vlan101
Internet  172.27.186.190         54   d481.d794.78c0  ARPA   Vlan101
Internet  172.27.186.193          0   d89e.f31b.c3f6  ARPA   Vlan101
Internet  172.27.186.196          0   d89e.f31b.e49d  ARPA   Vlan101
Internet  172.27.186.197          0   d89e.f31b.dc8b  ARPA   Vlan101
Internet  172.27.186.199          0   d89e.f321.f2bb  ARPA   Vlan101
Internet  172.27.186.207         88   a89f.baaa.4327  ARPA   Vlan101
Internet  172.27.186.208         36   a896.7521.6a82  ARPA   Vlan101
Internet  172.27.186.209          0   d89e.f320.0797  ARPA   Vlan101
Internet  172.27.186.216          0   0050.56ad.3cb2  ARPA   Vlan101
Internet  172.27.186.218          0   d89e.f321.f60d  ARPA   Vlan101
Internet  172.27.186.221        198   dc66.7249.a399  ARPA   Vlan101
Internet  172.27.186.223          0   1065.30f6.1b3f  ARPA   Vlan101
Internet  172.27.186.225          0   d89e.f31b.de6c  ARPA   Vlan101
Internet  172.27.186.226         14   0023.15f4.c9ff  ARPA   Vlan101
Internet  172.27.186.228          0   0050.56ad.0888  ARPA   Vlan101
Internet  172.27.186.230          0   d89e.f31b.ede8  ARPA   Vlan101
Internet  172.27.186.234          0   0023.15f4.c62b  ARPA   Vlan101
Internet  172.27.186.236          0   588a.5a01.787e  ARPA   Vlan101
Internet  172.27.186.237          0   a44c.c871.5fd2  ARPA   Vlan101
Internet  172.27.186.238          0   d89e.f322.0207  ARPA   Vlan101
Internet  172.27.186.239          0   1065.30f6.1b2d  ARPA   Vlan101
Internet  172.27.186.240          0   0023.15f4.ca9f  ARPA   Vlan101
Internet  172.27.186.241          0   a44c.c870.c013  ARPA   Vlan101
Internet  172.27.186.242         28   d094.66ff.196b  ARPA   Vlan101
Internet  172.27.186.244          0   2816.ad51.f0f9  ARPA   Vlan101
Internet  172.27.186.246          0   588a.5a0e.afcb  ARPA   Vlan101
Internet  172.27.186.247         79   b4cd.27e2.f273  ARPA   Vlan101
Internet  172.27.186.254          0   d89e.f31b.f3fe  ARPA   Vlan101
Internet  172.27.187.2            0   d89e.f321.fea0  ARPA   Vlan101
Internet  172.27.187.4            0   d89e.f31b.d8c5  ARPA   Vlan101
Internet  172.27.187.7            0   2816.ad52.436a  ARPA   Vlan101
Internet  172.27.187.9            0   d425.8bea.4eee  ARPA   Vlan101
Internet  172.27.187.15           0   1065.30f5.c1f6  ARPA   Vlan101
Internet  172.27.187.18           0   1065.30f6.1b3a  ARPA   Vlan101
Internet  172.27.187.19           0   0021.6afd.4ddc  ARPA   Vlan101
Internet  172.27.187.21           0   a44c.c870.5e9b  ARPA   Vlan101
Internet  172.27.187.23           0   0023.15f5.263e  ARPA   Vlan101
Internet  172.27.187.25           0   d89e.f31b.bcef  ARPA   Vlan101
Internet  172.27.187.26           7   60f6.772a.f2ab  ARPA   Vlan101
Internet  172.27.187.31           0   Incomplete      ARPA   
Internet  172.27.187.32           0   d89e.f31b.f810  ARPA   Vlan101
Internet  172.27.187.34         132   d425.8beb.eb96  ARPA   Vlan101
Internet  172.27.187.36           0   d89e.f31b.d9c0  ARPA   Vlan101
Internet  172.27.187.43           0   d89e.f321.fed7  ARPA   Vlan101
Internet  172.27.187.44         227   fc2d.5eff.7079  ARPA   Vlan101
Internet  172.27.187.47           0   d89e.f31b.d9ae  ARPA   Vlan101
Internet  172.27.187.49           0   0023.15e6.7c6a  ARPA   Vlan101
Internet  172.27.187.52          46   0021.6afd.b5ba  ARPA   Vlan101
Internet  172.27.187.55         144   1092.667c.a3c0  ARPA   Vlan101
Internet  172.27.187.58           0   a44c.c870.bff4  ARPA   Vlan101
Internet  172.27.187.72           0   0021.6afd.4dff  ARPA   Vlan101
Internet  172.27.187.73           0   1065.30f6.a095  ARPA   Vlan101
Internet  172.27.187.75           0   1065.30f3.23fe  ARPA   Vlan101
Internet  172.27.187.76           0   1065.30f3.1c94  ARPA   Vlan101
Internet  172.27.187.77           0   1065.30f6.185b  ARPA   Vlan101
Internet  172.27.187.78           0   d89e.f31b.f303  ARPA   Vlan101
Internet  172.27.187.81           0   d89e.f31f.f957  ARPA   Vlan101
Internet  172.27.187.82           0   d89e.f31b.ddc1  ARPA   Vlan101
Internet  172.27.187.86           0   d89e.f31b.eb91  ARPA   Vlan101
Internet  172.27.187.90         129   d4c9.4b85.d72e  ARPA   Vlan101
Internet  172.27.187.92           0   1065.30f5.c1fc  ARPA   Vlan101
Internet  172.27.187.94           0   d89e.f31f.db02  ARPA   Vlan101
Internet  172.27.187.96           0   1065.30f5.c250  ARPA   Vlan101
Internet  172.27.187.101        195   6014.66c3.e957  ARPA   Vlan101
Internet  172.27.187.102          0   0023.15f4.ca4a  ARPA   Vlan101
Internet  172.27.187.104          0   d89e.f31b.d5b6  ARPA   Vlan101
Internet  172.27.187.105          0   1065.30f3.1e40  ARPA   Vlan101
Internet  172.27.187.108        121   fcb6.d88c.4b4d  ARPA   Vlan101
Internet  172.27.187.109        154   0023.15f5.b72f  ARPA   Vlan101
Internet  172.27.187.113         23   d425.8beb.ed1c  ARPA   Vlan101
Internet  172.27.187.114          0   d89e.f31b.d8ce  ARPA   Vlan101
Internet  172.27.187.116         47   d425.8beb.eb5a  ARPA   Vlan101
Internet  172.27.187.117        163   d425.8bea.3693  ARPA   Vlan101
Internet  172.27.187.118          0   d89e.f31b.ed66  ARPA   Vlan101
Internet  172.27.187.119          0   0023.15f4.ca77  ARPA   Vlan101
Internet  172.27.187.120        142   0017.fc10.c2a0  ARPA   Vlan101
Internet  172.27.187.121          0   d094.663d.9de6  ARPA   Vlan101
Internet  172.27.187.123          0   d89e.f31b.f316  ARPA   Vlan101
Internet  172.27.187.124          0   1065.30f2.8205  ARPA   Vlan101
Internet  172.27.187.129          0   a44c.c870.5eae  ARPA   Vlan101
Internet  172.27.187.130          0   d89e.f31b.eb95  ARPA   Vlan101
Internet  172.27.187.131          0   1803.7336.3e95  ARPA   Vlan101
Internet  172.27.187.133          0   1065.30f6.1b30  ARPA   Vlan101
Internet  172.27.187.134        115   c08c.714c.949c  ARPA   Vlan101
Internet  172.27.187.136          0   a44c.c870.bff9  ARPA   Vlan101
Internet  172.27.187.137         70   d463.c65d.5a4d  ARPA   Vlan101
Internet  172.27.187.138          4   d89e.f31b.f4b0  ARPA   Vlan101
Internet  172.27.187.140          0   d89e.f321.f181  ARPA   Vlan101
Internet  172.27.187.141          0   d89e.f31b.f224  ARPA   Vlan101
Internet  172.27.187.142        122   0023.15e8.6188  ARPA   Vlan101
Internet  172.27.187.145          0   d425.8bea.4ef8  ARPA   Vlan101
Internet  172.27.187.146          0   d89e.f322.0102  ARPA   Vlan101
Internet  172.27.187.147          0   d89e.f31b.edba  ARPA   Vlan101
Internet  172.27.187.148          0   d425.8beb.ec86  ARPA   Vlan101
Internet  172.27.187.149         97   a44c.c870.bff7  ARPA   Vlan101
Internet  172.27.187.150          0   d89e.f31b.dcde  ARPA   Vlan101
Internet  172.27.187.151          0   d89e.f31b.eed7  ARPA   Vlan101
Internet  172.27.187.155          0   d89e.f321.f1e7  ARPA   Vlan101
Internet  172.27.187.156          0   1065.30f6.1be4  ARPA   Vlan101
Internet  172.27.187.159          0   1065.30f6.a26c  ARPA   Vlan101
Internet  172.27.187.160          0   e4b9.7a32.cabe  ARPA   Vlan101
Internet  172.27.187.165         82   a44c.c870.604c  ARPA   Vlan101
Internet  172.27.187.167          0   d89e.f31b.d903  ARPA   Vlan101
Internet  172.27.187.169          0   d89e.f31b.ee04  ARPA   Vlan101
Internet  172.27.187.178          0   1065.30f6.a22b  ARPA   Vlan101
Internet  172.27.187.184          0   d89e.f321.f251  ARPA   Vlan101
Internet  172.27.187.187          0   1803.7337.dd49  ARPA   Vlan101
Internet  172.27.187.189          0   a44c.c870.bff3  ARPA   Vlan101
Internet  172.27.187.193          0   1065.30f6.1bf9  ARPA   Vlan101
Internet  172.27.187.194          0   1803.7337.dce1  ARPA   Vlan101
Internet  172.27.187.196          0   a44c.c870.600a  ARPA   Vlan101
Internet  172.27.187.198         39   d094.66fe.a995  ARPA   Vlan101
Internet  172.27.187.203          0   d425.8bea.4912  ARPA   Vlan101
Internet  172.27.187.207          0   000e.c6b6.79f6  ARPA   Vlan101
Internet  172.27.187.214          0   a44c.c871.600e  ARPA   Vlan101
Internet  172.27.187.216          3   d425.8bea.4980  ARPA   Vlan101
Internet  172.27.187.219          0   d89e.f31b.de63  ARPA   Vlan101
Internet  172.27.187.224          0   60f6.772a.f337  ARPA   Vlan101
Internet  172.27.187.225        242   0479.7010.fa32  ARPA   Vlan101
Internet  172.27.187.227         90   78f8.82d0.50a0  ARPA   Vlan101
Internet  172.27.187.228          0   d425.8beb.ec95  ARPA   Vlan101
Internet  172.27.187.229          0   d89e.f31b.efa9  ARPA   Vlan101
Internet  172.27.187.230        187   0008.22a1.5551  ARPA   Vlan101
Internet  172.27.187.231        207   240d.c20e.a10d  ARPA   Vlan101
Internet  172.27.187.232        199   ec10.7bcd.16a8  ARPA   Vlan101
Internet  172.27.187.234          0   a44c.c824.5f62  ARPA   Vlan101
Internet  172.27.187.248          0   d89e.f31b.f8ca  ARPA   Vlan101
Internet  172.27.187.250          0   588a.5a06.2d9f  ARPA   Vlan101
Internet  172.27.187.254          0   d89e.f31b.dfd8  ARPA   Vlan101
Internet  172.27.188.1            -   00b1.e35a.cb61  ARPA   Vlan1102
Internet  172.27.188.2            0   0050.56ad.6b7a  ARPA   Vlan1102
Internet  172.27.188.4           17   1098.36ae.5082  ARPA   Vlan1102
Internet  172.27.188.5           73   90b1.1c08.3a87  ARPA   Vlan1102
Internet  172.27.188.8           54   1098.36ae.5081  ARPA   Vlan1102
Internet  172.27.188.9           72   90b1.1c49.f6c6  ARPA   Vlan1102
Internet  172.27.188.10           0   782b.cb56.bfae  ARPA   Vlan1102
Internet  172.27.188.12           0   Incomplete      ARPA   
Internet  172.27.188.13           0   0050.56ad.5067  ARPA   Vlan1102
Internet  172.27.188.14           1   d094.663d.9de5  ARPA   Vlan1102
Internet  172.27.188.15           0   Incomplete      ARPA   
Internet  172.27.188.17           0   000c.29e1.b881  ARPA   Vlan1102
Internet  172.27.188.18           0   000c.2988.f02c  ARPA   Vlan1102
Internet  172.27.188.20           0   0050.56ad.1eae  ARPA   Vlan1102
Internet  172.27.188.21           0   0050.56ad.7fde  ARPA   Vlan1102
Internet  172.27.188.24         193   000e.1115.2d0e  ARPA   Vlan1102
Internet  172.27.188.25           0   Incomplete      ARPA   
Internet  172.27.188.29           0   001b.218e.a7a0  ARPA   Vlan1102
Internet  172.27.188.32          20   0026.7344.44e1  ARPA   Vlan1102
Internet  172.27.188.33           0   0050.56ad.458f  ARPA   Vlan1102
Internet  172.27.188.37           0   90b1.1c05.6aea  ARPA   Vlan1102
Internet  172.27.188.41           0   0050.56ad.21b5  ARPA   Vlan1102
Internet  172.27.188.42          82   d094.663c.a508  ARPA   Vlan1102
Internet  172.27.188.53          37   0050.56ad.655f  ARPA   Vlan1102
Internet  172.27.188.54         206   8018.44e4.f351  ARPA   Vlan1102
Internet  172.27.188.55           0   0050.56ad.68e2  ARPA   Vlan1102
Internet  172.27.188.56         245   8018.44e4.f150  ARPA   Vlan1102
Internet  172.27.188.58           0   000a.f7be.5c1b  ARPA   Vlan1102
Internet  172.27.188.59           0   0050.56ad.1a34  ARPA   Vlan1102
Internet  172.27.188.60           0   0010.18dc.1830  ARPA   Vlan1102
Internet  172.27.188.62           0   001b.217e.78c0  ARPA   Vlan1102
Internet  172.27.188.65           -   00b1.e35a.cb70  ARPA   Vlan802
Internet  172.27.188.67          43   d094.663d.9ddf  ARPA   Vlan802
Internet  172.27.188.68           5   0050.56ad.2fe2  ARPA   Vlan802
Internet  172.27.188.69           1   0050.56ad.0b88  ARPA   Vlan802
Internet  172.27.188.74           0   Incomplete      ARPA   
Internet  172.27.188.83          14   0050.56ad.678b  ARPA   Vlan802
Internet  172.27.188.85           0   Incomplete      ARPA   
Internet  172.27.188.86           7   001e.67c4.fb75  ARPA   Vlan802
Internet  172.27.188.87           4   001e.6783.cb83  ARPA   Vlan802
Internet  172.27.188.90         154   001e.67c5.5b6d  ARPA   Vlan802
Internet  172.27.188.91           0   40f2.e9f4.b354  ARPA   Vlan802
Internet  172.27.188.92           0   40f2.e9f4.c90c  ARPA   Vlan802
Internet  172.27.188.104         80   d094.663c.a2ea  ARPA   Vlan802
Internet  172.27.188.106        201   d094.663f.6c75  ARPA   Vlan802
Internet  172.27.188.113         31   001e.67b3.ce28  ARPA   Vlan802
Internet  172.27.188.122          0   0050.56ad.7333  ARPA   Vlan802
Internet  172.27.188.123          6   0050.56ad.7408  ARPA   Vlan802
Internet  172.27.188.129          -   00b1.e35a.cb67  ARPA   Vlan3
Internet  172.27.188.130        176   00a5.bf52.3667  ARPA   Vlan3
Internet  172.27.188.132        138   247e.120f.14c1  ARPA   Vlan3
Internet  172.27.188.133         16   b4a8.b9da.7ac3  ARPA   Vlan3
Internet  172.27.188.138         15   501c.b047.cdc3  ARPA   Vlan3
Internet  172.27.188.139        218   247e.1211.43c1  ARPA   Vlan3
Internet  172.27.188.140         24   247e.1205.a8c3  ARPA   Vlan3
Internet  172.27.188.141        267   5006.ab33.a243  ARPA   Vlan3
Internet  172.27.188.142         57   247e.1211.c641  ARPA   Vlan3
Internet  172.27.188.144        187   247e.12db.3a41  ARPA   Vlan3
Internet  172.27.188.145        146   247e.1211.bec1  ARPA   Vlan3
Internet  172.27.188.146        130   247e.127b.dcc1  ARPA   Vlan3
Internet  172.27.188.147        171   247e.12db.4f41  ARPA   Vlan3
Internet  172.27.188.148        110   247e.12db.5a41  ARPA   Vlan3
Internet  172.27.188.153          0   707d.b9c3.a4e8  ARPA   Vlan3
Internet  172.27.188.154          0   380e.4d66.4c80  ARPA   Vlan3
Internet  172.27.188.155          0   00a7.4293.53a4  ARPA   Vlan3
Internet  172.27.188.156          0   380e.4d66.4c6c  ARPA   Vlan3
Internet  172.27.188.157         11   0017.fc21.fb98  ARPA   Vlan3
Internet  172.27.188.158        211   0017.fc10.8392  ARPA   Vlan3
Internet  172.27.188.167        117   d067.e58e.c2f8  ARPA   Vlan3
Internet  172.27.188.171          0   380e.4d66.4c84  ARPA   Vlan3
Internet  172.27.188.172          0   380e.4d66.21d0  ARPA   Vlan3
Internet  172.27.188.173         36   247e.12db.7641  ARPA   Vlan3
Internet  172.27.188.176        241   549f.3525.1f2e  ARPA   Vlan3
Internet  172.27.188.177        174   549f.3522.5c02  ARPA   Vlan3
Internet  172.27.188.178         88   f8bc.124b.7662  ARPA   Vlan3
Internet  172.27.188.179        123   f8bc.124b.4b02  ARPA   Vlan3
Internet  172.27.188.180        200   1418.774b.b170  ARPA   Vlan3
Internet  172.27.188.181        216   c81f.66c2.86cb  ARPA   Vlan3
Internet  172.27.188.182         23   1418.774b.b170  ARPA   Vlan3
Internet  172.27.188.183        118   f8bc.124f.f13e  ARPA   Vlan3
Internet  172.27.188.184         89   000e.1115.1e6e  ARPA   Vlan3
Internet  172.27.188.185        122   f8b1.5642.49dd  ARPA   Vlan3
Internet  172.27.188.188         68   001e.6783.cb88  ARPA   Vlan3
Internet  172.27.188.189        130   90b1.1c00.c2d8  ARPA   Vlan3
Internet  172.27.188.193          -   00b1.e35a.cb57  ARPA   Vlan4
Internet  172.27.188.194          0   Incomplete      ARPA   
Internet  172.27.188.196          0   707d.b9c3.a524  ARPA   Vlan4
Internet  172.27.188.198         18   2cab.eb4d.e3c1  ARPA   Vlan4
Internet  172.27.188.199          0   380e.4d66.4f34  ARPA   Vlan4
Internet  172.27.188.200          0   380e.4d66.4d24  ARPA   Vlan4
Internet  172.27.188.202          0   380e.4d66.4c5c  ARPA   Vlan4
Internet  172.27.188.203          0   0041.d24f.88c1  ARPA   Vlan4
Internet  172.27.188.204          0   00cc.fc9e.f0c1  ARPA   Vlan4
Internet  172.27.188.205          3   0016.6cb2.12ff  ARPA   Vlan4
Internet  172.27.188.206          0   3890.a5d0.bf28  ARPA   Vlan4
Internet  172.27.188.210          0   743e.2b11.3390  ARPA   Vlan4
Internet  172.27.188.211          0   743e.2b10.fa40  ARPA   Vlan4
Internet  172.27.188.212        163   c4b9.cdbb.2441  ARPA   Vlan4
Internet  172.27.188.215        104   247e.1210.4142  ARPA   Vlan4
Internet  172.27.188.216          0   707d.b969.de9c  ARPA   Vlan4
Internet  172.27.188.217          0   00a7.42cc.0824  ARPA   Vlan4
Internet  172.27.188.218        235   0016.6cc7.6a24  ARPA   Vlan4
Internet  172.27.189.1            -   00b1.e35a.cb5d  ARPA   Vlan103
Internet  172.27.189.10           0   0021.b797.168e  ARPA   Vlan103
Internet  172.27.189.65           -   00b1.e35a.cb4f  ARPA   Vlan413
Internet  172.27.189.67           0   2857.becf.b662  ARPA   Vlan413
Internet  172.27.189.68           0   94e1.ac33.127a  ARPA   Vlan413
Internet  172.27.189.70           0   2857.becf.b737  ARPA   Vlan413
Internet  172.27.189.71           0   2857.becf.b729  ARPA   Vlan413
Internet  172.27.189.72           0   2857.becf.b6a6  ARPA   Vlan413
Internet  172.27.189.73           0   2857.becf.b72c  ARPA   Vlan413
Internet  172.27.189.75           0   2857.becf.b712  ARPA   Vlan413
Internet  172.27.189.76           0   2857.becf.b6ab  ARPA   Vlan413
Internet  172.27.189.80           2   94e1.accc.d6de  ARPA   Vlan413
Internet  172.27.189.81           0   94e1.accc.d701  ARPA   Vlan413
Internet  172.27.189.82           2   94e1.accc.d6bd  ARPA   Vlan413
Internet  172.27.189.83           2   94e1.accc.d765  ARPA   Vlan413
Internet  172.27.189.84           1   94e1.accc.d685  ARPA   Vlan413
Internet  172.27.189.86           0   94e1.accc.d6ed  ARPA   Vlan413
Internet  172.27.189.87           0   0016.6cc7.6a25  ARPA   Vlan413
Internet  172.27.189.129          -   00b1.e35a.cb48  ARPA   Vlan901
Internet  172.27.189.134          0   Incomplete      ARPA   
Internet  172.27.189.137         43   64f6.9d0b.ab91  ARPA   Vlan901
Internet  172.27.189.138        141   a44c.1170.6701  ARPA   Vlan901
Internet  172.27.189.139        129   c067.afc5.74e1  ARPA   Vlan901
Internet  172.27.190.1            -   00b1.e35a.cb62  ARPA   Vlan301
Internet  172.27.190.4          110   0045.1dab.b030  ARPA   Vlan301
Internet  172.27.190.5           50   0045.1dab.aa90  ARPA   Vlan301
Internet  172.27.190.11          98   ec1d.8b2b.10a1  ARPA   Vlan301
Internet  172.27.190.12           7   0008.32ab.0ce3  ARPA   Vlan301
Internet  172.27.190.13         161   ec1d.8b2b.d1b4  ARPA   Vlan301
Internet  172.27.190.14         174   ec1d.8b2b.79e8  ARPA   Vlan301
Internet  172.27.190.15         110   ac44.f219.ea5e  ARPA   Vlan301
Internet  172.27.190.16           0   ec1d.8b2b.087b  ARPA   Vlan301
Internet  172.27.190.17          46   ec1d.8b2b.7aa2  ARPA   Vlan301
Internet  172.27.190.18           0   ec1d.8b2b.d1d2  ARPA   Vlan301
Internet  172.27.190.20         171   ec1d.8b2b.0d0e  ARPA   Vlan301
Internet  172.27.190.21          95   ec1d.8b2b.7ab0  ARPA   Vlan301
Internet  172.27.190.22          30   ec1d.8b2b.148d  ARPA   Vlan301
Internet  172.27.190.23           9   0008.32ab.0e21  ARPA   Vlan301
Internet  172.27.190.24          84   ec1d.8b2b.7a8b  ARPA   Vlan301
Internet  172.27.190.28         122   ec1d.8b2b.d8e0  ARPA   Vlan301
Internet  172.27.190.29          50   ec1d.8b2b.d1f9  ARPA   Vlan301
Internet  172.27.190.30          17   ec1d.8b2b.11ba  ARPA   Vlan301
Internet  172.27.190.31          27   ec1d.8b2b.745f  ARPA   Vlan301
Internet  172.27.190.32           3   ec1d.8b2b.7a68  ARPA   Vlan301
Internet  172.27.190.33           1   ec1d.8b2b.7088  ARPA   Vlan301
Internet  172.27.190.34           2   0008.32aa.fdce  ARPA   Vlan301
Internet  172.27.190.35         209   ec1d.8b2b.710c  ARPA   Vlan301
Internet  172.27.190.36          25   ec1d.8b2b.06c2  ARPA   Vlan301
Internet  172.27.190.37         165   ec1d.8b2b.10cb  ARPA   Vlan301
Internet  172.27.190.38           6   ec1d.8b2b.6ef2  ARPA   Vlan301
Internet  172.27.190.39          63   ec1d.8b2b.d58a  ARPA   Vlan301
Internet  172.27.190.40         118   ec1d.8b2a.4fef  ARPA   Vlan301
Internet  172.27.190.42         231   ec1d.8b2b.11bf  ARPA   Vlan301
Internet  172.27.190.43         175   ec1d.8b2b.7a0c  ARPA   Vlan301
Internet  172.27.190.44          53   ec1d.8b2b.7a6f  ARPA   Vlan301
Internet  172.27.190.45         201   ec1d.8b2b.1d69  ARPA   Vlan301
Internet  172.27.190.48          31   ec1d.8b2b.d067  ARPA   Vlan301
Internet  172.27.190.50         126   ec1d.8bba.50d2  ARPA   Vlan301
Internet  172.27.190.51           9   0008.32ab.014c  ARPA   Vlan301
Internet  172.27.190.52          52   ec1d.8b2b.7a0e  ARPA   Vlan301
Internet  172.27.190.53          61   ec1d.8b2b.d5c4  ARPA   Vlan301
Internet  172.27.190.55         226   ec1d.8b2b.d1d5  ARPA   Vlan301
Internet  172.27.190.56          47   ec1d.8b2b.0451  ARPA   Vlan301
Internet  172.27.190.57          66   ec1d.8b2b.125a  ARPA   Vlan301
Internet  172.27.190.58         131   ec1d.8b2b.d1b6  ARPA   Vlan301
Internet  172.27.190.59         237   ec1d.8b2b.d90a  ARPA   Vlan301
Internet  172.27.190.60          67   ec1d.8b2b.d882  ARPA   Vlan301
Internet  172.27.190.61          77   ec1d.8b2b.74ed  ARPA   Vlan301
Internet  172.27.190.63          65   0008.32aa.f6f3  ARPA   Vlan301
Internet  172.27.190.64          81   0008.32aa.f39f  ARPA   Vlan301
Internet  172.27.190.65           0   ec1d.8b2b.d727  ARPA   Vlan301
Internet  172.27.190.66         242   ec1d.8b2b.cf3a  ARPA   Vlan301
Internet  172.27.190.67          71   ec1d.8b2b.1312  ARPA   Vlan301
Internet  172.27.190.68          14   ec1d.8b2b.d611  ARPA   Vlan301
Internet  172.27.190.69          22   ec1d.8b2b.d16f  ARPA   Vlan301
Internet  172.27.190.70         126   ec1d.8b2b.d1b3  ARPA   Vlan301
Internet  172.27.190.72         112   ec1d.8b2b.06d5  ARPA   Vlan301
Internet  172.27.190.73         126   ec1d.8b2b.d202  ARPA   Vlan301
Internet  172.27.190.74          44   ec1d.8b2b.0139  ARPA   Vlan301
Internet  172.27.190.75         100   ac44.f219.e92e  ARPA   Vlan301
Internet  172.27.190.76         156   ec1d.8b2b.7b5a  ARPA   Vlan301
Internet  172.27.190.77          95   ec1d.8bba.4b64  ARPA   Vlan301
Internet  172.27.190.78          27   ec1d.8bba.5048  ARPA   Vlan301
Internet  172.27.190.79         187   ec1d.8b2b.043b  ARPA   Vlan301
Internet  172.27.190.80         114   0008.32ab.0a0d  ARPA   Vlan301
Internet  172.27.190.81          51   ec1d.8b2b.d018  ARPA   Vlan301
Internet  172.27.190.82         129   ec1d.8b2b.7069  ARPA   Vlan301
Internet  172.27.190.83          13   ec1d.8b2b.d0c6  ARPA   Vlan301
Internet  172.27.190.84         248   ec1d.8b2b.13e3  ARPA   Vlan301
Internet  172.27.190.85          43   ec1d.8b2b.0b6b  ARPA   Vlan301
Internet  172.27.190.86          31   ec1d.8b2b.065a  ARPA   Vlan301
Internet  172.27.190.87          33   ec1d.8bba.5186  ARPA   Vlan301
Internet  172.27.190.88           1   ec1d.8b2b.d502  ARPA   Vlan301
Internet  172.27.190.89         217   ec1d.8b2a.dc3c  ARPA   Vlan301
Internet  172.27.190.90           0   ec1d.8b2b.d847  ARPA   Vlan301
Internet  172.27.190.91          30   ec1d.8b2b.d460  ARPA   Vlan301
Internet  172.27.190.92          48   ec1d.8b2b.0be9  ARPA   Vlan301
Internet  172.27.190.93         215   ec1d.8b2b.d556  ARPA   Vlan301
Internet  172.27.190.94          82   ec1d.8b2b.6eed  ARPA   Vlan301
Internet  172.27.190.95         139   ec1d.8b2a.fb31  ARPA   Vlan301
Internet  172.27.190.96          98   ec1d.8b2b.7aa8  ARPA   Vlan301
Internet  172.27.190.97          30   ec1d.8b2b.d8a0  ARPA   Vlan301
Internet  172.27.190.98         151   ec1d.8b2b.d0a8  ARPA   Vlan301
Internet  172.27.190.99          92   ec1d.8b2b.d879  ARPA   Vlan301
Internet  172.27.190.100        157   ec1d.8b2b.d145  ARPA   Vlan301
Internet  172.27.190.101        236   ec1d.8b2b.d242  ARPA   Vlan301
Internet  172.27.190.102         70   ec1d.8b2b.d589  ARPA   Vlan301
Internet  172.27.190.103         13   ec1d.8b2b.122c  ARPA   Vlan301
Internet  172.27.190.104        257   ec1d.8b2b.18d7  ARPA   Vlan301
Internet  172.27.190.105        221   ec1d.8b2b.12a1  ARPA   Vlan301
Internet  172.27.190.106        168   ec1d.8b2b.d23d  ARPA   Vlan301
Internet  172.27.190.107          0   ec1d.8b2a.f66e  ARPA   Vlan301
Internet  172.27.190.108          3   0008.32ab.11bd  ARPA   Vlan301
Internet  172.27.190.109        225   ac44.f219.eb98  ARPA   Vlan301
Internet  172.27.190.110        104   ec1d.8b2b.7ad8  ARPA   Vlan301
Internet  172.27.190.111         14   ec1d.8b2b.1411  ARPA   Vlan301
Internet  172.27.190.113         92   ec1d.8b2b.7aa3  ARPA   Vlan301
Internet  172.27.190.114          4   ec1d.8b2b.d14c  ARPA   Vlan301
Internet  172.27.190.116         32   ec1d.8b2b.79e3  ARPA   Vlan301
Internet  172.27.190.117         39   ec1d.8b2b.cf71  ARPA   Vlan301
Internet  172.27.190.119        162   ec1d.8b2b.d8a5  ARPA   Vlan301
Internet  172.27.190.120         91   ec1d.8b2a.dcb2  ARPA   Vlan301
Internet  172.27.190.121         24   ec1d.8b2b.10cd  ARPA   Vlan301
Internet  172.27.190.122        146   ec1d.8b2b.7ab3  ARPA   Vlan301
Internet  172.27.190.123          8   0008.32aa.ca2c  ARPA   Vlan301
Internet  172.27.190.124        103   ec1d.8b2b.7460  ARPA   Vlan301
Internet  172.27.190.125          0   ec1d.8b2b.d28d  ARPA   Vlan301
Internet  172.27.190.126         42   ec1d.8b2b.d13a  ARPA   Vlan301
Internet  172.27.190.127         27   ec1d.8b2b.d6b4  ARPA   Vlan301
Internet  172.27.190.128         76   ec1d.8b2b.d420  ARPA   Vlan301
Internet  172.27.190.129          1   0008.32ab.ac37  ARPA   Vlan301
Internet  172.27.190.130         45   ec1d.8b2b.d85f  ARPA   Vlan301
Internet  172.27.190.131         48   ec1d.8b2b.7a0f  ARPA   Vlan301
Internet  172.27.190.132          2   ec1d.8b2b.1c71  ARPA   Vlan301
Internet  172.27.190.133         60   ec1d.8b2b.7a26  ARPA   Vlan301
Internet  172.27.190.135          4   ec1d.8b2b.0adf  ARPA   Vlan301
Internet  172.27.190.137        111   ec1d.8b2b.d591  ARPA   Vlan301
Internet  172.27.190.139         39   0027.902b.75d2  ARPA   Vlan301
Internet  172.27.190.140        211   ec1d.8b2b.138d  ARPA   Vlan301
Internet  172.27.190.141         37   ec1d.8b2b.10af  ARPA   Vlan301
Internet  172.27.190.143         27   ec1d.8b2b.d2c2  ARPA   Vlan301
Internet  172.27.190.145         18   ec1d.8b2b.d16e  ARPA   Vlan301
Internet  172.27.190.146        126   ec1d.8b2b.cfb0  ARPA   Vlan301
Internet  172.27.190.147         55   ac44.f219.f9cf  ARPA   Vlan301
Internet  172.27.190.149        146   ec1d.8b2b.75e2  ARPA   Vlan301
Internet  172.27.190.150        187   ec1d.8b2b.cf3e  ARPA   Vlan301
Internet  172.27.190.151          0   dceb.941d.8df1  ARPA   Vlan301
Internet  172.27.190.152        126   ec1d.8b2b.d8e1  ARPA   Vlan301
Internet  172.27.190.153         29   00d6.fe04.17d6  ARPA   Vlan301
Internet  172.27.190.154          5   ec1d.8b2a.dddc  ARPA   Vlan301
Internet  172.27.190.155         23   0008.32aa.c3b4  ARPA   Vlan301
Internet  172.27.190.156         76   ec1d.8b2b.d01a  ARPA   Vlan301
Internet  172.27.190.158         51   0008.32ab.0bc0  ARPA   Vlan301
Internet  172.27.190.159          0   dceb.941d.8dc2  ARPA   Vlan301
Internet  172.27.190.160         29   ec1d.8b2b.0668  ARPA   Vlan301
Internet  172.27.190.162        105   00b8.b33a.66a8  ARPA   Vlan301
Internet  172.27.190.163        142   ec1d.8b2b.cf42  ARPA   Vlan301
Internet  172.27.190.165         77   ec1d.8b2b.d1b7  ARPA   Vlan301
Internet  172.27.190.166        131   ec1d.8b2b.d431  ARPA   Vlan301
Internet  172.27.190.167         16   ec1d.8b2b.d2df  ARPA   Vlan301
Internet  172.27.190.169        142   ec1d.8b2b.d149  ARPA   Vlan301
Internet  172.27.190.170         48   ec1d.8b2b.6dbc  ARPA   Vlan301
Internet  172.27.190.171        179   ec1d.8b2b.760c  ARPA   Vlan301
Internet  172.27.190.172        121   ec1d.8b2b.d516  ARPA   Vlan301
Internet  172.27.190.173         82   ec1d.8b2b.d180  ARPA   Vlan301
Internet  172.27.190.174         25   ec1d.8b2b.d8cb  ARPA   Vlan301
Internet  172.27.190.175        118   ec1d.8b2b.d2bc  ARPA   Vlan301
Internet  172.27.190.176         98   ec1d.8b2b.d40f  ARPA   Vlan301
Internet  172.27.190.177        125   ec1d.8b2b.0431  ARPA   Vlan301
Internet  172.27.190.178         16   ec1d.8b2b.d6c4  ARPA   Vlan301
Internet  172.27.190.179         18   0008.32aa.cbc7  ARPA   Vlan301
Internet  172.27.190.180         17   ec1d.8b2b.d9d1  ARPA   Vlan301
Internet  172.27.190.185        179   ac44.f219.ea5e  ARPA   Vlan301
Internet  172.27.190.187         32   ec1d.8b2b.1311  ARPA   Vlan301
Internet  172.27.190.188          2   ec1d.8b2b.74ae  ARPA   Vlan301
Internet  172.27.190.189        215   ec1d.8b2a.fab7  ARPA   Vlan301
Internet  172.27.190.190        153   ec1d.8b2b.0be4  ARPA   Vlan301
Internet  172.27.190.191         23   ec1d.8b2b.142f  ARPA   Vlan301
Internet  172.27.190.192         46   ec1d.8b2b.d1d1  ARPA   Vlan301
Internet  172.27.190.195        218   ec1d.8b2b.065e  ARPA   Vlan301
Internet  172.27.190.196         56   ec1d.8b2b.d557  ARPA   Vlan301
Internet  172.27.190.197        212   ec1d.8b2b.d20f  ARPA   Vlan301
Internet  172.27.190.198         85   ec1d.8b2b.d144  ARPA   Vlan301
Internet  172.27.190.199          7   0008.32aa.c078  ARPA   Vlan301
Internet  172.27.190.200         82   ec1d.8b2b.7ab5  ARPA   Vlan301
Internet  172.27.190.201         21   ec1d.8b2b.cfdd  ARPA   Vlan301
Internet  172.27.190.202         59   ec1d.8b2b.73c7  ARPA   Vlan301
Internet  172.27.190.203         54   ec1d.8b2b.d00f  ARPA   Vlan301
Internet  172.27.190.204         36   ec1d.8b2b.758c  ARPA   Vlan301
Internet  172.27.190.205         58   ec1d.8b2b.d27f  ARPA   Vlan301
Internet  172.27.190.206        162   ec1d.8b2b.1aa6  ARPA   Vlan301
Internet  172.27.190.207         21   0008.32aa.c1d1  ARPA   Vlan301
Internet  172.27.190.208         49   ec1d.8b2b.74b3  ARPA   Vlan301
Internet  172.27.190.209        127   ec1d.8b2b.7a53  ARPA   Vlan301
Internet  172.27.190.210         30   ec1d.8b2b.d141  ARPA   Vlan301
Internet  172.27.190.211        121   ec1d.8b2b.cf38  ARPA   Vlan301
Internet  172.27.190.214         82   ec1d.8b2b.6dbd  ARPA   Vlan301
Internet  172.27.190.215         39   ec1d.8b2b.e733  ARPA   Vlan301
Internet  172.27.190.216         23   ec1d.8b2b.7995  ARPA   Vlan301
Internet  172.27.190.217         11   ec1d.8b2b.10b1  ARPA   Vlan301
Internet  172.27.190.218        155   ec1d.8b2b.e72f  ARPA   Vlan301
Internet  172.27.190.219        205   ec1d.8b2a.6dcc  ARPA   Vlan301
Internet  172.27.190.220         40   ec1d.8b2b.0c36  ARPA   Vlan301
Internet  172.27.190.222         94   ec1d.8b2b.12fb  ARPA   Vlan301
Internet  172.27.190.223         48   ec1d.8b2b.109d  ARPA   Vlan301
Internet  172.27.190.224         34   0008.32aa.fc78  ARPA   Vlan301
Internet  172.27.190.225        149   ec1d.8b2b.7a69  ARPA   Vlan301
Internet  172.27.190.226        109   ec1d.8b2b.d4bf  ARPA   Vlan301
Internet  172.27.190.229        103   ec1d.8b2b.11bc  ARPA   Vlan301
Internet  172.27.190.231         12   ec1d.8b2b.799a  ARPA   Vlan301
Internet  172.27.190.232         97   ec1d.8b2b.d1fd  ARPA   Vlan301
Internet  172.27.190.233        209   ec1d.8b2b.03c9  ARPA   Vlan301
Internet  172.27.190.235         79   ec1d.8b2b.d2c9  ARPA   Vlan301
Internet  172.27.190.242        122   ec1d.8b2a.c1be  ARPA   Vlan301
Internet  172.27.190.243         54   ec1d.8b2b.7abe  ARPA   Vlan301
Internet  172.27.190.245         20   ec1d.8b2b.142e  ARPA   Vlan301
Internet  172.27.190.247        114   ec1d.8b2b.70cd  ARPA   Vlan301
Internet  172.27.190.248         40   0008.32aa.8e14  ARPA   Vlan301
Internet  172.27.190.249         28   ec1d.8b2b.1245  ARPA   Vlan301
Internet  172.27.190.250        145   ec1d.8b2b.0ebe  ARPA   Vlan301
Internet  172.27.190.251         83   ec1d.8b2b.d148  ARPA   Vlan301
Internet  172.27.190.252          0   dceb.941d.8c02  ARPA   Vlan301
Internet  172.27.190.254        249   ec1d.8b2b.d740  ARPA   Vlan301
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
 All    0100.0ccc.cccc    STATIC      CPU
 All    0100.0ccc.cccd    STATIC      CPU
 All    0180.c200.0000    STATIC      CPU
 All    0180.c200.0001    STATIC      CPU
 All    0180.c200.0002    STATIC      CPU
 All    0180.c200.0003    STATIC      CPU
 All    0180.c200.0004    STATIC      CPU
 All    0180.c200.0005    STATIC      CPU
 All    0180.c200.0006    STATIC      CPU
 All    0180.c200.0007    STATIC      CPU
 All    0180.c200.0008    STATIC      CPU
 All    0180.c200.0009    STATIC      CPU
 All    0180.c200.000a    STATIC      CPU
 All    0180.c200.000b    STATIC      CPU
 All    0180.c200.000c    STATIC      CPU
 All    0180.c200.000d    STATIC      CPU
 All    0180.c200.000e    STATIC      CPU
 All    0180.c200.000f    STATIC      CPU
 All    0180.c200.0010    STATIC      CPU
 All    0180.c200.0021    STATIC      CPU
 All    ffff.ffff.ffff    STATIC      CPU
   1    001d.9cd8.6f13    DYNAMIC     Te1/0/3
   1    0023.24ef.75ec    DYNAMIC     Te1/0/3
   1    0041.d24f.8899    DYNAMIC     Po14
   1    00a5.bf52.3624    DYNAMIC     Te1/0/14
   1    00b1.e35a.cb47    STATIC      Vl1 
   1    00cc.fc9e.f099    DYNAMIC     Te1/0/8
   1    247e.1205.a8b1    DYNAMIC     Te2/0/3
   1    247e.120f.14b1    DYNAMIC     Te1/0/1
   1    247e.1210.4119    DYNAMIC     Po16
   1    247e.1210.411a    DYNAMIC     Po16
   1    247e.1211.43b1    DYNAMIC     Te1/0/3
   1    247e.1211.beb1    DYNAMIC     Te1/0/6
   1    247e.1211.c631    DYNAMIC     Te2/0/4
   1    247e.127b.dc99    DYNAMIC     Te2/0/5
   1    247e.12db.3a19    DYNAMIC     Te1/0/5
   1    247e.12db.4f19    DYNAMIC     Te1/0/7
   1    247e.12db.5a19    DYNAMIC     Te1/1/6
   1    247e.12db.7619    DYNAMIC     Te2/0/6
   1    5006.ab33.a231    DYNAMIC     Te1/0/4
   1    501c.b047.cd99    DYNAMIC     Te2/0/1
   1    6cdd.3069.1934    DYNAMIC     Po14
   1    b4a8.b9da.7ab1    DYNAMIC     Po1
   1    dcf7.19e1.cba4    DYNAMIC     Te2/0/14
   1    f454.33ab.7cfa    DYNAMIC     Te1/0/3
   2    b4a8.b9da.7ab1    DYNAMIC     Po1
   3    00a5.bf52.3624    DYNAMIC     Te1/0/14
   3    00a5.bf52.3667    DYNAMIC     Te1/0/14
   3    00a7.4293.53a4    DYNAMIC     Te2/0/4
   3    00b1.e35a.cb67    STATIC      Vl3 
   3    247e.1205.a8b1    DYNAMIC     Te2/0/3
   3    247e.1205.a8c3    DYNAMIC     Te2/0/3
   3    247e.120f.14b1    DYNAMIC     Te1/0/1
   3    247e.120f.14c1    DYNAMIC     Te1/0/1
   3    247e.1211.43b1    DYNAMIC     Te1/0/3
   3    247e.1211.43c1    DYNAMIC     Te1/0/3
   3    247e.1211.beb1    DYNAMIC     Te1/0/6
   3    247e.1211.bec1    DYNAMIC     Te1/0/6
   3    247e.1211.c631    DYNAMIC     Te2/0/4
   3    247e.1211.c641    DYNAMIC     Te2/0/4
   3    247e.127b.dc99    DYNAMIC     Te2/0/5
   3    247e.127b.dcc1    DYNAMIC     Te2/0/5
   3    247e.12db.3a19    DYNAMIC     Te1/0/5
   3    247e.12db.3a41    DYNAMIC     Te1/0/5
   3    247e.12db.4f19    DYNAMIC     Te1/0/7
   3    247e.12db.4f41    DYNAMIC     Te1/0/7
   3    247e.12db.5a19    DYNAMIC     Te1/1/6
   3    247e.12db.5a41    DYNAMIC     Te1/1/6
   3    247e.12db.7619    DYNAMIC     Te2/0/6
   3    247e.12db.7641    DYNAMIC     Te2/0/6
   3    380e.4d66.21d0    DYNAMIC     Te1/0/6
   3    380e.4d66.4c6c    DYNAMIC     Po1
   3    380e.4d66.4c80    DYNAMIC     Te1/0/1
   3    380e.4d66.4c84    DYNAMIC     Po1
   3    5006.ab33.a231    DYNAMIC     Te1/0/4
   3    5006.ab33.a243    DYNAMIC     Te1/0/4
   3    501c.b047.cd99    DYNAMIC     Te2/0/1
   3    501c.b047.cdc3    DYNAMIC     Te2/0/1
   3    707d.b9c3.a4e8    DYNAMIC     Te1/0/1
   3    b4a8.b9da.7ac3    DYNAMIC     Po1
   3    c81f.66c2.86cb    DYNAMIC     Te2/0/8
   3    d067.e58e.c2f8    DYNAMIC     Te2/0/8
   3    dcf7.19e1.cba4    DYNAMIC     Te2/0/14
   3    f8bc.124b.4b02    DYNAMIC     Te2/0/8
   3    f8bc.124b.7662    DYNAMIC     Te2/0/8
   4    0016.6cb2.12ff    DYNAMIC     Te1/0/1
   4    0041.d24f.8899    DYNAMIC     Po14
   4    0041.d24f.88c1    DYNAMIC     Po14
   4    00a7.42cc.0824    DYNAMIC     Po14
   4    00b1.e35a.cb57    STATIC      Vl4 
   4    00cc.fc9e.f099    DYNAMIC     Te1/0/8
   4    00cc.fc9e.f0c1    DYNAMIC     Te1/0/8
   4    247e.1210.4119    DYNAMIC     Po16
   4    247e.1210.411a    DYNAMIC     Po16
   4    247e.1210.4142    DYNAMIC     Po16
   4    2cab.eb4d.e3c1    DYNAMIC     Te1/0/3
   4    380e.4d66.4c5c    DYNAMIC     Te1/0/3
   4    380e.4d66.4d24    DYNAMIC     Te2/0/4
   4    380e.4d66.4f34    DYNAMIC     Te1/0/3
   4    3890.a5d0.bf28    DYNAMIC     Po16
   4    6cdd.3069.1934    DYNAMIC     Po14
   4    707d.b969.de9c    DYNAMIC     Po14
   4    707d.b9c3.a524    DYNAMIC     Te1/0/3
   4    743e.2b10.fa40    DYNAMIC     Po16
   4    743e.2b11.3390    DYNAMIC     Po16
   4    c4b9.cdbb.2441    DYNAMIC     Po16
 101    0002.d109.d704    DYNAMIC     Po1
 101    0002.d10c.e9bb    DYNAMIC     Po1
 101    0002.d110.6500    DYNAMIC     Po1
 101    0002.d115.e778    DYNAMIC     Po1
 101    000e.c6b6.79f6    DYNAMIC     Te1/0/1
 101    0011.3243.d093    DYNAMIC     Po1
 101    0015.6da0.708f    DYNAMIC     Po1
 101    0015.6dda.6f6d    DYNAMIC     Po1
 101    0015.6dda.6f7c    DYNAMIC     Po1
 101    0015.6ddc.8f78    DYNAMIC     Po1
 101    0015.6ddc.c711    DYNAMIC     Po1
 101    0015.6df2.372a    DYNAMIC     Po1
 101    0017.fc10.cc0a    DYNAMIC     Te2/0/4
 101    0017.fc10.cc0b    DYNAMIC     Te2/0/4
 101    0017.fc10.cc4e    DYNAMIC     Po1
 101    0017.fc10.cc51    DYNAMIC     Po1
 101    0017.fc3e.03ce    DYNAMIC     Te2/0/4
 101    0021.6afd.4b8e    DYNAMIC     Po1
 101    0021.6afd.4ddc    DYNAMIC     Po1
 101    0021.6afd.4dff    DYNAMIC     Po1
 101    0021.6afd.b59c    DYNAMIC     Po1
 101    0021.b767.9072    DYNAMIC     Te1/0/6
 101    0023.15e6.7ac1    DYNAMIC     Po1
 101    0023.15e6.7c0b    DYNAMIC     Po1
 101    0023.15e6.7c6a    DYNAMIC     Te1/0/6
 101    0023.15f4.c61c    DYNAMIC     Te1/0/6
 101    0023.15f4.c62b    DYNAMIC     Te1/0/6
 101    0023.15f4.ca4a    DYNAMIC     Te1/0/6
 101    0023.15f4.ca77    DYNAMIC     Po1
 101    0023.15f4.ca9f    DYNAMIC     Te2/0/4
 101    0023.15f5.263e    DYNAMIC     Te2/0/4
 101    0023.15f6.c9f3    DYNAMIC     Po1
 101    0027.22a6.1522    DYNAMIC     Po1
 101    0027.22ac.012d    DYNAMIC     Po1
 101    0027.22f2.fd17    DYNAMIC     Po1
 101    0050.56ad.0853    DYNAMIC     Te1/1/3
 101    0050.56ad.0888    DYNAMIC     Te2/1/3
 101    0050.56ad.1b0d    DYNAMIC     Te2/1/3
 101    0050.56ad.24d5    DYNAMIC     Te1/1/3
 101    0050.56ad.26cb    DYNAMIC     Te2/1/3
 101    0050.56ad.3cb2    DYNAMIC     Te1/1/3
 101    0050.56ad.52ea    DYNAMIC     Te1/1/3
 101    0050.56ad.64d1    DYNAMIC     Te2/1/3
 101    0050.56ad.70d4    DYNAMIC     Te1/1/3
 101    00b1.e35a.cb41    STATIC      Vl101 
 101    0418.d692.29d3    DYNAMIC     Po1
 101    1002.b5b5.7368    DYNAMIC     Te1/0/1
 101    1062.e50b.e911    DYNAMIC     Po1
 101    1065.30f2.8205    DYNAMIC     Po1
 101    1065.30f3.1c94    DYNAMIC     Te1/0/4
 101    1065.30f3.1e40    DYNAMIC     Te2/0/4
 101    1065.30f3.23fe    DYNAMIC     Po1
 101    1065.30f5.c1f5    DYNAMIC     Te2/0/4
 101    1065.30f5.c1f6    DYNAMIC     Po1
 101    1065.30f5.c1fc    DYNAMIC     Te1/0/6
 101    1065.30f5.c250    DYNAMIC     Te2/0/3
 101    1065.30f6.185b    DYNAMIC     Po1
 101    1065.30f6.1b1a    DYNAMIC     Te2/0/4
 101    1065.30f6.1b2d    DYNAMIC     Po1
 101    1065.30f6.1b30    DYNAMIC     Te2/0/3
 101    1065.30f6.1b3a    DYNAMIC     Po1
 101    1065.30f6.1b3f    DYNAMIC     Po1
 101    1065.30f6.1be4    DYNAMIC     Te2/0/4
 101    1065.30f6.1bf9    DYNAMIC     Po1
 101    1065.30f6.a095    DYNAMIC     Te1/0/4
 101    1065.30f6.a22b    DYNAMIC     Po1
 101    1065.30f6.a26c    DYNAMIC     Te2/0/4
 101    14ab.c547.15e4    DYNAMIC     Te2/0/4
 101    1803.7336.3e95    DYNAMIC     Te1/0/7
 101    1803.7337.dce1    DYNAMIC     Te1/0/3
 101    1803.7337.dd49    DYNAMIC     Te1/0/7
 101    2047.47d1.9507    DYNAMIC     Te1/0/1
 101    247e.120f.14b1    DYNAMIC     Te1/0/1
 101    24a4.3c7e.ea1e    DYNAMIC     Po1
 101    2816.ad51.f0f9    DYNAMIC     Po1
 101    2816.ad52.436a    DYNAMIC     Po1
 101    44d9.e768.9b99    DYNAMIC     Po1
 101    4c34.8836.9174    DYNAMIC     Te1/0/1
 101    4c34.8836.9192    DYNAMIC     Te1/0/1
 101    588a.5a01.787e    DYNAMIC     Te2/0/4
 101    588a.5a06.2d9f    DYNAMIC     Te1/0/1
 101    588a.5a0e.afcb    DYNAMIC     Po1
 101    60f6.772a.f2ec    DYNAMIC     Po1
 101    60f6.772a.f337    DYNAMIC     Po1
 101    60f6.772a.f341    DYNAMIC     Po14
 101    64db.8b6a.ca17    DYNAMIC     Po1
 101    6872.510e.2dee    DYNAMIC     Po1
 101    6c2b.59d9.5c37    DYNAMIC     Te2/0/4
 101    78f8.82d0.50a0    DYNAMIC     Po1
 101    8000.0b81.7261    DYNAMIC     Te1/0/1
 101    8019.34e2.efe1    DYNAMIC     Te1/0/1
 101    802a.a808.b2c1    DYNAMIC     Po1
 101    a434.d972.015a    DYNAMIC     Po1
 101    a434.d972.06c8    DYNAMIC     Po14
 101    a44c.c824.5f62    DYNAMIC     Te1/0/3
 101    a44c.c870.5e48    DYNAMIC     Te1/0/4
 101    a44c.c870.5e76    DYNAMIC     Te1/0/1
 101    a44c.c870.5e9b    DYNAMIC     Te1/0/6
 101    a44c.c870.5eae    DYNAMIC     Po1
 101    a44c.c870.600a    DYNAMIC     Po1
 101    a44c.c870.bff3    DYNAMIC     Te2/0/4
 101    a44c.c870.bff4    DYNAMIC     Te2/0/3
 101    a44c.c870.bff7    DYNAMIC     Te2/0/4
 101    a44c.c870.bff9    DYNAMIC     Po1
 101    a44c.c870.c013    DYNAMIC     Po1
 101    a44c.c871.5fd2    DYNAMIC     Po1
 101    a44c.c871.600e    DYNAMIC     Po1
 101    a44c.c880.2cc3    DYNAMIC     Po1
 101    b8ae.ed9e.52da    DYNAMIC     Po1
 101    d077.14cc.a960    DYNAMIC     Po1
 101    d094.663d.9de6    DYNAMIC     Te1/0/1
 101    d425.8bea.2e9b    DYNAMIC     Te1/0/6
 101    d425.8bea.4912    DYNAMIC     Te1/0/6
 101    d425.8bea.4944    DYNAMIC     Te1/0/3
 101    d425.8bea.4976    DYNAMIC     Te2/0/4
 101    d425.8bea.4980    DYNAMIC     Te1/0/6
 101    d425.8bea.4ea3    DYNAMIC     Te1/0/3
 101    d425.8bea.4eee    DYNAMIC     Po1
 101    d425.8bea.4ef8    DYNAMIC     Te1/0/1
 101    d425.8beb.ea74    DYNAMIC     Po1
 101    d425.8beb.eb41    DYNAMIC     Po1
 101    d425.8beb.eb5f    DYNAMIC     Po1
 101    d425.8beb.ebf5    DYNAMIC     Po1
 101    d425.8beb.ec3b    DYNAMIC     Te2/0/4
 101    d425.8beb.ec59    DYNAMIC     Po1
 101    d425.8beb.ec86    DYNAMIC     Te1/0/1
 101    d425.8beb.ec95    DYNAMIC     Te1/0/3
 101    d425.8beb.ece5    DYNAMIC     Po1
 101    d425.8beb.ecea    DYNAMIC     Po1
 101    d46a.6ad9.8e85    DYNAMIC     Te1/0/1
 101    d89e.f31b.bcef    DYNAMIC     Te1/0/1
 101    d89e.f31b.c3f6    DYNAMIC     Te2/0/3
 101    d89e.f31b.d38a    DYNAMIC     Te2/0/3
 101    d89e.f31b.d5b6    DYNAMIC     Te1/0/3
 101    d89e.f31b.d8c5    DYNAMIC     Te1/0/5
 101    d89e.f31b.d8ce    DYNAMIC     Po1
 101    d89e.f31b.d903    DYNAMIC     Te1/0/1
 101    d89e.f31b.d9ae    DYNAMIC     Te2/0/3
 101    d89e.f31b.d9c0    DYNAMIC     Po1
 101    d89e.f31b.dc8b    DYNAMIC     Te1/0/6
 101    d89e.f31b.dcde    DYNAMIC     Te1/0/3
 101    d89e.f31b.ddc1    DYNAMIC     Te2/0/3
 101    d89e.f31b.de36    DYNAMIC     Te1/0/3
 101    d89e.f31b.de50    DYNAMIC     Te1/0/4
 101    d89e.f31b.de63    DYNAMIC     Po1
 101    d89e.f31b.de6c    DYNAMIC     Po14
 101    d89e.f31b.df6f    DYNAMIC     Te1/0/3
 101    d89e.f31b.dfd8    DYNAMIC     Te2/0/3
 101    d89e.f31b.dfdb    DYNAMIC     Po1
 101    d89e.f31b.e49d    DYNAMIC     Po14
 101    d89e.f31b.e9f9    DYNAMIC     Te2/0/3
 101    d89e.f31b.ea9d    DYNAMIC     Te1/0/4
 101    d89e.f31b.eb91    DYNAMIC     Po1
 101    d89e.f31b.eb95    DYNAMIC     Te1/1/6
 101    d89e.f31b.ed66    DYNAMIC     Po1
 101    d89e.f31b.edba    DYNAMIC     Po1
 101    d89e.f31b.ede8    DYNAMIC     Te2/0/3
 101    d89e.f31b.ee04    DYNAMIC     Po16
 101    d89e.f31b.eed7    DYNAMIC     Po1
 101    d89e.f31b.ef6e    DYNAMIC     Te1/0/1
 101    d89e.f31b.efa9    DYNAMIC     Te2/0/3
 101    d89e.f31b.effa    DYNAMIC     Te1/0/3
 101    d89e.f31b.f224    DYNAMIC     Te1/0/3
 101    d89e.f31b.f27c    DYNAMIC     Te2/0/3
 101    d89e.f31b.f303    DYNAMIC     Te2/0/4
 101    d89e.f31b.f316    DYNAMIC     Te2/0/3
 101    d89e.f31b.f3d4    DYNAMIC     Po14
 101    d89e.f31b.f3fe    DYNAMIC     Te1/0/3
 101    d89e.f31b.f4b0    DYNAMIC     Te1/0/3
 101    d89e.f31b.f598    DYNAMIC     Te2/0/4
 101    d89e.f31b.f810    DYNAMIC     Po1
 101    d89e.f31b.f8ca    DYNAMIC     Te1/0/1
 101    d89e.f31b.fb15    DYNAMIC     Te1/0/8
 101    d89e.f31f.bf56    DYNAMIC     Te1/0/3
 101    d89e.f31f.db02    DYNAMIC     Te1/0/4
 101    d89e.f31f.db0e    DYNAMIC     Te1/0/6
 101    d89e.f31f.f957    DYNAMIC     Po14
 101    d89e.f320.0797    DYNAMIC     Po1
 101    d89e.f321.f181    DYNAMIC     Po1
 101    d89e.f321.f1e7    DYNAMIC     Te1/0/3
 101    d89e.f321.f251    DYNAMIC     Po1
 101    d89e.f321.f2bb    DYNAMIC     Te1/0/3
 101    d89e.f321.f60d    DYNAMIC     Po1
 101    d89e.f321.fdd6    DYNAMIC     Te1/0/3
 101    d89e.f321.fea0    DYNAMIC     Po1
 101    d89e.f321.fed7    DYNAMIC     Po1
 101    d89e.f322.0102    DYNAMIC     Te1/0/3
 101    d89e.f322.0207    DYNAMIC     Po1
 101    dc9f.dba6.604b    DYNAMIC     Po1
 101    e4b9.7a32.cabe    DYNAMIC     Te1/0/1
 101    f09f.c24c.2dd2    DYNAMIC     Po1
 101    f09f.c25c.3a9d    DYNAMIC     Po1
 101    f859.7144.a41c    DYNAMIC     Te1/0/1
 102    0000.749c.47bf    DYNAMIC     Te2/0/4
 102    0008.32aa.c0bd    DYNAMIC     Te2/0/4
 102    0008.32aa.fde6    DYNAMIC     Te2/0/4
 102    0016.6c73.8685    DYNAMIC     Te1/0/1
 102    0021.b707.2b00    DYNAMIC     Te1/0/6
 102    0021.b707.6bd6    DYNAMIC     Po1
 102    0021.b707.cb47    DYNAMIC     Te1/0/3
 102    0021.b72d.30fb    DYNAMIC     Po1
 102    0021.b747.3a41    DYNAMIC     Po1
 102    0021.b747.51bc    DYNAMIC     Te2/0/4
 102    0021.b747.5abf    DYNAMIC     Po14
 102    0021.b747.7a94    DYNAMIC     Po1
 102    0021.b747.913a    DYNAMIC     Te2/0/4
 102    0021.b747.bab8    DYNAMIC     Te2/0/4
 102    0021.b747.dac0    DYNAMIC     Te1/0/3
 102    0021.b747.dac2    DYNAMIC     Te2/0/4
 102    0021.b767.603d    DYNAMIC     Po1
 102    0021.b767.a06c    DYNAMIC     Te1/0/5
 102    0021.b767.a08e    DYNAMIC     Te2/0/4
 102    0021.b767.a091    DYNAMIC     Te1/0/3
 102    0021.b767.a0c9    DYNAMIC     Te2/0/3
 102    0021.b767.d071    DYNAMIC     Te2/0/4
 102    0021.b767.e0bb    DYNAMIC     Te1/0/3
 102    0021.b7ab.1e3c    DYNAMIC     Po1
 102    0021.b7ab.1e56    DYNAMIC     Te2/0/3
 102    0021.b7ab.2e33    DYNAMIC     Po1
 102    0021.b7ab.2ee3    DYNAMIC     Te1/0/3
 102    0021.b7ab.b255    DYNAMIC     Te1/0/4
 102    0021.b7ab.ee43    DYNAMIC     Te1/0/6
 102    0021.b7ab.eed5    DYNAMIC     Po1
 102    0025.b31e.0420    DYNAMIC     Te1/0/14
 102    0026.7341.3e85    DYNAMIC     Po1
 102    0026.7341.3f3c    DYNAMIC     Po1
 102    0026.7341.69b7    DYNAMIC     Te1/0/4
 102    0026.739e.59e2    DYNAMIC     Te1/0/8
 102    0026.73aa.52e3    DYNAMIC     Te1/0/6
 102    0026.73ae.87f1    DYNAMIC     Po16
 102    0030.56a2.e6e8    DYNAMIC     Te2/0/1
 102    0030.56a2.e726    DYNAMIC     Po1
 102    0030.56a2.e79d    DYNAMIC     Te2/0/1
 102    0050.56ad.2708    DYNAMIC     Te2/1/3
 102    0050.56ad.28ee    DYNAMIC     Te1/1/3
 102    0050.56ad.3dda    DYNAMIC     Te2/1/3
 102    0050.56ad.6110    DYNAMIC     Te1/1/3
 102    0050.56ad.6179    DYNAMIC     Te1/1/3
 102    0050.56ad.6787    DYNAMIC     Te2/1/3
 102    00b1.e35a.cb4d    STATIC      Vl102 
 102    a44c.c870.bfee    DYNAMIC     Po1
 102    b0e8.9280.e52b    DYNAMIC     Te1/0/3
 102    d094.66fe.a9b7    DYNAMIC     Po1
 102    d89e.f31b.d8c3    DYNAMIC     Te1/0/6
 102    d89e.f31b.eba7    DYNAMIC     Te1/0/6
 102    d89e.f31f.af02    DYNAMIC     Te2/0/1
 102    d89e.f320.0828    DYNAMIC     Te2/0/1
 102    d89e.f320.1857    DYNAMIC     Po1
 102    ec1d.8b2a.f109    DYNAMIC     Te2/0/3
 102    ec1d.8b2b.d14b    DYNAMIC     Te2/0/3
 102    ec1d.8b2b.d2b2    DYNAMIC     Te2/0/4
 103    0021.b797.168e    DYNAMIC     Te1/0/1
 103    00b1.e35a.cb5d    STATIC      Vl103 
 301    0008.32aa.8e14    DYNAMIC     Te1/0/1
 301    0008.32aa.c078    DYNAMIC     Te2/0/3
 301    0008.32aa.c1d1    DYNAMIC     Te1/0/3
 301    0008.32aa.c3b4    DYNAMIC     Te2/0/4
 301    0008.32aa.ca2c    DYNAMIC     Po1
 301    0008.32aa.cbc7    DYNAMIC     Te1/0/6
 301    0008.32aa.f39f    DYNAMIC     Po1
 301    0008.32aa.f6f3    DYNAMIC     Po1
 301    0008.32aa.fc78    DYNAMIC     Te1/0/3
 301    0008.32aa.fdce    DYNAMIC     Te2/0/4
 301    0008.32ab.014c    DYNAMIC     Po1
 301    0008.32ab.0a0d    DYNAMIC     Po1
 301    0008.32ab.0bc0    DYNAMIC     Te2/0/4
 301    0008.32ab.0ce3    DYNAMIC     Po1
 301    0008.32ab.0e21    DYNAMIC     Po1
 301    0008.32ab.11bd    DYNAMIC     Po1
 301    0008.32ab.ac37    DYNAMIC     Po1
 301    0027.902b.75d2    DYNAMIC     Po1
 301    0045.1dab.aa90    DYNAMIC     Te1/0/14
 301    0045.1dab.b030    DYNAMIC     Te1/0/14
 301    00b1.e35a.cb62    STATIC      Vl301 
 301    00b8.b33a.66a8    DYNAMIC     Te1/0/6
 301    00d6.fe04.17d6    DYNAMIC     Te1/0/6
 301    ac44.f219.e92e    DYNAMIC     Te2/0/4
 301    ac44.f219.ea5e    DYNAMIC     Te2/0/4
 301    ac44.f219.eb98    DYNAMIC     Po1
 301    ac44.f219.f9cf    DYNAMIC     Te1/0/3
 301    dceb.941d.8c02    DYNAMIC     Te1/0/3
 301    dceb.941d.8dc2    DYNAMIC     Te2/0/4
 301    dceb.941d.8df1    DYNAMIC     Te1/0/6
 301    ec1d.8b2a.4fef    DYNAMIC     Po1
 301    ec1d.8b2a.6dcc    DYNAMIC     Te1/0/7
 301    ec1d.8b2a.c1be    DYNAMIC     Po1
 301    ec1d.8b2a.dc3c    DYNAMIC     Te2/0/4
 301    ec1d.8b2a.dcb2    DYNAMIC     Po1
 301    ec1d.8b2a.dddc    DYNAMIC     Te1/0/1
 301    ec1d.8b2a.f66e    DYNAMIC     Po14
 301    ec1d.8b2a.fab7    DYNAMIC     Te2/0/3
 301    ec1d.8b2a.fb31    DYNAMIC     Po1
 301    ec1d.8b2b.0139    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.03c9    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.0431    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.043b    DYNAMIC     Po1
 301    ec1d.8b2b.0451    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.065a    DYNAMIC     Po1
 301    ec1d.8b2b.065e    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.0668    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.06c2    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.06d5    DYNAMIC     Po1
 301    ec1d.8b2b.087b    DYNAMIC     Po1
 301    ec1d.8b2b.0adf    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.0b6b    DYNAMIC     Po1
 301    ec1d.8b2b.0be4    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.0be9    DYNAMIC     Po1
 301    ec1d.8b2b.0c36    DYNAMIC     Te1/0/4
 301    ec1d.8b2b.0d0e    DYNAMIC     Po1
 301    ec1d.8b2b.0ebe    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.109d    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.10a1    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.10af    DYNAMIC     Po16
 301    ec1d.8b2b.10b1    DYNAMIC     Te1/0/4
 301    ec1d.8b2b.10cb    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.10cd    DYNAMIC     Po1
 301    ec1d.8b2b.11ba    DYNAMIC     Po1
 301    ec1d.8b2b.11bc    DYNAMIC     Po1
 301    ec1d.8b2b.11bf    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.122c    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.1245    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.125a    DYNAMIC     Po16
 301    ec1d.8b2b.12a1    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.12fb    DYNAMIC     Te1/0/5
 301    ec1d.8b2b.1311    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.1312    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.138d    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.13e3    DYNAMIC     Po1
 301    ec1d.8b2b.1411    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.142e    DYNAMIC     Po1
 301    ec1d.8b2b.142f    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.148d    DYNAMIC     Po1
 301    ec1d.8b2b.18d7    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.1aa6    DYNAMIC     Po1
 301    ec1d.8b2b.1c71    DYNAMIC     Po1
 301    ec1d.8b2b.1d69    DYNAMIC     Po1
 301    ec1d.8b2b.6dbc    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.6dbd    DYNAMIC     Te1/0/4
 301    ec1d.8b2b.6eed    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.6ef2    DYNAMIC     Po1
 301    ec1d.8b2b.7069    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.7088    DYNAMIC     Po1
 301    ec1d.8b2b.70cd    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.710c    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.73c7    DYNAMIC     Po1
 301    ec1d.8b2b.745f    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.7460    DYNAMIC     Po1
 301    ec1d.8b2b.74ae    DYNAMIC     Te2/0/6
 301    ec1d.8b2b.74b3    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.74ed    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.758c    DYNAMIC     Po1
 301    ec1d.8b2b.75e2    DYNAMIC     Po1
 301    ec1d.8b2b.760c    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.7995    DYNAMIC     Te1/0/4
 301    ec1d.8b2b.799a    DYNAMIC     Te1/0/5
 301    ec1d.8b2b.79e3    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.79e8    DYNAMIC     Po1
 301    ec1d.8b2b.7a0c    DYNAMIC     Po1
 301    ec1d.8b2b.7a0e    DYNAMIC     Po1
 301    ec1d.8b2b.7a0f    DYNAMIC     Po1
 301    ec1d.8b2b.7a26    DYNAMIC     Po1
 301    ec1d.8b2b.7a53    DYNAMIC     Po1
 301    ec1d.8b2b.7a68    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.7a69    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.7a6f    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.7a8b    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.7aa2    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.7aa3    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.7aa8    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.7ab0    DYNAMIC     Po1
 301    ec1d.8b2b.7ab3    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.7ab5    DYNAMIC     Te1/1/6
 301    ec1d.8b2b.7abe    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.7ad8    DYNAMIC     Po1
 301    ec1d.8b2b.7b5a    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.cf38    DYNAMIC     Po1
 301    ec1d.8b2b.cf3a    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.cf3e    DYNAMIC     Po1
 301    ec1d.8b2b.cf42    DYNAMIC     Po1
 301    ec1d.8b2b.cf71    DYNAMIC     Po1
 301    ec1d.8b2b.cfb0    DYNAMIC     Te2/0/1
 301    ec1d.8b2b.cfdd    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d00f    DYNAMIC     Po1
 301    ec1d.8b2b.d018    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d01a    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d067    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d0a8    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d0c6    DYNAMIC     Po1
 301    ec1d.8b2b.d13a    DYNAMIC     Po14
 301    ec1d.8b2b.d141    DYNAMIC     Te1/0/4
 301    ec1d.8b2b.d144    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.d145    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d148    DYNAMIC     Te2/0/6
 301    ec1d.8b2b.d149    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d14c    DYNAMIC     Po1
 301    ec1d.8b2b.d16e    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d16f    DYNAMIC     Po1
 301    ec1d.8b2b.d180    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d1b3    DYNAMIC     Po1
 301    ec1d.8b2b.d1b4    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.d1b6    DYNAMIC     Po1
 301    ec1d.8b2b.d1b7    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d1d1    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.d1d2    DYNAMIC     Po1
 301    ec1d.8b2b.d1d5    DYNAMIC     Po1
 301    ec1d.8b2b.d1f9    DYNAMIC     Po1
 301    ec1d.8b2b.d1fd    DYNAMIC     Po1
 301    ec1d.8b2b.d202    DYNAMIC     Po1
 301    ec1d.8b2b.d20f    DYNAMIC     Po14
 301    ec1d.8b2b.d23d    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.d242    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d27f    DYNAMIC     Po1
 301    ec1d.8b2b.d28d    DYNAMIC     Po1
 301    ec1d.8b2b.d2bc    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d2c2    DYNAMIC     Po1
 301    ec1d.8b2b.d2c9    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d2df    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d40f    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d420    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d431    DYNAMIC     Po1
 301    ec1d.8b2b.d460    DYNAMIC     Po1
 301    ec1d.8b2b.d4bf    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d502    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d516    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d556    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d557    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d589    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d58a    DYNAMIC     Po1
 301    ec1d.8b2b.d591    DYNAMIC     Te1/0/1
 301    ec1d.8b2b.d5c4    DYNAMIC     Po14
 301    ec1d.8b2b.d611    DYNAMIC     Te2/0/5
 301    ec1d.8b2b.d6b4    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d6c4    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d727    DYNAMIC     Po1
 301    ec1d.8b2b.d740    DYNAMIC     Po1
 301    ec1d.8b2b.d847    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d85f    DYNAMIC     Po1
 301    ec1d.8b2b.d879    DYNAMIC     Te1/0/3
 301    ec1d.8b2b.d882    DYNAMIC     Po1
 301    ec1d.8b2b.d8a0    DYNAMIC     Po1
 301    ec1d.8b2b.d8a5    DYNAMIC     Po14
 301    ec1d.8b2b.d8cb    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d8e0    DYNAMIC     Po1
 301    ec1d.8b2b.d8e1    DYNAMIC     Te1/0/6
 301    ec1d.8b2b.d90a    DYNAMIC     Te2/0/4
 301    ec1d.8b2b.d9d1    DYNAMIC     Te2/0/3
 301    ec1d.8b2b.e72f    DYNAMIC     Po16
 301    ec1d.8b2b.e733    DYNAMIC     Te1/0/4
 301    ec1d.8bba.4b64    DYNAMIC     Te1/0/1
 301    ec1d.8bba.5048    DYNAMIC     Te2/0/4
 301    ec1d.8bba.50d2    DYNAMIC     Po1
 301    ec1d.8bba.5186    DYNAMIC     Te1/0/1
 401    000c.be02.da58    DYNAMIC     Po1
 401    0017.fc3e.03db    DYNAMIC     Po1
 401    0030.56a1.5bbf    DYNAMIC     Po1
 401    00b1.e35a.cb6f    STATIC      Vl401 
 401    90ac.3f06.6c4f    DYNAMIC     Po1
 401    90ac.3f06.6c56    DYNAMIC     Te1/0/1
 401    90ac.3f06.6c59    DYNAMIC     Te2/0/1
 401    90ac.3f06.6c67    DYNAMIC     Te2/0/3
 401    90ac.3f06.6c6b    DYNAMIC     Te1/0/1
 401    90ac.3f06.6dcb    DYNAMIC     Te1/0/4
 401    90ac.3f06.6e81    DYNAMIC     Te2/0/4
 401    90ac.3f06.6e8b    DYNAMIC     Po1
 401    90ac.3f06.6ea6    DYNAMIC     Po1
 401    d89e.f31f.da92    DYNAMIC     Po1
 403    00b1.e35a.cb6f    STATIC      Vl403 
 405    00b1.e35a.cb6f    STATIC      Vl405 
 406    00b1.e35a.cb72    STATIC      Vl406 
 411    00b1.e35a.cb6e    STATIC      Vl411 
 412    00b1.e35a.cb5b    STATIC      Vl412 
 413    0016.6c9a.37ef    DYNAMIC     Po14
 413    0016.6cb2.12fe    DYNAMIC     Te1/0/14
 413    0016.6cc7.6a25    DYNAMIC     Te1/0/1
 413    00b1.e35a.cb4f    STATIC      Vl413 
 413    2857.becf.b662    DYNAMIC     Po14
 413    2857.becf.b6a6    DYNAMIC     Po14
 413    2857.becf.b6ab    DYNAMIC     Te1/0/8
 413    2857.becf.b712    DYNAMIC     Po14
 413    2857.becf.b729    DYNAMIC     Te1/0/8
 413    2857.becf.b72c    DYNAMIC     Po14
 413    2857.becf.b730    DYNAMIC     Te1/0/8
 413    2857.becf.b737    DYNAMIC     Te1/0/8
 413    94e1.ac33.127a    DYNAMIC     Po14
 413    94e1.accc.d685    DYNAMIC     Po16
 413    94e1.accc.d6bd    DYNAMIC     Po16
 413    94e1.accc.d6de    DYNAMIC     Po16
 413    94e1.accc.d6ed    DYNAMIC     Po16
 413    94e1.accc.d701    DYNAMIC     Po16
 413    94e1.accc.d765    DYNAMIC     Po16
 777    00b1.e35a.cb7b    STATIC      Vl777 
 802    001e.6783.cb83    DYNAMIC     Te1/0/14
 802    001e.67c4.fb75    DYNAMIC     Te1/1/5
 802    001e.67c5.5b6c    DYNAMIC     Po3
 802    001e.67c5.5b6d    DYNAMIC     Po3
 802    0050.56ad.0b88    DYNAMIC     Te2/1/3
 802    0050.56ad.678b    DYNAMIC     Te2/1/3
 802    0050.56ad.7333    DYNAMIC     Te1/1/3
 802    0050.56ad.7408    DYNAMIC     Te2/1/3
 802    00b1.e35a.cb70    STATIC      Vl802 
 802    40f2.e9f4.b354    DYNAMIC     Te1/0/14
 802    40f2.e9f4.b355    DYNAMIC     Te1/0/14
 802    40f2.e9f4.c90c    DYNAMIC     Te1/0/14
 802    40f2.e9f4.c90d    DYNAMIC     Te1/0/14
 802    d094.663f.6c75    DYNAMIC     Te1/0/14
 402    1418.7733.c186    DYNAMIC     Po1
 402    6400.6a53.ad00    DYNAMIC     Po1
 402    6400.6a54.1b96    DYNAMIC     Po1
 901    00b1.e35a.cb48    STATIC      Vl901 
 901    64f6.9d0b.ab91    DYNAMIC     Te1/0/14
 901    a44c.1170.6701    DYNAMIC     Te1/0/14
 901    c067.afc5.74e1    DYNAMIC     Te1/0/14
1102    000a.f7be.5c1b    DYNAMIC     Te1/0/14
1102    000c.2988.f02c    DYNAMIC     Te1/0/14
1102    000c.29e1.b881    DYNAMIC     Te1/0/14
1102    0010.18dc.1830    DYNAMIC     Te1/1/2
1102    0010.18dc.1832    DYNAMIC     Te2/1/1
1102    001b.217e.78c0    DYNAMIC     Te1/0/12
1102    001b.217e.78c2    DYNAMIC     Te1/0/12
1102    001b.218e.a7a0    DYNAMIC     Te2/0/10
1102    001b.218e.a7a1    DYNAMIC     Te1/0/10
1102    0026.7344.44e1    DYNAMIC     Te1/0/14
1102    0050.56ad.1a34    DYNAMIC     Te1/1/3
1102    0050.56ad.1eae    DYNAMIC     Te2/1/3
1102    0050.56ad.21b5    DYNAMIC     Te1/1/3
1102    0050.56ad.458f    DYNAMIC     Te2/1/3
1102    0050.56ad.5067    DYNAMIC     Te2/1/3
1102    0050.56ad.68e2    DYNAMIC     Te2/1/3
1102    0050.56ad.6b7a    DYNAMIC     Te1/1/3
1102    0050.56ad.7fde    DYNAMIC     Te1/1/3
1102    00a5.bf52.3624    DYNAMIC     Te1/0/14
1102    00b1.e35a.cb61    STATIC      Vl1102 
1102    1098.36ae.5081    DYNAMIC     Te1/0/14
1102    782b.cb56.bfae    DYNAMIC     Te1/0/1
1102    8018.44e4.f150    DYNAMIC     Te1/0/14
1102    8018.44e4.f351    DYNAMIC     Te1/0/14
1102    90b1.1c05.6aea    DYNAMIC     Te1/0/14
1102    90b1.1c08.3a87    DYNAMIC     Te2/0/8
1102    90b1.1c49.f6c6    DYNAMIC     Te2/0/8
1102    d094.663d.9de5    DYNAMIC     Te1/0/14
Total Mac Addresses for this criterion: 646
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh inter status 

Port      Name               Status       Vlan       Duplex  Speed Type
Te1/0/1   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/2   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/3   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/4   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/5   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/6   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseLX SFP
Te1/0/7   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/0/8   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseLX SFP
Te1/0/9                      connected    trunk        full   1000 1000BaseSX SFP
Te1/0/10  M075-MediaSerBacku connected    1102         full    10G SFP-10GBase-SR
Te1/0/11  ESX03              notconnect   1            full    10G SFP-10GBase-SR
Te1/0/12  Nva_Consola_Respal connected    1102         full    10G SFP-10GBase-SR
Te1/0/13  ESX4               connected    trunk        full    10G SFP-10GBase-SR
Te1/0/14  Link to ACCESS-SWI connected    trunk        full    10G SFP-10GBase-SR
Te1/0/15  -Stack Virtual-    connected    4094         full    10G SFP-10GBase-SR
Te1/0/16  -Stack Virtual-    connected    4094         full    10G SFP-10GBase-SR
Te1/1/1   Netbackup_2-2      connected    802          full    10G SFP-10GBase-SR
Te1/1/2   CONSOLA DE RESPALD connected    1102         full    10G SFP-10GBase-SR
Te1/1/3   ESX5               connected    trunk        full    10G SFP-10GBase-SR
Te1/1/4   MX-LRS1-M050       notconnect   802          full    10G SFP-10GBase-SR
Te1/1/5   NetBackup_1-1      connected    802          full    10G SFP-10GBase-SR
Te1/1/6   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te1/1/7                      notconnect   1            auto   auto unknown
Te1/1/8                      notconnect   1            auto   auto unknown
Te2/0/1   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te2/0/2   Link to ACCESS-SWI disabled     1            full   1000 1000BaseSX SFP
Te2/0/3   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te2/0/4   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te2/0/5   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseLX SFP
Te2/0/6   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseSX SFP
Te2/0/7   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseLX SFP
Te2/0/8   PowerConnect_1     connected    trunk        full   1000 1000BaseSX SFP
Te2/0/9                      connected    trunk        full   1000 1000BaseSX SFP
Te2/0/10  M075-MediaSerBacku connected    1102         full    10G SFP-10GBase-SR
Te2/0/11  ESX03              connected    trunk        full    10G SFP-10GBase-SR
Te2/0/12  Tunnel DRP_2       notconnect   403          full  10000 SFP-10GBase-SR
Te2/0/13  ESX4_02            connected    trunk        full    10G SFP-10GBase-SR
Te2/0/14  Link to ACCESS-SWI connected    trunk        full    10G SFP-10GBase-SR
Te2/0/15                     connected    4094         full    10G SFP-10GBase-SR
Te2/0/16                     connected    4094         full    10G SFP-10GBase-SR
Te2/1/1   CONSOLA DE RESPALD connected    1102         full    10G SFP-10GBase-SR
Te2/1/2                      connected    802          full    10G SFP-10GBase-SR
Te2/1/3   ESX5_02            connected    trunk        full    10G SFP-10GBase-SR
Te2/1/4   MX-LRS1-M050       notconnect   802          full    10G SFP-10GBase-SR
Te2/1/5   NetBackup_1-2      connected    411          full    10G SFP-10GBase-SR
Te2/1/6   Link to ACCESS-SWI connected    trunk        full   1000 1000BaseLX SFP
Te2/1/7                      notconnect   1            auto   auto unknown
Te2/1/8                      notconnect   1            auto   auto unknown
Po1                          connected    trunk      a-full a-1000 N/A
Po3       Netbackup_2-2      connected    802        a-full  a-10G N/A
Po10      Port-CH TunnelDRP  notconnect   403          auto   auto N/A
Po14                         connected    trunk      a-full a-1000 N/A
Po16                         connected    trunk      a-full a-1000 N/A
Po20                         notconnect   unassigned   auto   auto N/A
Po21                         notconnect   unassigned   full   1000 N/A
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh inter des
Interface                      Status         Protocol Description
Vl1                            up             up       
Vl2                            admin down     down     
Vl3                            up             up       
Vl4                            up             up       
Vl77                           down           down     
Vl80                           down           down     
Vl101                          up             up       
Vl102                          up             up       
Vl103                          up             up       
Vl301                          up             up       
Vl401                          up             up       
Vl403                          up             up       
Vl405                          up             up       
Vl406                          up             up       
Vl411                          up             up       
Vl412                          up             up       
Vl413                          up             up       
Vl777                          up             up       
Vl801                          admin down     down     
Vl802                          up             up       
Vl803                          admin down     down     
Vl901                          up             up       Transit_To_Core
Vl1102                         up             up       
Vl1103                         down           down     
Gi0/0                          down           down     
Te1/0/1                        up             up       Link to ACCESS-SWITCH001
Te1/0/2                        up             up       Link to ACCESS-SWITCH011 Gi5/0/52
Te1/0/3                        up             up       Link to ACCESS-SWITCH031 Gi
Te1/0/4                        up             up       Link to ACCESS-SWITCH051 Gi
Te1/0/5                        up             up       Link to ACCESS-SWITCH071 Gi
Te1/0/6                        up             up       Link to ACCESS-SWITCH081 Gi
Te1/0/7                        up             up       Link to ACCESS-SWITCH101 Gi
Te1/0/8                        up             up       Link to ACCESS-SWITCH151 Gi
Te1/0/9                        up             up       
Te1/0/10                       up             up       M075-MediaSerBackup_1
Te1/0/11                       down           down     ESX03
Te1/0/12                       up             up       Nva_Consola_Respaldos
Te1/0/13                       up             up       ESX4
Te1/0/14                       up             up       Link to ACCESS-SWITCH931 Te1/1/8
Te1/0/15                       up             up       -Stack Virtual-
Te1/0/16                       up             up       -Stack Virtual-
Te1/1/1                        up             up       Netbackup_2-2
Te1/1/2                        up             up       CONSOLA DE RESPALDOS LRS
Te1/1/3                        up             up       ESX5
Te1/1/4                        down           down     MX-LRS1-M050
Te1/1/5                        up             up       NetBackup_1-1
Te1/1/6                        up             up       Link to ACCESS-SWITCH111
Te1/1/7                        down           down     
Te1/1/8                        down           down     
Fo1/1/1                        down           down     
Fo1/1/2                        down           down     
Te2/0/1                        up             up       Link to ACCESS-SWITCH021
Te2/0/2                        admin down     down     Link to ACCESS-SWITCH011
Te2/0/3                        up             up       Link to ACCESS-SWITCH041
Te2/0/4                        up             up       Link to ACCESS-SWITCH061
Te2/0/5                        up             up       Link to ACCESS-SWITCH081
Te2/0/6                        up             up       Link to ACCESS-SWITCH101
Te2/0/7                        up             up       Link to ACCESS-SWITCH141
Te2/0/8                        up             up       PowerConnect_1
Te2/0/9                        up             up       
Te2/0/10                       up             up       M075-MediaSerBackup_2
Te2/0/11                       up             up       ESX03
Te2/0/12                       down           down     Tunnel DRP_2
Te2/0/13                       up             up       ESX4_02
Te2/0/14                       up             up       Link to ACCESS-SWITCH931 Te2/1/8
Te2/0/15                       up             up       
Te2/0/16                       up             up       
Te2/1/1                        up             up       CONSOLA DE RESPALDOS LRS
Te2/1/2                        up             up       
Te2/1/3                        up             up       ESX5_02
Te2/1/4                        down           down     MX-LRS1-M050
Te2/1/5                        up             up       NetBackup_1-2
Te2/1/6                        up             up       Link to ACCESS-SWITCH141
Te2/1/7                        down           down     
Te2/1/8                        down           down     
Fo2/1/1                        down           down     
Fo2/1/2                        down           down     
Po1                            up             up       
Po3                            up             up       Netbackup_2-2
Po10                           down           down     Port-CH TunnelDRP
Po14                           up             up       
Po16                           up             up       
Po20                           down           down     
Po21                           down           down     
Tu403                          up             up       
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh inter
Vlan1 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb47 (bia 00b1.e35a.cb47)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:13, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     6015874 packets input, 2161369645 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan2 is administratively down, line protocol is down , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb77 (bia 00b1.e35a.cb77)
  Internet address is 172.27.172.1/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 16w0d, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     771009 packets input, 76178027 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     6894 packets output, 413640 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan3 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb67 (bia 00b1.e35a.cb67)
  Internet address is 172.27.188.129/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:13, output 00:01:17, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1054000 bits/sec, 232 packets/sec
  5 minute output rate 582000 bits/sec, 172 packets/sec
     87703190465 packets input, 126615029827383 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     15307380144 packets output, 2682569922126 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan4 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb57 (bia 00b1.e35a.cb57)
  Internet address is 172.27.188.193/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 133000 bits/sec, 61 packets/sec
  5 minute output rate 604000 bits/sec, 71 packets/sec
     380162095 packets input, 160796301286 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     358447508 packets output, 246172768607 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan77 is down, line protocol is down 
  Hardware is Ethernet SVI, address is 00b1.e35a.cb73 (bia 00b1.e35a.cb73)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan80 is down, line protocol is down 
  Hardware is Ethernet SVI, address is 00b1.e35a.cb4a (bia 00b1.e35a.cb4a)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan101 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb41 (bia 00b1.e35a.cb41)
  Internet address is 172.27.186.1/23
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 5/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/421464/10796 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5874000 bits/sec, 2025 packets/sec
  5 minute output rate 21032000 bits/sec, 2738 packets/sec
     10014385516 packets input, 4000068425533 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     13576484173 packets output, 12036318776554 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan102 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb4d (bia 00b1.e35a.cb4d)
  Internet address is 172.27.128.1/22
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 271000 bits/sec, 124 packets/sec
  5 minute output rate 1316000 bits/sec, 194 packets/sec
     3564126301 packets input, 1727459745190 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     2447234041 packets output, 1447104056417 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan103 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb5d (bia 00b1.e35a.cb5d)
  Internet address is 172.27.189.1/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:21, output 00:00:21, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1000 bits/sec, 1 packets/sec
  5 minute output rate 1000 bits/sec, 1 packets/sec
     10082016 packets input, 4152614346 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     11486324 packets output, 2878103721 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan301 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb62 (bia 00b1.e35a.cb62)
  Internet address is 172.27.190.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:03, output 00:00:03, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/9155/2156 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 861000 bits/sec, 595 packets/sec
  5 minute output rate 864000 bits/sec, 606 packets/sec
     1588341772 packets input, 257224333586 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     1593624218 packets output, 323220932828 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan401 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb6f (bia 00b1.e35a.cb6f)
  Internet address is 172.27.185.1/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:16, output 00:00:03, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 20000 bits/sec, 16 packets/sec
  5 minute output rate 47000 bits/sec, 14 packets/sec
     320078276 packets input, 92938394634 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     289641628 packets output, 179715822314 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan403 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb6f (bia 00b1.e35a.cb6f)
  Internet address is 172.27.184.129/25
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:07, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     46 packets input, 3366 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     1247717 packets output, 74863020 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan405 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb6f (bia 00b1.e35a.cb6f)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     44 packets input, 3168 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan406 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb72 (bia 00b1.e35a.cb72)
  Internet address is 172.27.134.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:02, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     44 packets input, 3168 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     309466 packets output, 18567960 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan411 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb6e (bia 00b1.e35a.cb6e)
  Internet address is 172.27.181.193/27
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 16w0d, output 12w6d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     6515 packets input, 516312 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     766 packets output, 45960 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan412 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb5b (bia 00b1.e35a.cb5b)
  Internet address is 172.27.181.225/27
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     44 packets input, 3168 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     2 packets output, 120 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan413 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb4f (bia 00b1.e35a.cb4f)
  Internet address is 172.27.189.65/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 2/255, rxload 6/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 24711000 bits/sec, 3748 packets/sec
  5 minute output rate 11586000 bits/sec, 2027 packets/sec
     29681999689 packets input, 25225707758730 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     17793027760 packets output, 13804046518515 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan777 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb7b (bia 00b1.e35a.cb7b)
  Internet address is 10.10.10.129/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 16w0d, output 2w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     2312480 packets input, 188524231 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     1239423 packets output, 102403331 bytes, 0 underruns
     0 output errors, 3 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan801 is administratively down, line protocol is down , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb7e (bia 00b1.e35a.cb7e)
  Internet address is 172.27.188.1/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 2w2d, output 2w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     3219070172 packets input, 3279552127729 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     811434336 packets output, 196463521827 bytes, 0 underruns
     0 output errors, 10 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan802 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb70 (bia 00b1.e35a.cb70)
  Internet address is 172.27.188.65/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:04, output 00:00:04, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 150000 bits/sec, 67 packets/sec
  5 minute output rate 138000 bits/sec, 64 packets/sec
     65807786861 packets input, 29020860114349 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     258260769135 packets output, 302503773051977 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan803 is administratively down, line protocol is down , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb57 (bia 00b1.e35a.cb57)
  Internet address is 172.27.172.65/27
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     44 packets input, 3168 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     25548 packets output, 1532880 bytes, 0 underruns
     0 output errors, 2 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan901 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb48 (bia 00b1.e35a.cb48)
  Description: Transit_To_Core
  Internet address is 172.27.189.129/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 3/255, rxload 4/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/44794/10 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 19212000 bits/sec, 4294 packets/sec
  5 minute output rate 12561000 bits/sec, 4083 packets/sec
     33070040241 packets input, 16593578661027 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     37534426619 packets output, 25985639285923 bytes, 0 underruns
     0 output errors, 1 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan1102 is up, line protocol is up , Autostate Enabled
  Hardware is Ethernet SVI, address is 00b1.e35a.cb61 (bia 00b1.e35a.cb61)
  Internet address is 172.27.188.1/26
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 3/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/93 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 14807000 bits/sec, 2239 packets/sec
  5 minute output rate 5107000 bits/sec, 1539 packets/sec
     253432565114 packets input, 261316246551121 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     128192724006 packets output, 96919394272812 bytes, 0 underruns
     0 output errors, 10 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
Vlan1103 is down, line protocol is down 
  Hardware is Ethernet SVI, address is 00b1.e35a.cb52 (bia 00b1.e35a.cb52)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
GigabitEthernet0/0 is down, line protocol is down 
  Hardware is RP management port, address is 00b1.e35a.cb00 (bia 00b1.e35a.cb00)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full Duplex, 1000Mbps, link type is auto, media type is RJ45
  output flow-control is unsupported, input flow-control is unsupported
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     9 packets input, 592 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     13 packets output, 1094 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
TenGigabitEthernet1/0/1 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb01 (bia 00b1.e35a.cb01)
  Description: Link to ACCESS-SWITCH001
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 3/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 74248
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1054000 bits/sec, 703 packets/sec
  5 minute output rate 12779000 bits/sec, 1519 packets/sec
     5383392179 packets input, 974965541768 bytes, 0 no buffer
     Received 61486680 broadcasts (43821845 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 43821845 multicast, 0 pause input
     0 input packets with dribble condition detected
     12353869020 packets output, 14791884457386 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     4 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/2 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb02 (bia 00b1.e35a.cb02)
  Description: Link to ACCESS-SWITCH011 Gi5/0/52
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 3/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:14, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 576874
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 2298000 bits/sec, 1022 packets/sec
  5 minute output rate 14206000 bits/sec, 1740 packets/sec
     4339041941 packets input, 1638720453168 bytes, 0 no buffer
     Received 250873753 broadcasts (126903433 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 126903433 multicast, 0 pause input
     0 input packets with dribble condition detected
     6828181743 packets output, 6031951590772 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/3 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb03 (bia 00b1.e35a.cb03)
  Description: Link to ACCESS-SWITCH031 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 12953
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 714000 bits/sec, 242 packets/sec
  5 minute output rate 1480000 bits/sec, 375 packets/sec
     1532656431 packets input, 549126989093 bytes, 0 no buffer
     Received 52254269 broadcasts (35858068 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 35858068 multicast, 0 pause input
     0 input packets with dribble condition detected
     2623857259 packets output, 1693806133640 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/4 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb04 (bia 00b1.e35a.cb04)
  Description: Link to ACCESS-SWITCH051 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:13, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 7048
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 98000 bits/sec, 43 packets/sec
  5 minute output rate 310000 bits/sec, 207 packets/sec
     413188968 packets input, 199033528496 bytes, 0 no buffer
     Received 15253948 broadcasts (11305859 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 11305859 multicast, 0 pause input
     0 input packets with dribble condition detected
     1273960020 packets output, 431381730620 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     4 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/5 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb05 (bia 00b1.e35a.cb05)
  Description: Link to ACCESS-SWITCH071 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:09, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 978
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 11000 bits/sec, 6 packets/sec
  5 minute output rate 146000 bits/sec, 165 packets/sec
     72476189 packets input, 20621895315 bytes, 0 no buffer
     Received 4485160 broadcasts (3482908 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 3482908 multicast, 0 pause input
     0 input packets with dribble condition detected
     895487689 packets output, 127306039674 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     2 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/6 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb06 (bia 00b1.e35a.cb06)
  Description: Link to ACCESS-SWITCH081 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseLX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:08, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 11402
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 278000 bits/sec, 113 packets/sec
  5 minute output rate 778000 bits/sec, 278 packets/sec
     743576321 packets input, 247302419881 bytes, 0 no buffer
     Received 20902455 broadcasts (15704587 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 15704587 multicast, 0 pause input
     0 input packets with dribble condition detected
     1691431457 packets output, 753086019607 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     4 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/7 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb07 (bia 00b1.e35a.cb07)
  Description: Link to ACCESS-SWITCH101 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:09, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 1094
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 17000 bits/sec, 8 packets/sec
  5 minute output rate 131000 bits/sec, 160 packets/sec
     75478799 packets input, 35949282748 bytes, 0 no buffer
     Received 4048149 broadcasts (3104828 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 3104828 multicast, 0 pause input
     0 input packets with dribble condition detected
     884456681 packets output, 124773362696 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/8 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb08 (bia 00b1.e35a.cb08)
  Description: Link to ACCESS-SWITCH151 Gi
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseLX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 536
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 2749000 bits/sec, 292 packets/sec
  5 minute output rate 287000 bits/sec, 453 packets/sec
     2476195256 packets input, 3012136488153 bytes, 0 no buffer
     Received 12937791 broadcasts (8875206 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 8875206 multicast, 0 pause input
     0 input packets with dribble condition detected
     3286495156 packets output, 276855391374 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/9 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb09 (bia 00b1.e35a.cb09)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:02, output 00:00:06, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 205
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5541000 bits/sec, 479 packets/sec
  5 minute output rate 306000 bits/sec, 501 packets/sec
     6098262307 packets input, 8879691480213 bytes, 0 no buffer
     Received 7066769 broadcasts (6749694 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 6749694 multicast, 0 pause input
     0 input packets with dribble condition detected
     4261307002 packets output, 425143372257 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/10 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0a (bia 00b1.e35a.cb0a)
  Description: M075-MediaSerBackup_1
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     1836392974 packets input, 254375331111 bytes, 0 no buffer
     Received 46892 broadcasts (8538 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 8538 multicast, 4455 pause input
     0 input packets with dribble condition detected
     47263993 packets output, 8629542169 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/11 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0b (bia 00b1.e35a.cb0b)
  Description: ESX03
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 5 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/12 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0c (bia 00b1.e35a.cb0c)
  Description: Nva_Consola_Respaldos
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:07, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 2286
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     16792538189 packets input, 9951926599594 bytes, 0 no buffer
     Received 1764731 broadcasts (1444973 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 1444973 multicast, 59428 pause input
     0 input packets with dribble condition detected
     503995197 packets output, 55618120684 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     330 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/13 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0d (bia 00b1.e35a.cb0d)
  Description: ESX4
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 103000 bits/sec, 136 packets/sec
     1655040091 packets input, 357475803354 bytes, 0 no buffer
     Received 24570699 broadcasts (15900440 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 15900440 multicast, 7966 pause input
     0 input packets with dribble condition detected
     10921288461 packets output, 6286841253050 bytes, 0 underruns
     0 output errors, 0 collisions, 9 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/14 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0e (bia 00b1.e35a.cb0e)
  Description: Link to ACCESS-SWITCH931 Te1/1/8
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 34829000 bits/sec, 7627 packets/sec
  5 minute output rate 29562000 bits/sec, 6633 packets/sec
     251204123508 packets input, 248042268047026 bytes, 0 no buffer
     Received 39612056 broadcasts (21049123 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 21049123 multicast, 0 pause input
     0 input packets with dribble condition detected
     92414728629 packets output, 47058141511554 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/15 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb0f (bia 00b1.e35a.cb0f)
  Description: -Stack Virtual-
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 2
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 12463000 bits/sec, 1545 packets/sec
  5 minute output rate 1695000 bits/sec, 533 packets/sec
     81075634091 packets input, 115497383566085 bytes, 0 no buffer
     Received 90689832 broadcasts (90689832 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 90689832 multicast, 0 pause input
     0 input packets with dribble condition detected
     40662906514 packets output, 25299000443345 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/0/16 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb10 (bia 00b1.e35a.cb10)
  Description: -Stack Virtual-
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 7861000 bits/sec, 1076 packets/sec
  5 minute output rate 5676000 bits/sec, 1820 packets/sec
     13781239012 packets input, 10053779548395 bytes, 0 no buffer
     Received 133450331 broadcasts (133450331 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 133450331 multicast, 0 pause input
     0 input packets with dribble condition detected
     20226315017 packets output, 14482809693018 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/1 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb11 (bia 00b1.e35a.cb11)
  Description: Netbackup_2-2
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:17, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 20000 bits/sec, 9 packets/sec
  5 minute output rate 27000 bits/sec, 20 packets/sec
     182265712 packets input, 57883485432 bytes, 0 no buffer
     Received 330018 broadcasts (330016 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 330016 multicast, 0 pause input
     0 input packets with dribble condition detected
     356067430 packets output, 59903899438 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/2 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb12 (bia 00b1.e35a.cb12)
  Description: CONSOLA DE RESPALDOS LRS
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:07, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5000 bits/sec, 2 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     6567149803 packets input, 4647200953835 bytes, 0 no buffer
     Received 1006982 broadcasts (331003 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 331003 multicast, 12 pause input
     0 input packets with dribble condition detected
     316331853 packets output, 37169490035 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     306 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/3 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb13 (bia 00b1.e35a.cb13)
  Description: ESX5
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:07, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/1 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 650000 bits/sec, 282 packets/sec
  5 minute output rate 2276000 bits/sec, 651 packets/sec
     3131688976 packets input, 1621122366217 bytes, 0 no buffer
     Received 19342965 broadcasts (8928674 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 8928674 multicast, 8126 pause input
     0 input packets with dribble condition detected
     12481306499 packets output, 6842636050494 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     305 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/4 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb14 (bia 00b1.e35a.cb14)
  Description: MX-LRS1-M050
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 7w5d, output 7w5d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1024641118 packets input, 602974051062 bytes, 0 no buffer
     Received 316713 broadcasts (172472 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 172472 multicast, 0 pause input
     0 input packets with dribble condition detected
     59587267 packets output, 6081703960 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     297 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/5 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb15 (bia 00b1.e35a.cb15)
  Description: NetBackup_1-1
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 13004
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 33000 bits/sec, 5 packets/sec
  5 minute output rate 5000 bits/sec, 4 packets/sec
     55317933402 packets input, 19674646116676 bytes, 0 no buffer
     Received 209180 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 7 pause input
     0 input packets with dribble condition detected
     251191128648 packets output, 299706325605086 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/6 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb16 (bia 00b1.e35a.cb16)
  Description: Link to ACCESS-SWITCH111
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 1994
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5000 bits/sec, 4 packets/sec
  5 minute output rate 131000 bits/sec, 160 packets/sec
     88119105 packets input, 19138941314 bytes, 0 no buffer
     Received 4435865 broadcasts (3342277 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 3342277 multicast, 0 pause input
     0 input packets with dribble condition detected
     923661150 packets output, 153356222287 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/7 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb17 (bia 00b1.e35a.cb17)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Auto-duplex, Auto-speed, link type is auto, media type is unknown
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet1/1/8 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.cb18 (bia 00b1.e35a.cb18)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Auto-duplex, Auto-speed, link type is auto, media type is unknown
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     1 packets output, 566 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
FortyGigabitEthernet1/1/1 is down, line protocol is down (notconnect) 
  Hardware is not present
  Hardware is Forty Gigabit Ethernet, address is 00b1.e35a.cb19 (bia 00b1.e35a.cb19)
  MTU 1500 bytes, BW 40000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
FortyGigabitEthernet1/1/2 is down, line protocol is down (notconnect) 
  Hardware is not present
  Hardware is Forty Gigabit Ethernet, address is 00b1.e35a.cb1a (bia 00b1.e35a.cb1a)
  MTU 1500 bytes, BW 40000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/1 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad01 (bia 00b1.e35a.ad01)
  Description: Link to ACCESS-SWITCH021
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:16, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 17000 bits/sec, 15 packets/sec
  5 minute output rate 207000 bits/sec, 170 packets/sec
     263218417 packets input, 91609358982 bytes, 0 no buffer
     Received 4164756 broadcasts (2793383 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 2793383 multicast, 0 pause input
     0 input packets with dribble condition detected
     1051360039 packets output, 193655042978 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     3 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/2 is administratively down, line protocol is down (disabled) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad02 (bia 00b1.e35a.ad02)
  Description: Link to ACCESS-SWITCH011
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 15w4d, output 15w4d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 105545
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     134057344 packets input, 33390544886 bytes, 0 no buffer
     Received 29931677 broadcasts (16270222 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 16270222 multicast, 0 pause input
     0 input packets with dribble condition detected
     37000507 packets output, 13554248711 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/3 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad03 (bia 00b1.e35a.ad03)
  Description: Link to ACCESS-SWITCH041
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:04, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 14082
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 204000 bits/sec, 96 packets/sec
  5 minute output rate 532000 bits/sec, 243 packets/sec
     804078355 packets input, 283656512010 bytes, 0 no buffer
     Received 41783231 broadcasts (30557465 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 30557465 multicast, 0 pause input
     0 input packets with dribble condition detected
     1683304650 packets output, 721831766567 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/4 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad04 (bia 00b1.e35a.ad04)
  Description: Link to ACCESS-SWITCH061
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:13, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 472703
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1702000 bits/sec, 416 packets/sec
  5 minute output rate 2723000 bits/sec, 592 packets/sec
     1867984126 packets input, 692023959694 bytes, 0 no buffer
     Received 67675543 broadcasts (50743374 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 50743374 multicast, 0 pause input
     0 input packets with dribble condition detected
     3076927219 packets output, 1833515159597 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     3 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/5 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad05 (bia 00b1.e35a.ad05)
  Description: Link to ACCESS-SWITCH081
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseLX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:26, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 152
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 115000 bits/sec, 155 packets/sec
     74770530 packets input, 11820837072 bytes, 0 no buffer
     Received 2226597 broadcasts (1785578 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 1785578 multicast, 0 pause input
     0 input packets with dribble condition detected
     881144921 packets output, 129486458114 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/6 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad06 (bia 00b1.e35a.ad06)
  Description: Link to ACCESS-SWITCH101
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 118000 bits/sec, 154 packets/sec
     18712451 packets input, 3347839803 bytes, 0 no buffer
     Received 941601 broadcasts (915918 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 915918 multicast, 0 pause input
     0 input packets with dribble condition detected
     809872666 packets output, 82533493765 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/7 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad07 (bia 00b1.e35a.ad07)
  Description: Link to ACCESS-SWITCH141
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseLX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:04, output 00:00:14, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 6
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 4451000 bits/sec, 405 packets/sec
  5 minute output rate 91000 bits/sec, 79 packets/sec
     2141724541 packets input, 2688263093609 bytes, 0 no buffer
     Received 19014410 broadcasts (14224178 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 14224178 multicast, 0 pause input
     0 input packets with dribble condition detected
     477907755 packets output, 109738194950 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     1 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/8 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad08 (bia 00b1.e35a.ad08)
  Description: PowerConnect_1
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 107000 bits/sec, 20 packets/sec
  5 minute output rate 15000 bits/sec, 12 packets/sec
     97130950560 packets input, 141484919499575 bytes, 0 no buffer
     Received 5870837 broadcasts (5612992 multicasts)
     0 runts, 1 giants, 0 throttles 
     1 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 5612992 multicast, 0 pause input
     0 input packets with dribble condition detected
     16071639151 packets output, 1939257958904 bytes, 0 underruns
     0 output errors, 0 collisions, 6 interface resets
     363 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/9 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad09 (bia 00b1.e35a.ad09)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseSX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:11, output 00:00:06, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 880
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5482000 bits/sec, 493 packets/sec
  5 minute output rate 30000 bits/sec, 37 packets/sec
     3333586580 packets input, 4365069750773 bytes, 0 no buffer
     Received 3239874 broadcasts (2439634 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 2439634 multicast, 0 pause input
     0 input packets with dribble condition detected
     264960483 packets output, 35607333986 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     2 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/10 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0a (bia 00b1.e35a.ad0a)
  Description: M075-MediaSerBackup_2
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 1435
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     1876901995 packets input, 259195237420 bytes, 0 no buffer
     Received 260507 broadcasts (6774 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 6774 multicast, 117249 pause input
     0 input packets with dribble condition detected
     46717259682 packets output, 68327266317851 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/11 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0b (bia 00b1.e35a.ad0b)
  Description: ESX03
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 6d21h, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 103000 bits/sec, 138 packets/sec
     2993774781 packets input, 2075472579884 bytes, 0 no buffer
     Received 15448883 broadcasts (5417499 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 5417499 multicast, 0 pause input
     0 input packets with dribble condition detected
     7314869699 packets output, 3578758723917 bytes, 0 underruns
     0 output errors, 0 collisions, 5 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/12 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0c (bia 00b1.e35a.ad0c)
  Description: Tunnel DRP_2
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 16w2d, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     483 packets input, 37766 bytes, 0 no buffer
     Received 483 broadcasts (78 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 78 multicast, 0 pause input
     0 input packets with dribble condition detected
     40 packets output, 6718 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/13 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0d (bia 00b1.e35a.ad0d)
  Description: ESX4_02
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:07, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 101000 bits/sec, 136 packets/sec
     7952678111 packets input, 6434833322542 bytes, 0 no buffer
     Received 20929409 broadcasts (13972867 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 13972867 multicast, 16909 pause input
     0 input packets with dribble condition detected
     7480315314 packets output, 2598375565716 bytes, 0 underruns
     0 output errors, 0 collisions, 6 interface resets
     319 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/14 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0e (bia 00b1.e35a.ad0e)
  Description: Link to ACCESS-SWITCH931 Te2/1/8
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 3000 bits/sec, 4 packets/sec
  5 minute output rate 112000 bits/sec, 148 packets/sec
     54017853 packets input, 5311262954 bytes, 0 no buffer
     Received 52416467 broadcasts (17872564 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 17872564 multicast, 0 pause input
     0 input packets with dribble condition detected
     775628849 packets output, 75037303535 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/15 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad0f (bia 00b1.e35a.ad0f)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 33
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1695000 bits/sec, 533 packets/sec
  5 minute output rate 12467000 bits/sec, 1544 packets/sec
     40662905149 packets input, 25299000137691 bytes, 0 no buffer
     Received 128674919 broadcasts (128674919 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 128674919 multicast, 0 pause input
     0 input packets with dribble condition detected
     81075629152 packets output, 115497378354677 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/0/16 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad10 (bia 00b1.e35a.ad10)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  Last input never, output 16w2d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5673000 bits/sec, 1823 packets/sec
  5 minute output rate 7878000 bits/sec, 1079 packets/sec
     20226309078 packets input, 14482806766264 bytes, 0 no buffer
     Received 71569428 broadcasts (71569428 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 71569428 multicast, 0 pause input
     0 input packets with dribble condition detected
     13781235681 packets output, 10053777044853 bytes, 0 underruns
     0 output errors, 0 collisions, 3 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/1 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad11 (bia 00b1.e35a.ad11)
  Description: CONSOLA DE RESPALDOS LRS
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:07, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1000 bits/sec, 1 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     204766960 packets input, 302476946991 bytes, 0 no buffer
     Received 368378 broadcasts (328229 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 328229 multicast, 132 pause input
     0 input packets with dribble condition detected
     64374058 packets output, 10825672777 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     308 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/2 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad12 (bia 00b1.e35a.ad12)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:26, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 25000 bits/sec, 9 packets/sec
  5 minute output rate 2000 bits/sec, 1 packets/sec
     191086833 packets input, 57039442832 bytes, 0 no buffer
     Received 330007 broadcasts (330007 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 330007 multicast, 0 pause input
     0 input packets with dribble condition detected
     18591384 packets output, 2405694312 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/3 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad13 (bia 00b1.e35a.ad13)
  Description: ESX5_02
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:18, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 1083000 bits/sec, 553 packets/sec
  5 minute output rate 2454000 bits/sec, 750 packets/sec
     6633662690 packets input, 3025486825862 bytes, 0 no buffer
     Received 25763202 broadcasts (14021868 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 14021868 multicast, 9246 pause input
     0 input packets with dribble condition detected
     9151636953 packets output, 4439233912106 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     299 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/4 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad14 (bia 00b1.e35a.ad14)
  Description: MX-LRS1-M050
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 7w5d, output 7w5d, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     144836138 packets input, 77963187884 bytes, 0 no buffer
     Received 229950 broadcasts (172836 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 172836 multicast, 0 pause input
     0 input packets with dribble condition detected
     26852241 packets output, 5170579854 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     294 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/5 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad15 (bia 00b1.e35a.ad15)
  Description: NetBackup_1-2
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 10Gb/s, link type is auto, media type is SFP-10GBase-SR
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     6499 packets input, 515400 bytes, 0 no buffer
     Received 6478 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     5669195 packets output, 528373528 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/6 is up, line protocol is up (connected) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad16 (bia 00b1.e35a.ad16)
  Description: Link to ACCESS-SWITCH141
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Full-duplex, 1000Mb/s, link type is auto, media type is 1000BaseLX SFP
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:08, output 00:00:20, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 4588
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 5792000 bits/sec, 537 packets/sec
  5 minute output rate 588000 bits/sec, 1005 packets/sec
     4187133111 packets input, 5629081187379 bytes, 0 no buffer
     Received 11754737 broadcasts (10082299 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 10082299 multicast, 0 pause input
     0 input packets with dribble condition detected
     6688351681 packets output, 585863467365 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     3 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/7 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad17 (bia 00b1.e35a.ad17)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Auto-duplex, Auto-speed, link type is auto, media type is unknown
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
TenGigabitEthernet2/1/8 is down, line protocol is down (notconnect) 
  Hardware is Ten Gigabit Ethernet, address is 00b1.e35a.ad18 (bia 00b1.e35a.ad18)
  MTU 1500 bytes, BW 10000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  Auto-duplex, Auto-speed, link type is auto, media type is unknown
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
FortyGigabitEthernet2/1/1 is down, line protocol is down (notconnect) 
  Hardware is Forty Gigabit Ethernet, address is 00b1.e35a.ad19 (bia 00b1.e35a.ad19)
  MTU 1500 bytes, BW 40000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
FortyGigabitEthernet2/1/2 is down, line protocol is down (notconnect) 
  Hardware is Forty Gigabit Ethernet, address is 00b1.e35a.ad1a (bia 00b1.e35a.ad1a)
  MTU 1500 bytes, BW 40000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel1 is up, line protocol is up (connected) 
  Hardware is EtherChannel, address is 00b1.e35a.cb02 (bia 00b1.e35a.cb02)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 3/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 1000Mb/s, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  Members in this channel: Te1/0/2 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 03:48:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 682419
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 2298000 bits/sec, 1023 packets/sec
  5 minute output rate 14206000 bits/sec, 1740 packets/sec
     4473099285 packets input, 1672110998054 bytes, 0 no buffer
     Received 280805430 broadcasts (143173655 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 143173655 multicast, 0 pause input
     0 input packets with dribble condition detected
     6865182250 packets output, 6045505839483 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel3 is up, line protocol is up (connected) 
  Hardware is EtherChannel, address is 00b1.e35a.cb11 (bia 00b1.e35a.cb11)
  Description: Netbackup_2-2
  MTU 1500 bytes, BW 20000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 10Gb/s, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  Members in this channel: Te1/1/1 Te2/1/2 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 47000 bits/sec, 20 packets/sec
  5 minute output rate 28000 bits/sec, 21 packets/sec
     373352545 packets input, 114922928264 bytes, 0 no buffer
     Received 660025 broadcasts (660023 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 660023 multicast, 0 pause input
     0 input packets with dribble condition detected
     374658814 packets output, 62309593750 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel10 is down, line protocol is down (notconnect) 
  Hardware is EtherChannel, address is 0000.0000.0000 (bia 0000.0000.0000)
  Description: Port-CH TunnelDRP
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Auto-duplex, Auto-speed, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel14 is up, line protocol is up (connected) 
  Hardware is EtherChannel, address is 00b1.e35a.ad07 (bia 00b1.e35a.ad07)
  MTU 1500 bytes, BW 2000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 1000Mb/s, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  Members in this channel: Te2/0/7 Te2/1/6 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 4594
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 10257000 bits/sec, 950 packets/sec
  5 minute output rate 689000 bits/sec, 1090 packets/sec
     6328857652 packets input, 8317344280988 bytes, 0 no buffer
     Received 30769147 broadcasts (24306477 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 24306477 multicast, 0 pause input
     0 input packets with dribble condition detected
     7166259436 packets output, 695601662315 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel16 is up, line protocol is up (connected) 
  Hardware is EtherChannel, address is 00b1.e35a.cb09 (bia 00b1.e35a.cb09)
  MTU 1500 bytes, BW 2000000 Kbit/sec, DLY 10 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 1000Mb/s, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  Members in this channel: Te1/0/9 Te2/0/9 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 1085
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 11028000 bits/sec, 974 packets/sec
  5 minute output rate 340000 bits/sec, 542 packets/sec
     9431848887 packets input, 13244761230986 bytes, 0 no buffer
     Received 10306643 broadcasts (9189328 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 9189328 multicast, 0 pause input
     0 input packets with dribble condition detected
     4526267485 packets output, 460750706243 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel20 is down, line protocol is down (notconnect) 
  Hardware is EtherChannel, address is 0000.0000.0000 (bia 0000.0000.0000)
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Auto-duplex, Auto-speed, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Port-channel21 is down, line protocol is down (notconnect) 
  Hardware is EtherChannel, address is 0000.0000.0000 (bia 0000.0000.0000)
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 1000Mb/s, link type is auto, media type is N/A
  input flow-control is on, output flow-control is unsupported 
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes, 0 no buffer
     Received 0 broadcasts (0 multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     0 packets output, 0 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
Tunnel403 is up, line protocol is up 
  Hardware is Tunnel
  Internet address is 172.31.254.1/24
  MTU 17868 bytes, BW 5120 Kbit/sec, DLY 50000 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 172.27.189.129, destination 172.27.157.2
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 15w5d, output 15w5d, output hang never
  Last clearing of "show interface" counters 16w2d
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1545604 packets input, 99132796 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     5 packets output, 620 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh inter transceiver
If device is externally calibrated, only calibrated values are printed.
++ : high alarm, +  : high warning, -  : low warning, -- : low alarm.
NA or N/A: not applicable, Tx: transmit, Rx: receive.
mA: milliamperes, dBm: decibels (milliwatts).

                                             Optical   Optical
             Temperature  Voltage  Current   Tx Power  Rx Power
Port         (Celsius)    (Volts)  (mA)      (dBm)     (dBm)
---------    -----------  -------  --------  --------  --------
Te1/0/1      24.5       3.29       7.5      -4.4      -4.6   
Te1/0/2      25.7       3.25       6.0      -4.4      -5.1   
Te1/0/3      25.5       3.29       6.7      -4.4      -5.3   
Te1/0/4      25.5       3.27       5.9      -4.4      -8.5   
Te1/0/5      25.6       3.26       6.2      -4.4      -6.5   
Te1/0/6      23.3       3.28      12.2      -7.1      -8.8   
Te1/0/7      25.7       3.26       7.0      -4.4      -7.3   
Te1/0/8      23.1       3.28      13.2      -7.0      -6.6   
Te1/0/9      26.2       3.28       6.4      -4.9      -7.6   
Te1/0/10     23.8       3.29       6.7      -2.2      -2.8   
Te1/0/11     23.4       3.29       6.7      -2.2     -29.6   
Te1/0/12     23.8       3.29       6.5      -2.1      -2.4   
Te1/0/13     24.3       3.28       6.9      -2.2      -3.1   
Te1/0/14     22.4       3.28       6.9      -2.2      -2.6   
Te1/0/15     23.9       3.28       6.1      -2.2      -2.0   
Te1/0/16     24.5       3.28       7.3      -2.2      -2.5   
Te1/1/1      23.7       3.28       6.5      -2.1      -3.0   
Te1/1/2      24.7       3.28       6.2      -2.3      -3.1   
Te1/1/3      23.2       3.28       6.0      -2.2      -2.9   
Te1/1/4      24.7       3.28       6.0      -2.2     -33.0   
Te1/1/5      23.2       3.24       5.8      -2.5      -3.1   
Te1/1/6      24.4       3.28       6.5      -4.4      -9.2   
Te2/0/1      24.2       3.27       6.4      -4.4      -5.7   
Te2/0/2      19.3       3.29       0.0       N/A       N/A   
Te2/0/3      24.1       3.28       5.9      -4.4      -5.9   
Te2/0/4      24.8       3.26       6.4      -4.4      -5.1   
Te2/0/5      25.0       3.30      12.6      -6.9     -14.7   
Te2/0/6      24.8       3.29       5.7      -4.4      -9.9   
Te2/0/7      22.0       3.30      11.8      -7.2      -4.6   
Te2/0/8      23.5       3.32       4.5      -5.7      -6.8   
Te2/0/9      24.4       3.30       4.1      -5.7     -10.4   
Te2/0/10     23.0       3.29       6.2      -2.2      -3.0   
Te2/0/11     23.4       3.29       7.1      -2.2      -3.6   
Te2/0/12     23.0       3.28       7.0      -2.1     -27.7   
Te2/0/13     24.0       3.29       6.1      -2.2      -2.9   
Te2/0/14     22.7       3.28       6.7      -2.2      -2.0   
Te2/0/15     22.7       3.28       6.5      -2.2      -2.3   
Te2/0/16     22.3       3.29       6.3      -2.2      -2.5   
Te2/1/1      23.3       3.27       5.9      -2.2      -2.7   
Te2/1/2      23.5       3.27       6.4      -2.5      -3.5   
Te2/1/3      23.2       3.27       7.4      -2.2      -3.8   
Te2/1/4      23.9       3.27       5.8      -2.0     -40.0   
Te2/1/5      25.5       3.25       7.7      -2.0      -2.2   
Te2/1/6      23.4       3.29      21.4      -5.9      -9.1   


MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh span

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    4097
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4097   (priority 4096 sys-id-ext 1)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0002
  Spanning tree enabled protocol rstp
  Root ID    Priority    4098
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4098   (priority 4096 sys-id-ext 2)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0003
  Spanning tree enabled protocol rstp
  Root ID    Priority    4099
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4099   (priority 4096 sys-id-ext 3)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0004
  Spanning tree enabled protocol rstp
  Root ID    Priority    4100
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4100   (priority 4096 sys-id-ext 4)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0060
  Spanning tree enabled protocol rstp
  Root ID    Priority    4156
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4156   (priority 4096 sys-id-ext 60)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0101
  Spanning tree enabled protocol rstp
  Root ID    Priority    4197
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4197   (priority 4096 sys-id-ext 101)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0102
  Spanning tree enabled protocol rstp
  Root ID    Priority    4198
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4198   (priority 4096 sys-id-ext 102)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0103
  Spanning tree enabled protocol rstp
  Root ID    Priority    4199
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4199   (priority 4096 sys-id-ext 103)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0301
  Spanning tree enabled protocol rstp
  Root ID    Priority    4397
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4397   (priority 4096 sys-id-ext 301)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0401
  Spanning tree enabled protocol rstp
  Root ID    Priority    4497
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4497   (priority 4096 sys-id-ext 401)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0402
  Spanning tree enabled protocol rstp
  Root ID    Priority    4498
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4498   (priority 4096 sys-id-ext 402)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0403
  Spanning tree enabled protocol rstp
  Root ID    Priority    4499
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4499   (priority 4096 sys-id-ext 403)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0404
  Spanning tree enabled protocol rstp
  Root ID    Priority    4500
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4500   (priority 4096 sys-id-ext 404)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0405
  Spanning tree enabled protocol rstp
  Root ID    Priority    4501
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4501   (priority 4096 sys-id-ext 405)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0406
  Spanning tree enabled protocol rstp
  Root ID    Priority    4502
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4502   (priority 4096 sys-id-ext 406)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0407
  Spanning tree enabled protocol rstp
  Root ID    Priority    4503
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4503   (priority 4096 sys-id-ext 407)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0408
  Spanning tree enabled protocol rstp
  Root ID    Priority    4504
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4504   (priority 4096 sys-id-ext 408)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0409
  Spanning tree enabled protocol rstp
  Root ID    Priority    4505
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4505   (priority 4096 sys-id-ext 409)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0410
  Spanning tree enabled protocol rstp
  Root ID    Priority    4506
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4506   (priority 4096 sys-id-ext 410)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0411
  Spanning tree enabled protocol rstp
  Root ID    Priority    4507
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4507   (priority 4096 sys-id-ext 411)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Te2/1/5             Desg FWD 2         128.117  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0412
  Spanning tree enabled protocol rstp
  Root ID    Priority    4508
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4508   (priority 4096 sys-id-ext 412)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0413
  Spanning tree enabled protocol rstp
  Root ID    Priority    4509
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4509   (priority 4096 sys-id-ext 413)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0777
  Spanning tree enabled protocol rstp
  Root ID    Priority    4873
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4873   (priority 4096 sys-id-ext 777)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0801
  Spanning tree enabled protocol rstp
  Root ID    Priority    4897
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4897   (priority 4096 sys-id-ext 801)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0802
  Spanning tree enabled protocol rstp
  Root ID    Priority    4898
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4898   (priority 4096 sys-id-ext 802)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/5             Desg FWD 2         128.21   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po3                 Desg FWD 1         128.2283 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0803
  Spanning tree enabled protocol rstp
  Root ID    Priority    4899
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4899   (priority 4096 sys-id-ext 803)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN0901
  Spanning tree enabled protocol rstp
  Root ID    Priority    4997
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4997   (priority 4096 sys-id-ext 901)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN1100
  Spanning tree enabled protocol rstp
  Root ID    Priority    33868
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    33868  (priority 32768 sys-id-ext 1100)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN1102
  Spanning tree enabled protocol rstp
  Root ID    Priority    33870
             Address     00a5.bf52.3600
             Cost        2
             Port        14 (TenGigabitEthernet1/0/14)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    33870  (priority 32768 sys-id-ext 1102)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/10            Desg FWD 2         128.10   P2p 
Te1/0/12            Desg FWD 2         128.12   P2p 
Te1/0/13            Desg FWD 2         128.13   P2p 
Te1/0/14            Root FWD 2         128.14   P2p 
Te1/1/2             Desg FWD 2         128.18   P2p 
Te1/1/3             Desg FWD 2         128.19   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/8             Desg FWD 4         128.104  P2p 
Te2/0/10            Desg FWD 2         128.106  P2p 
Te2/0/11            Desg FWD 2         128.107  P2p 
Te2/0/13            Desg FWD 2         128.109  P2p 
Te2/0/14            Altn BLK 2         128.110  P2p 
Te2/1/1             Desg FWD 2         128.113  P2p 
Te2/1/3             Desg FWD 2         128.115  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 



VLAN1203
  Spanning tree enabled protocol rstp
  Root ID    Priority    33971
             Address     00b1.e35a.cb00
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    33971  (priority 32768 sys-id-ext 1203)
             Address     00b1.e35a.cb00
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Te1/0/1             Desg FWD 4         128.1    P2p 
Te1/0/3             Desg FWD 4         128.3    P2p 
Te1/0/4             Desg FWD 4         128.4    P2p 
Te1/0/5             Desg FWD 4         128.5    P2p 
Te1/0/6             Desg FWD 4         128.6    P2p 
Te1/0/7             Desg FWD 4         128.7    P2p 
Te1/0/8             Desg FWD 4         128.8    P2p 
Te1/0/14            Desg FWD 2         128.14   P2p 
Te1/1/6             Desg FWD 4         128.22   P2p 
Te2/0/1             Desg FWD 4         128.97   P2p 
Te2/0/3             Desg FWD 4         128.99   P2p 
Te2/0/4             Desg FWD 4         128.100  P2p 
Te2/0/5             Desg FWD 4         128.101  P2p 
Te2/0/6             Desg FWD 4         128.102  P2p 
Te2/0/14            Desg FWD 2         128.110  P2p 
Po1                 Desg FWD 4         128.2281 P2p 
Po14                Desg FWD 3         128.2294 P2p 
Po16                Desg FWD 3         128.2296 P2p 


MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh span summary
Switch is in rapid-pvst mode
Root bridge for: VLAN0001-VLAN0004, VLAN0060, VLAN0101-VLAN0103, VLAN0301
  VLAN0401-VLAN0413, VLAN0777, VLAN0801-VLAN0803, VLAN0901, VLAN1100, VLAN120
EtherChannel misconfig guard            is enabled
Extended system ID                      is enabled
Portfast Default                        is disabled
PortFast BPDU Guard Default            is disabled
Portfast BPDU Filter Default           is disabled
Loopguard Default                      is disabled
UplinkFast                              is disabled
BackboneFast                            is disabled
Configured Pathcost method used is short

Name                   Blocking Listening Learning Forwarding STP Active
---------------------- -------- --------- -------- ---------- ----------
VLAN0001                     0         0        0         18         18
VLAN0002                     0         0        0         18         18
VLAN0003                     0         0        0         24         24
VLAN0004                     0         0        0         18         18
VLAN0060                     0         0        0         18         18
VLAN0101                     0         0        0         23         23
VLAN0102                     0         0        0         23         23
VLAN0103                     0         0        0         18         18
VLAN0301                     0         0        0         18         18
VLAN0401                     0         0        0         20         20
VLAN0402                     0         0        0         18         18
VLAN0403                     0         0        0         18         18
VLAN0404                     0         0        0         18         18
VLAN0405                     0         0        0         18         18
VLAN0406                     0         0        0         23         23
VLAN0407                     0         0        0         23         23
VLAN0408                     0         0        0         23         23
VLAN0409                     0         0        0         18         18
VLAN0410                     0         0        0         18         18
VLAN0411                     0         0        0         25         25
VLAN0412                     0         0        0         18         18
VLAN0413                     0         0        0         18         18
VLAN0777                     0         0        0         18         18
VLAN0801                     0         0        0         24         24
VLAN0802                     0         0        0         26         26
VLAN0803                     0         0        0         18         18
VLAN0901                     0         0        0         18         18
VLAN1100                     0         0        0         18         18
VLAN1102                     1         0        0         28         29
VLAN1203                     0         0        0         18         18
---------------------- -------- --------- -------- ---------- ----------
30 vlans                     1         0        0        604        605
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 172.27.189.134 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 172.27.189.134
      5.0.0.0/32 is subnetted, 2 subnets
S        5.39.123.4 [1/0] via 172.27.189.139
S        5.39.123.20 [1/0] via 172.27.189.139
      10.0.0.0/8 is variably subnetted, 603 subnets, 16 masks
O E2     10.0.176.0/20 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.1.17.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.1.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.1.195.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.2.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.10.0.0/17 [110/1] via 172.27.189.139, 1d22h, Vlan901
C        10.10.10.0/24 is directly connected, Vlan777
L        10.10.10.129/32 is directly connected, Vlan777
O E2     10.10.206.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.10.211.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.10.253.3/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     10.10.253.4/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     10.10.253.248/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     10.20.0.0/16 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.3/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.8/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.24/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.40/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.48/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.56/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.64/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.72/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.80/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.88/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.96/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.104/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.112/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.120/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.136/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.1.252/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.21.3.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.30.0.0/16 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.3/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.8/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.24/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.40/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.48/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.56/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.64/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.72/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.80/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.88/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.96/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.104/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.112/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.120/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.136/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.0.144/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.1.252/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.3.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.4.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.64.0/18 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.128.0/18 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.31.166.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.0.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.40.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.42.0/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.91.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.128.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.160.0/20 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.40.192.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.0.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.2.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.4.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.6.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.7.0/24 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.9.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.10.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.12.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.13.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.14.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.15.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.15.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.16.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.17.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.18.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.21.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.23.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.24.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.24.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.25.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.26.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.27.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.28.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.29.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.29.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.29.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.29.192/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.29.224/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.31.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.32.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.33.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.34.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.34.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.34.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.34.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.36.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.36.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.36.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.36.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.37.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.37.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.37.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.38.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.38.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.39.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.39.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.40.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.40.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.41.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.42.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.43.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.44.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.44.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.44.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.45.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.45.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.45.192/26 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.46.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.46.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.46.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.46.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.50.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.50.128/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.50.156/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.51.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.52.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.52.128/26 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.52.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.56.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.56.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.56.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.88.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.88.129/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.96.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.104.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.112.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.120.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.136.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.140.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.144.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.147.112/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.148.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.156.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.160.0/22 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.164.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.168.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.172.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.176.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.180.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.184.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.188.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.192.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.196.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.200.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.204.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.208.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.212.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.216.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.220.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.224.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.228.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.232.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.233.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.234.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.235.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.238.0/24 [110/1] via 172.27.189.139, 05:07:21, Vlan901
O E2     10.42.239.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.241.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.243.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.244.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.245.0/24 [110/1] via 172.27.189.139, 03:22:54, Vlan901
O E2     10.42.246.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.247.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.248.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.250.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.250.136/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.250.184/29 [110/1] via 172.27.189.139, 02:09:39, Vlan901
O E2     10.42.250.192/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.251.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.32/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.80/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.128/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.144/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.160/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.253.176/28 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.253.192/28 [110/1] via 172.27.189.139, 02:08:12, Vlan901
O E2     10.42.253.208/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.4/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.16/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.24/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.28/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.32/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.36/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.40/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.44/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.48/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.52/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.42.254.56/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.50.0.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.50.0.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.53.0.26/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.53.0.29/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.60.0.0/16 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.0.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.0.8/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.0.16/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.1.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.1.8/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.1.16/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.61.1.24/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.0/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.8/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.16/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.24/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.128/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.144/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.160/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.168/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.64.132.176/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.70.0.0/20 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.70.0.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.70.32.0/20 [110/1] via 172.27.189.139, 01:06:43, Vlan901
O E2     10.70.32.0/28 [110/1] via 172.27.189.139, 01:06:43, Vlan901
O E2     10.80.2.31/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.2.100/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.12.132/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.9/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.53/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.67/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.82/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.93/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.94/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.52.95/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.80.54.81/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.85.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.90.3.0/24 [110/1] via 172.27.189.139, 14:17:19, Vlan901
O E2     10.90.3.254/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.90.32.0/23 [110/1] via 172.27.189.139, 14:17:19, Vlan901
O E2     10.90.34.0/23 [110/1] via 172.27.189.139, 14:17:19, Vlan901
O E2     10.92.0.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.92.0.254/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.92.4.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.93.220.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.68.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.69.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.70.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.75.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.76.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.96.175.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.111.20.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.114.192.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.115.0.0/16 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.24.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.24.64/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.24.72/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.24.96/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.25.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.26.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.26.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.26.192/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.26.224/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.158.27.0/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.172.0.0/22 [110/1] via 172.27.189.139, 1d18h, Vlan901
O E2     10.172.1.240/29 [110/1] via 172.27.189.139, 1d18h, Vlan901
O E2     10.172.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.172.128.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.172.135.208/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.172.192.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.172.200.0/22 [110/1] via 172.27.189.139, 02:01:34, Vlan901
O E2     10.172.204.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.0.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.6.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.173.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.12.0/22 [110/1] via 172.27.189.139, 08:42:54, Vlan901
O E2     10.173.16.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.24.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.32.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.40.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.48.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.56.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.64.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.72.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.80.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.88.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.96.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.104.0/22 [110/1] via 172.27.189.139, 08:38:42, Vlan901
O E2     10.173.112.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.173.116.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.128.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.136.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.173.138.0/24 [110/1] via 172.27.189.139, 05:55:40, Vlan901
O E2     10.173.156.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.174.0.0/18 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.1.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.11.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.17.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.21.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.22.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.23.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.24.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.25.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.33.0/25 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.33.128/25 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.34.0/25 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.34.128/25 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.35.0/29 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.36.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.37.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.42.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.44.0/22 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.45.240/29 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.52.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.53.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.60.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.60.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.64.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.67.240/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.80.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.88.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.89.80/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.96.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.128/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.160/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.192/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.208/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.97.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.98.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.98.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.104.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.104.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.104.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.108.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.109.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.175.132.0/23 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.175.133.80/29 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.176.0.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.1.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.3.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.6.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.7.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.9.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.10.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.11.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.12.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.15.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.16.0/24 [110/1] via 172.27.189.139, 10:27:26, Vlan901
O E2     10.176.22.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.29.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.30.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.31.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.40.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.42.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.44.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.46.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.60.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.64.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.65.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.66.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.68.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.69.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.70.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.72.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.74.0/24 [110/1] via 172.27.189.139, 08:42:54, Vlan901
O E2     10.176.75.0/24 [110/1] via 172.27.189.139, 08:38:42, Vlan901
O E2     10.176.76.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.77.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.78.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.79.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.80.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.81.0/24 [110/1] via 172.27.189.139, 15:36:32, Vlan901
O E2     10.176.84.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.85.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.87.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.88.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.89.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.90.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.91.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.92.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.94.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.98.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.100.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.104.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.106.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.108.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.120.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.124.0/24 [110/1] via 172.27.189.139, 1d18h, Vlan901
O E2     10.176.126.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.127.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.129.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.131.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.133.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.134.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.136.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.139.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.143.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.150.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.152.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.152.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.156.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.157.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.160.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.162.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.164.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.166.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.168.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.169.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.170.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.171.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.172.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.176.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.178.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.179.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.180.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.183.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.184.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.186.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.187.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.188.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.191.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.192.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.193.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.194.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.197.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.198.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.199.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.200.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.201.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.202.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.203.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.204.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.205.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.206.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.207.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.209.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.210.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.212.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.216.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.218.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.220.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.222.0/24 [110/1] via 172.27.189.139, 01:58:20, Vlan901
O E2     10.176.225.0/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.227.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.229.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.230.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.232.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.236.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.240.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.243.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.246.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.250.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.176.254.0/24 [110/1] via 172.27.189.139, 1d11h, Vlan901
O E2     10.177.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.16.208/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.21.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.28.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.28.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.29.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.30.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.32.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.34.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.36.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.38.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.40.0/23 [110/1] via 172.27.189.139, 1d15h, Vlan901
O E2     10.177.43.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.44.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.45.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.46.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.49.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.52.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.56.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.60.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.73.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.74.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.92.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.93.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.95.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.96.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.98.8/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.98.12/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.98.253/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.98.254/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.100.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.102.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.103.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.177.104.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.177.105.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.105.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.106.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.107.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.108.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.177.110.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.112.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.112.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.112.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.116.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.117.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.117.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.117.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.118.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.119.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.120.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.124.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.128.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.129.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.133.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.134.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.137.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.140.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.144.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.146.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.148.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.152.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.154.0/23 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.177.156.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.158.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.158.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.158.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.160.0/23 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.162.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.164.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.166.0/24 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.167.0/24 [110/1] via 172.27.189.139, 15:29:46, Vlan901
O E2     10.177.168.0/23 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.170.0/23 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.172.0/23 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.174.0/28 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.174.192/26 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     10.177.185.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.226.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.228.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.240.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.248.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.177.251.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.179.0.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.179.2.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.179.8.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.179.8.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.184.12.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     10.196.75.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.200.0.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
S        10.203.0.75/32 [1/0] via 172.27.185.2
S        10.203.27.223/32 [1/0] via 172.27.185.2
S        10.203.27.224/32 [1/0] via 172.27.185.2
S        10.203.28.120/32 [1/0] via 172.27.185.2
S        10.203.28.121/32 [1/0] via 172.27.185.2
S        10.203.28.185/32 [1/0] via 172.27.185.2
S        10.203.28.186/32 [1/0] via 172.27.185.2
S        10.203.28.187/32 [1/0] via 172.27.185.2
S        10.203.28.188/32 [1/0] via 172.27.185.2
S        10.203.28.189/32 [1/0] via 172.27.185.2
S        10.203.28.190/32 [1/0] via 172.27.185.2
S        10.203.28.191/32 [1/0] via 172.27.185.2
S        10.203.28.192/32 [1/0] via 172.27.185.2
S        10.203.28.193/32 [1/0] via 172.27.185.2
S        10.203.28.194/32 [1/0] via 172.27.185.2
S        10.204.62.253/32 [1/0] via 172.27.185.2
S        10.208.220.23/32 [1/0] via 172.27.185.2
S        10.208.220.59/32 [1/0] via 172.27.185.2
S        10.208.220.200/32 [1/0] via 172.27.185.2
S        10.208.220.201/32 [1/0] via 172.27.185.2
S        10.208.220.202/32 [1/0] via 172.27.185.2
S        10.208.220.203/32 [1/0] via 172.27.185.2
S        10.208.221.139/32 [1/0] via 172.27.185.2
S        10.208.221.140/32 [1/0] via 172.27.185.2
S        10.208.221.141/32 [1/0] via 172.27.185.2
S        10.208.221.142/32 [1/0] via 172.27.185.2
S        10.208.221.143/32 [1/0] via 172.27.185.2
S        10.208.221.144/32 [1/0] via 172.27.185.2
S        10.208.221.145/32 [1/0] via 172.27.185.2
S        10.208.222.26/32 [1/0] via 172.27.185.2
S        10.208.222.27/32 [1/0] via 172.27.185.2
S        10.208.222.42/32 [1/0] via 172.27.185.2
S        10.208.222.43/32 [1/0] via 172.27.185.2
S        10.208.222.44/32 [1/0] via 172.27.185.2
S        10.208.222.45/32 [1/0] via 172.27.185.2
S        10.216.122.0/24 [1/0] via 172.27.185.2
O E2     10.222.222.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.224.30.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.240.4.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.240.126.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.254.254.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.255.8.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     10.255.24.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      12.0.0.0/32 is subnetted, 3 subnets
O E2     12.38.44.2 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     12.190.166.66 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     12.200.110.66 [110/1] via 172.27.189.139, 1d22h, Vlan901
      13.0.0.0/8 is variably subnetted, 5 subnets, 3 masks
S        13.65.246.180/32 [1/0] via 172.27.189.139
S        13.107.6.152/31 [1/0] via 172.27.189.139
S        13.107.18.10/31 [1/0] via 172.27.189.139
S        13.107.128.0/22 [1/0] via 172.27.189.139
S        13.107.136.0/22 [1/0] via 172.27.189.139
      15.0.0.0/24 is subnetted, 2 subnets
O E2     15.128.1.0 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     15.128.4.0 [110/1] via 172.27.189.139, 1d22h, Vlan901
      23.0.0.0/20 is subnetted, 1 subnets
S        23.103.160.0 [1/0] via 172.27.189.139
      24.0.0.0/32 is subnetted, 1 subnets
O E2     24.248.218.102 [110/1] via 172.27.189.139, 1d22h, Vlan901
      38.0.0.0/32 is subnetted, 1 subnets
O E2     38.140.119.18 [110/1] via 172.27.189.139, 1d22h, Vlan901
      40.0.0.0/8 is variably subnetted, 4 subnets, 3 masks
S        40.96.0.0/13 [1/0] via 172.27.189.139
S        40.104.0.0/15 [1/0] via 172.27.189.139
S        40.108.128.0/17 [1/0] via 172.27.189.139
S        40.126.0.0/15 [1/0] via 172.27.189.139
      46.0.0.0/32 is subnetted, 1 subnets
S        46.163.100.199 [1/0] via 172.27.189.139
      47.0.0.0/32 is subnetted, 1 subnets
O E2     47.44.57.50 [110/1] via 172.27.189.139, 1d22h, Vlan901
      50.0.0.0/32 is subnetted, 3 subnets
S        50.16.174.234 [1/0] via 172.27.189.139
O E2     50.84.3.2 [110/1] via 172.27.189.139, 1d22h, Vlan901
S        50.198.142.120 [1/0] via 172.27.189.139
      52.0.0.0/8 is variably subnetted, 4 subnets, 2 masks
S        52.51.248.160/32 [1/0] via 172.27.189.139
S        52.96.0.0/14 [1/0] via 172.27.189.139
S        52.104.0.0/14 [1/0] via 172.27.189.139
S        52.210.38.190/32 [1/0] via 172.27.189.139
      63.0.0.0/32 is subnetted, 2 subnets
O E2     63.96.229.9 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     63.230.214.74 [110/1] via 172.27.189.139, 1d22h, Vlan901
      64.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
O E2     64.76.82.176/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     64.76.82.186/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
      65.0.0.0/32 is subnetted, 1 subnets
O E2     65.205.82.52 [110/1] via 172.27.189.139, 1d22h, Vlan901
      70.0.0.0/32 is subnetted, 1 subnets
O E2     70.118.54.82 [110/1] via 172.27.189.139, 1d22h, Vlan901
      71.0.0.0/32 is subnetted, 1 subnets
O E2     71.97.230.102 [110/1] via 172.27.189.139, 1d22h, Vlan901
      72.0.0.0/32 is subnetted, 1 subnets
O E2     72.204.249.104 [110/1] via 172.27.189.139, 1d22h, Vlan901
      75.0.0.0/32 is subnetted, 3 subnets
O E2     75.59.147.45 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     75.142.188.179 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     75.142.188.187 [110/1] via 172.27.189.139, 1d22h, Vlan901
      78.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
S        78.40.77.0/24 [1/0] via 172.27.189.139
S        78.134.106.154/32 [1/0] via 172.27.189.139
S        78.134.106.157/32 [1/0] via 172.27.189.139
      79.0.0.0/32 is subnetted, 1 subnets
S        79.136.9.30 [1/0] via 172.27.189.139
      80.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
S        80.91.62.0/24 [1/0] via 172.27.189.139
S        80.146.172.224/27 [1/0] via 172.27.189.139
S        80.154.27.217/32 [1/0] via 172.27.189.139
      83.0.0.0/32 is subnetted, 1 subnets
O E2     83.231.210.78 [110/1] via 172.27.189.139, 1d22h, Vlan901
      85.0.0.0/32 is subnetted, 2 subnets
S        85.17.153.135 [1/0] via 172.27.189.139
S        85.37.200.240 [1/0] via 172.27.189.139
      87.0.0.0/32 is subnetted, 1 subnets
S        87.241.18.131 [1/0] via 172.27.189.139
      88.0.0.0/32 is subnetted, 3 subnets
S        88.149.216.170 [1/0] via 172.27.189.139
S        88.198.198.10 [1/0] via 172.27.189.139
S        88.198.198.11 [1/0] via 172.27.189.139
      91.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
O E2     91.212.234.0/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     91.212.234.16/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     91.212.234.39/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
      94.0.0.0/32 is subnetted, 2 subnets
S        94.254.44.179 [1/0] via 172.27.189.139
S        94.254.44.181 [1/0] via 172.27.189.139
      96.0.0.0/32 is subnetted, 1 subnets
O E2     96.76.184.221 [110/1] via 172.27.189.139, 1d22h, Vlan901
      104.0.0.0/17 is subnetted, 1 subnets
S        104.146.128.0 [1/0] via 172.27.189.139
      131.253.0.0/32 is subnetted, 1 subnets
S        131.253.33.215 [1/0] via 172.27.189.139
S     132.245.0.0/16 [1/0] via 172.27.189.139
      138.185.0.0/32 is subnetted, 3 subnets
O E2     138.185.224.46 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     138.185.224.250 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     138.185.225.46 [110/1] via 172.27.189.139, 1d22h, Vlan901
      150.171.0.0/22 is subnetted, 2 subnets
S        150.171.32.0 [1/0] via 172.27.189.139
S        150.171.40.0 [1/0] via 172.27.189.139
      159.63.0.0/32 is subnetted, 1 subnets
S        159.63.108.74 [1/0] via 172.27.189.139
      162.235.0.0/32 is subnetted, 1 subnets
O E2     162.235.149.233 [110/1] via 172.27.189.139, 1d22h, Vlan901
      169.57.0.0/32 is subnetted, 2 subnets
S        169.57.28.250 [1/0] via 172.27.189.139
S        169.57.62.200 [1/0] via 172.27.189.139
      172.16.0.0/16 is variably subnetted, 78 subnets, 10 masks
O E2     172.16.0.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.0.24/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
S        172.16.0.154/32 [1/0] via 172.27.189.139
O E2     172.16.10.74/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.16.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.16.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.24.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.31.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.32.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.36.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.39.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.44.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.45.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.48.0/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.49.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.50.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.51.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.56.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.60.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.61.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.62.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.62.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.62.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.63.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.64.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.68.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.71.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.71.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.71.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.71.8/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.71.12/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.80.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.83.64/28 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.16.84.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.86.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.88.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.92.160/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.96.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.97.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.100.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.103.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.104.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.111.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.111.204/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.111.208/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.111.253/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.111.254/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.112.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.116.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.117.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.118.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.120.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.123.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.124.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.126.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.156.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.159.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.160.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.163.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.164.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.164.160/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.168.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.172.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.173.128/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.174.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.185.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.205.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.211.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.227.217/32 [110/1] via 172.27.189.138, 2d02h, Vlan901
O E2     172.16.241.164/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.16.241.165/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.16.253.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.253.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.253.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.253.8/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.16.253.12/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.18.0.0/16 is variably subnetted, 18 subnets, 5 masks
O E2     172.18.8.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.12.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.16.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.20.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.32.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.33.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.40.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.41.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.48.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.49.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.56.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.57.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.72.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.73.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.101.42/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.160.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.161.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.18.208.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.20.0.0/16 is variably subnetted, 37 subnets, 3 masks
O E2     172.20.8.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.12.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.14.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.20.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.31.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.36.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.40.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.44.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.52.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.56.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.60.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.64.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.68.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.72.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.76.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.84.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.116.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.120.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.132.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.148.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.175.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.176.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.196.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
S        172.20.197.0/24 [1/0] via 172.27.189.139
O E2     172.20.198.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.208.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.212.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.214.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.220.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.226.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.242.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.246.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.248.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.250.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.252.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.252.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.20.254.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.21.0.0/16 is variably subnetted, 49 subnets, 10 masks
O E2     172.21.0.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.3.204/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.3.208/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.15.0/24 [110/1] via 172.27.189.139, 1d12h, Vlan901
O E2     172.21.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.20.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.24.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.28.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.32.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.34.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.35.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.36.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.37.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.37.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.37.192/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.37.224/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.37.240/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.38.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.39.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.39.16/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.39.32/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.39.48/28 [110/1] via 172.27.189.139, 1d04h, Vlan901
O E2     172.21.39.96/27 [110/1] via 172.27.189.139, 1d04h, Vlan901
O E2     172.21.39.192/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.40.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.41.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.42.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.44.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.48.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.52.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.54.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.56.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.64.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.84.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.100.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.101.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.114.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.122.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.156.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.159.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.160.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.200.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.208.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.218/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.219/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.220/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.228/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.232/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.21.255.236/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.23.0.0/16 is variably subnetted, 46 subnets, 7 masks
O E2     172.23.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.6.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.12.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.14.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.22.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.23.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.24.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.26.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.30.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.42.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.44.0/22 [110/1] via 172.27.189.139, 11:17:59, Vlan901
O E2     172.23.44.240/29 [110/1] via 172.27.189.139, 11:17:59, Vlan901
O E2     172.23.64.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.66.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.78.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.82.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.82.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.82.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.85.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.87.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.88.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.90.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.92.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.92.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.92.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.94.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.98.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.100.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.104.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.108.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.112.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.114.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.124.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.126.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.127.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.129.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.192.0/18 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.200.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.201.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.236.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.238.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.240.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.248.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.23.251.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.24.0.0/16 is variably subnetted, 74 subnets, 9 masks
O E2     172.24.1.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.2.0/24 [110/1] via 172.27.189.139, 1d12h, Vlan901
O E2     172.24.5.37/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.24.5.56/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.24.5.57/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.24.5.58/32 [110/1] via 172.27.189.137, 6w1d, Vlan901
O E2     172.24.5.59/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.60/32 [110/1] via 172.27.189.137, 2d08h, Vlan901
O E2     172.24.5.61/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.67/32 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.24.5.69/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.24.5.70/32 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.24.5.71/32 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.24.5.77/32 [110/1] via 172.27.189.137, 1w0d, Vlan901
O E2     172.24.5.78/32 [110/1] via 172.27.189.137, 1w0d, Vlan901
O E2     172.24.5.79/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.80/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.81/32 [110/1] via 172.27.189.137, 09:55:45, Vlan901
O E2     172.24.5.82/32 [110/1] via 172.27.189.137, 21:05:37, Vlan901
O E2     172.24.5.83/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.24.5.84/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.24.5.87/32 [110/1] via 172.27.189.137, 5w3d, Vlan901
O E2     172.24.5.88/32 [110/1] via 172.27.189.137, 00:21:20, Vlan901
O E2     172.24.5.89/32 [110/1] via 172.27.189.137, 2d05h, Vlan901
O E2     172.24.5.90/32 [110/1] via 172.27.189.137, 5d20h, Vlan901
O E2     172.24.5.91/32 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.24.5.92/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.24.5.95/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.96/32 [110/1] via 172.27.189.137, 3d21h, Vlan901
O E2     172.24.5.98/32 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.24.5.99/32 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.24.5.107/32 [110/1] via 172.27.189.137, 4w1d, Vlan901
O E2     172.24.5.108/32 [110/1] via 172.27.189.137, 2w6d, Vlan901
O E2     172.24.5.109/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.24.5.110/32 [110/1] via 172.27.189.137, 1w6d, Vlan901
O E2     172.24.5.122/32 [110/1] via 172.27.189.137, 00:20:38, Vlan901
O E2     172.24.5.123/32 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.24.5.188/32 [110/1] via 172.27.189.137, 1w4d, Vlan901
O E2     172.24.5.192/32 [110/1] via 172.27.189.137, 1w4d, Vlan901
O E2     172.24.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.12.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.16.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.24.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.36.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.38.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.40.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.42.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.46.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.50.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.56.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.64.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.66.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.68.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.76.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.80.0/25 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.80.128/27 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.80.160/29 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.81.0/25 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.82.0/24 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.83.0/27 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.83.64/26 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.83.128/27 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.83.160/29 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.83.192/27 [110/1] via 172.27.189.139, 1d02h, Vlan901
O E2     172.24.100.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.108.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.116.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.118.0/24 [110/1] via 172.27.189.139, 1d12h, Vlan901
O E2     172.24.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.152.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.160.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.164.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.24.208.0/20 [110/1] via 172.27.189.139, 1d16h, Vlan901
      172.25.0.0/16 is variably subnetted, 12 subnets, 4 masks
O E2     172.25.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.10.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.10.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.10.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.11.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.11.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.11.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.12.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.12.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.12.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.16.0/20 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.25.32.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.26.0.0/16 is variably subnetted, 66 subnets, 10 masks
O E2     172.26.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.2.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.8.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.12.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.14.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.27.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.27.16/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.28.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.31.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.32.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.34.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.35.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.35.160/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.40.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.42.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.48.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.56.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.60.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.62.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.64.0/24 [110/1] via 172.27.189.139, 1d14h, Vlan901
O E2     172.26.76.0/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.77.0/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.77.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.78.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.79.1/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.79.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.79.16/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.79.24/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.79.32/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.80.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.81.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.82.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.83.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.84.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.85.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.86.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.87.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.90.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.90.10/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.90.20/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.90.32/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.90.48/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.91.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.92.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.93.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.94.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.100.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.101.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.104.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.120.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.124.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.129.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.130.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.131.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.132.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.136.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.144.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.152.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.160.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.164.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.168.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.180.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.26.252.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.27.0.0/16 is variably subnetted, 219 subnets, 11 masks
O E2     172.27.0.0/17 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.46.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.82.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.101.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.102.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.20/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.82/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.125/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.163/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.170/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.194/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.215/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.220/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.223/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.224/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.104.250/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.105.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.102/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.105/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.107/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.114/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.106.132/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.108.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.108.20/32 [110/1] via 172.27.189.139, 21:27:51, Vlan901
O E2     172.27.109.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.110.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.111.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.112.0/25 [110/1] via 172.27.189.139, 21:11:05, Vlan901
O E2     172.27.113.0/24 [110/1] via 172.27.189.139, 16:10:14, Vlan901
O E2     172.27.114.0/24 [110/1] via 172.27.189.139, 03:46:18, Vlan901
O E2     172.27.115.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.116.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.117.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.118.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.27.119.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
C        172.27.128.0/22 is directly connected, Vlan102
L        172.27.128.1/32 is directly connected, Vlan102
C        172.27.134.0/24 is directly connected, Vlan406
L        172.27.134.1/32 is directly connected, Vlan406
O E2     172.27.135.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.135.128/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.135.160/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.136.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.137.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.137.5/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.138.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.138.187/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.139.0/24 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.27.140.0/24 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.27.140.160/28 [110/1] via 172.27.189.137, 6w2d, Vlan901
S        172.27.141.0/24 [120/0] via 172.27.189.138
S        172.27.142.0/24 [120/0] via 172.27.189.138
O E2     172.27.143.0/24 [110/1] via 172.27.189.137, 6w2d, Vlan901
S        172.27.144.0/24 [120/0] via 172.27.189.137
O E2     172.27.145.0/24 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.145.6/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.145.126/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.145.160/28 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.146.0/24 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.146.0/25 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.147.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.147.128/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.148.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.149.0/24 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.27.149.8/32 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.27.149.113/32 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.27.149.160/28 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.27.150.0/24 [110/1] via 172.27.189.137, 1w1d, Vlan901
S        172.27.151.0/24 [120/0] via 172.27.189.137
S        172.27.152.0/24 [120/0] via 172.27.189.137
O E2     172.27.153.0/24 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.153.160/28 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.154.0/24 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.155.0/24 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.155.160/28 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.156.0/24 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.157.0/24 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.157.16/28 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.157.32/27 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.157.64/26 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.157.128/25 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.158.0/24 [110/1] via 172.27.189.138, 1w2d, Vlan901
S        172.27.159.0/24 [120/0] via 172.27.189.137
S        172.27.160.0/24 [120/0] via 172.27.189.137
S        172.27.161.0/24 [120/0] via 172.27.189.137
S        172.27.162.0/24 [120/0] via 172.27.189.137
O E2     172.27.163.0/28 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.163.16/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.163.32/28 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.27.163.48/28 [110/1] via 172.27.189.138, 5d21h, Vlan901
O E2     172.27.163.64/28 [110/1] via 172.27.189.138, 2d02h, Vlan901
O E2     172.27.163.80/28 [110/1] via 172.27.189.137, 6w1d, Vlan901
S        172.27.163.96/28 [120/0] via 172.27.189.138
O E2     172.27.163.112/28 [110/1] via 172.27.189.137, 4w1d, Vlan901
O E2     172.27.163.128/28 [110/1] via 172.27.189.137, 5w2d, Vlan901
O E2     172.27.163.144/28 [110/1] via 172.27.189.137, 1w4d, Vlan901
O E2     172.27.163.160/28 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.163.176/28 [110/1] via 172.27.189.137, 5w3d, Vlan901
O E2     172.27.163.192/28 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.163.208/28 [110/1] via 172.27.189.137, 3w5d, Vlan901
S        172.27.163.224/28 [120/0] via 172.27.189.138
O E2     172.27.163.240/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.164.0/28 [110/1] via 172.27.189.138, 1d23h, Vlan901
O E2     172.27.165.0/27 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.165.32/27 [110/1] via 172.27.189.138, 2d02h, Vlan901
O E2     172.27.166.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.167.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.168.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.169.0/24 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.169.128/28 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.0/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.4/30 [110/1] via 172.27.189.137, 6w1d, Vlan901
O E2     172.27.170.8/30 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.170.12/30 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.27.170.28/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.32/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.36/30 [110/1] via 172.27.189.137, 00:21:57, Vlan901
O E2     172.27.170.40/30 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.170.44/30 [110/1] via 172.27.189.137, 09:57:52, Vlan901
O E2     172.27.170.48/30 [110/1] via 172.27.189.137, 21:06:44, Vlan901
O E2     172.27.170.52/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.56/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.60/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.64/30 [110/1] via 172.27.189.137, 1w0d, Vlan901
O E2     172.27.170.68/30 [110/1] via 172.27.189.137, 4w1d, Vlan901
O E2     172.27.170.72/30 [110/1] via 172.27.189.137, 2w6d, Vlan901
O E2     172.27.170.76/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.84/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.85/32 [110/1] via 172.27.189.137, 3d21h, Vlan901
O E2     172.27.170.92/30 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.27.170.96/30 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.170.100/30 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.170.104/30 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.170.116/30 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.170.120/30 [110/1] via 172.27.189.137, 1w2d, Vlan901
O        172.27.170.124/30 [110/2] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.136/30 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.170.140/30 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.27.170.144/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.148/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.152/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.156/30 [110/1] via 172.27.189.137, 1w6d, Vlan901
O E2     172.27.170.168/30 [110/1] via 172.27.189.137, 2d05h, Vlan901
O E2     172.27.170.172/30 [110/1] via 172.27.189.137, 5d20h, Vlan901
O E2     172.27.170.184/30 [110/1] via 172.27.189.137, 5w3d, Vlan901
O E2     172.27.170.188/30 [110/1] via 172.27.189.137, 4d20h, Vlan901
O E2     172.27.170.192/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.196/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.200/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.170.252/30 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.27.171.0/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.8/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.16/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.24/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.32/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.40/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.48/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.56/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.64/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.72/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.80/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.88/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.96/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.104/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.112/29 [110/1] via 172.27.189.138, 5d13h, Vlan901
O E2     172.27.171.120/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.152/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.160/29 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.27.171.224/29 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.27.171.232/29 [110/160] via 172.27.189.138, 1w2d, Vlan901
S        172.27.172.0/22 [120/0] via 172.27.189.137
O E2     172.27.172.0/26 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.172.64/27 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.172.96/28 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.172.112/29 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.172.120/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.172.121/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.172.122/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.172.123/32 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.172.124/32 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.172.128/25 [110/1] via 172.27.189.138, 5d11h, Vlan901
O E2     172.27.173.0/24 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.174.0/23 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.177.0/24 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.178.0/24 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.179.0/25 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.179.128/25 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.180.0/25 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.180.128/27 [110/1] via 172.27.189.138, 1w2d, Vlan901
O E2     172.27.180.160/27 [110/1] via 172.27.189.138, 1w2d, Vlan901
C        172.27.181.192/27 is directly connected, Vlan411
L        172.27.181.193/32 is directly connected, Vlan411
C        172.27.181.224/27 is directly connected, Vlan412
L        172.27.181.225/32 is directly connected, Vlan412
S        172.27.184.24/29 [1/0] via 172.27.185.2
C        172.27.184.128/25 is directly connected, Vlan403
L        172.27.184.129/32 is directly connected, Vlan403
C        172.27.185.0/26 is directly connected, Vlan401
L        172.27.185.1/32 is directly connected, Vlan401
C        172.27.186.0/23 is directly connected, Vlan101
L        172.27.186.1/32 is directly connected, Vlan101
C        172.27.188.0/26 is directly connected, Vlan1102
L        172.27.188.1/32 is directly connected, Vlan1102
C        172.27.188.64/26 is directly connected, Vlan802
L        172.27.188.65/32 is directly connected, Vlan802
C        172.27.188.128/26 is directly connected, Vlan3
L        172.27.188.129/32 is directly connected, Vlan3
C        172.27.188.192/26 is directly connected, Vlan4
L        172.27.188.193/32 is directly connected, Vlan4
C        172.27.189.0/26 is directly connected, Vlan103
L        172.27.189.1/32 is directly connected, Vlan103
C        172.27.189.64/26 is directly connected, Vlan413
L        172.27.189.65/32 is directly connected, Vlan413
C        172.27.189.128/26 is directly connected, Vlan901
L        172.27.189.129/32 is directly connected, Vlan901
C        172.27.190.0/24 is directly connected, Vlan301
L        172.27.190.1/32 is directly connected, Vlan301
      172.28.0.0/16 is variably subnetted, 22 subnets, 6 masks
O E2     172.28.16.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.24.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.32.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.48.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.58.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.60.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.64.0/23 [110/1] via 172.27.189.139, 03:25:35, Vlan901
O E2     172.28.72.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.74.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.80.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.88.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.96.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.100.35/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.100.36/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.104.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.112.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.112.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.120.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.120.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.128.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.129.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.28.224.0/19 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.29.0.0/16 is variably subnetted, 5 subnets, 2 masks
O E2     172.29.80.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.29.128.0/21 [110/160] via 172.27.189.138, 3d01h, Vlan901
O E2     172.29.136.0/21 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.29.144.0/24 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.29.146.0/24 [110/160] via 172.27.189.138, 4d02h, Vlan901
      172.30.0.0/16 is variably subnetted, 53 subnets, 11 masks
O E2     172.30.0.0/18 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.49.64/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.64.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.72.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.80.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.88.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.96.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.106.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.110.36/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.110.40/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.126.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.132.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.134.0/24 [110/1] via 172.27.189.139, 10:35:04, Vlan901
O E2     172.30.136.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.138.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.146.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.160.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.172.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.172.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.176.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.176.128/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.178.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.179.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.179.64/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.180.224/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.181.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.181.64/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.181.96/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.181.112/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.181.128/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.0/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.64/26 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.128/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.144/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.160/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.168/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.176/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.184/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.192/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.200/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.208/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.216/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.224/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.232/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.240/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.183.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.184.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.184.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.192.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.192.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.200.0/21 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.30.200.248/29 [110/1] via 172.27.189.139, 1d22h, Vlan901
      172.31.0.0/16 is variably subnetted, 119 subnets, 7 masks
O E2     172.31.40.18/32 [110/1] via 172.27.189.139, 11:17:59, Vlan901
O E2     172.31.40.28/32 [110/1] via 172.27.189.139, 01:58:20, Vlan901
O E2     172.31.40.38/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.40.48/32 [110/1] via 172.27.189.139, 1d18h, Vlan901
O E2     172.31.40.73/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.8/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.12/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.16/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.20/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.24/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.28/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.32/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.36/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.40/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.44/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.68/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.56.240/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.57.0/28 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.64.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.64.42/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.48/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.31.64.79/32 [110/1] via 172.27.189.137, 1w5d, Vlan901
O E2     172.31.64.80/32 [110/1] via 172.27.189.137, 6w1d, Vlan901
O E2     172.31.64.81/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.82/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.85/32 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.31.64.86/32 [110/1] via 172.27.189.137, 3w1d, Vlan901
O E2     172.31.64.91/32 [110/1] via 172.27.189.137, 4w1d, Vlan901
O E2     172.31.64.92/32 [110/1] via 172.27.189.137, 4w1d, Vlan901
O E2     172.31.64.93/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.94/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.95/32 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.31.64.96/32 [110/1] via 172.27.189.137, 1w2d, Vlan901
O E2     172.31.64.97/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.98/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.99/32 [110/1] via 172.27.189.137, 5w3d, Vlan901
O E2     172.31.64.100/32 [110/1] via 172.27.189.137, 5w3d, Vlan901
O E2     172.31.64.101/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.102/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.105/32 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.31.64.106/32 [110/1] via 172.27.189.137, 6w2d, Vlan901
O E2     172.31.64.107/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.108/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.109/32 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.31.64.110/32 [110/1] via 172.27.189.137, 1w1d, Vlan901
O E2     172.31.64.111/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.112/32 [110/1] via 172.27.189.137, 7w0d, Vlan901
O E2     172.31.64.113/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.114/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.115/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.31.64.116/32 [110/1] via 172.27.189.137, 1w3d, Vlan901
O E2     172.31.64.117/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.118/32 [110/1] via 172.27.189.137, 3w5d, Vlan901
O E2     172.31.64.200/32 [110/1] via 172.27.189.138, 7w0d, Vlan901
O E2     172.31.64.201/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.202/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.203/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.204/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.205/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.206/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.207/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.208/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.64.209/32 [110/160] via 172.27.189.138, 1w2d, Vlan901
O E2     172.31.72.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.80.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.88.0/25 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.96.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.112.2/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.112.5/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.112.8/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.112.9/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.114.0/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.114.4/32 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2     172.31.114.5/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.114.6/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.160/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.161/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.164/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.168/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.169/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.170/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.171/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.172/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.173/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.174/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.175/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.176/32 [110/1] via 172.27.189.139, 10:27:25, Vlan901
O E2     172.31.139.178/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.179/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.180/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.181/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.182/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.183/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.184/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.185/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.186/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.187/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.139.190/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.144.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.10/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.11/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.12/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.13/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.14/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.15/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.16/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.17/32 [110/1] via 172.27.189.139, 21:11:35, Vlan901
O E2     172.31.148.18/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.19/32 [110/1] via 172.27.189.139, 03:46:18, Vlan901
O E2     172.31.148.20/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.148.21/32 [110/1] via 172.27.189.139, 21:27:51, Vlan901
O E2     172.31.148.22/32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.174.32/27 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.176.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     172.31.200.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
C        172.31.254.0/24 is directly connected, Tunnel403
L        172.31.254.1/32 is directly connected, Tunnel403
      173.230.0.0/32 is subnetted, 1 subnets
S        173.230.135.175 [1/0] via 172.27.189.139
      184.68.0.0/32 is subnetted, 1 subnets
S        184.68.145.182 [1/0] via 172.27.189.139
      184.72.0.0/32 is subnetted, 2 subnets
S        184.72.42.7 [1/0] via 172.27.189.139
S        184.72.220.33 [1/0] via 172.27.189.139
      187.189.0.0/32 is subnetted, 1 subnets
S        187.189.18.157 [1/0] via 172.27.189.139
      191.234.0.0/22 is subnetted, 1 subnets
S        191.234.140.0 [1/0] via 172.27.189.139
      192.145.232.0/24 is variably subnetted, 3 subnets, 2 masks
S        192.145.232.32/32 [1/0] via 172.27.189.139
S        192.145.232.36/30 [1/0] via 172.27.189.139
S        192.145.232.38/32 [1/0] via 172.27.189.139
O E2  192.168.0.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.1.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.2.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.3.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.5.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.6.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.7.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.8.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.10.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.11.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.12.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.20.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.21.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.22.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.23.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.24.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      192.168.92.0/29 is subnetted, 1 subnets
O E2     192.168.92.0 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.94.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.95.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.97.0/24 [110/1] via 172.27.189.139, 03:25:35, Vlan901
O E2  192.168.98.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.99.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.100.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.108.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
      192.168.111.0/24 is variably subnetted, 2 subnets, 2 masks
O E2     192.168.111.0/24 [110/1] via 172.27.189.139, 05:07:21, Vlan901
O E2     192.168.111.24/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.116.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.126.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.128.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.130.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.150.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.160.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.164.0/22 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.170.0/23 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.172.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.200.0/24 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2  192.168.201.0/24 [110/1] via 172.27.189.139, 16:01:22, Vlan901
      192.168.240.0/29 is subnetted, 5 subnets
O E2     192.168.240.16 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     192.168.240.24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     192.168.240.32 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     192.168.240.48 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     192.168.240.64 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.168.250.0/24 [110/1] via 172.27.189.139, 16:01:22, Vlan901
O E2  192.168.252.0/24 [110/1] via 172.27.189.139, 16:01:22, Vlan901
      192.168.254.0/32 is subnetted, 1 subnets
O E2     192.168.254.3 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  192.200.9.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  193.101.92.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  193.242.175.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      194.39.131.0/32 is subnetted, 1 subnets
S        194.39.131.178 [1/0] via 172.27.189.139
      194.117.106.0/24 is variably subnetted, 2 subnets, 2 masks
O E2     194.117.106.128/30 [110/1] via 172.27.189.139, 1d22h, Vlan901
S        194.117.106.129/32 [1/0] via 172.27.189.139
      195.128.170.0/32 is subnetted, 1 subnets
O E2     195.128.170.40 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2  195.128.171.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      195.145.232.0/24 is variably subnetted, 3 subnets, 3 masks
S        195.145.232.0/26 [1/0] via 172.27.189.139
S        195.145.232.32/32 [1/0] via 172.27.189.139
S        195.145.232.36/30 [1/0] via 172.27.189.139
      198.50.162.0/32 is subnetted, 1 subnets
S        198.50.162.21 [1/0] via 172.27.189.139
      200.31.17.0/29 is subnetted, 1 subnets
O E2     200.31.17.128 [110/1] via 172.27.189.137, 7w0d, Vlan901
      200.52.139.0/32 is subnetted, 1 subnets
O E2     200.52.139.21 [110/1] via 172.27.189.139, 1d22h, Vlan901
      200.53.1.0/32 is subnetted, 1 subnets
S        200.53.1.48 [1/0] via 172.27.189.139
      200.56.200.0/32 is subnetted, 1 subnets
O E2     200.56.200.131 [110/1] via 172.27.189.139, 1d22h, Vlan901
      200.67.139.0/32 is subnetted, 1 subnets
S        200.67.139.251 [1/0] via 172.27.189.139
      200.76.5.0/32 is subnetted, 1 subnets
S        200.76.5.147 [1/0] via 172.27.189.134
      200.76.152.0/32 is subnetted, 3 subnets
S        200.76.152.227 [1/0] via 172.27.189.139
S        200.76.152.240 [1/0] via 172.27.189.139
S        200.76.152.243 [1/0] via 172.27.189.139
      200.188.11.0/32 is subnetted, 1 subnets
S        200.188.11.157 [1/0] via 172.27.189.139
      201.100.18.0/32 is subnetted, 1 subnets
O E2     201.100.18.26 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.100.32.0/32 is subnetted, 1 subnets
O E2     201.100.32.244 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.100.34.0/32 is subnetted, 1 subnets
O E2     201.100.34.197 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.100.65.0/32 is subnetted, 1 subnets
O E2     201.100.65.86 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.140.135.0/32 is subnetted, 1 subnets
O E2     201.140.135.117 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.140.139.0/32 is subnetted, 1 subnets
O E2     201.140.139.109 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.140.148.0/32 is subnetted, 1 subnets
O E2     201.140.148.226 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.140.170.0/32 is subnetted, 1 subnets
O E2     201.140.170.177 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.149.255.0/32 is subnetted, 2 subnets
S        201.149.255.24 [1/0] via 172.27.189.139
S        201.149.255.57 [1/0] via 172.27.189.139
      201.151.239.0/32 is subnetted, 1 subnets
S        201.151.239.195 [1/0] via 172.27.189.134
      201.155.131.0/32 is subnetted, 1 subnets
O E2     201.155.131.192 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.163.179.0/32 is subnetted, 1 subnets
O E2     201.163.179.188 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.5.0/32 is subnetted, 1 subnets
O E2     201.174.5.10 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.20.0/32 is subnetted, 1 subnets
O E2     201.174.20.58 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.27.0/32 is subnetted, 3 subnets
O E2     201.174.27.98 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     201.174.27.130 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     201.174.27.210 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.55.0/32 is subnetted, 1 subnets
O E2     201.174.55.202 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.56.0/32 is subnetted, 1 subnets
O E2     201.174.56.162 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.65.0/32 is subnetted, 1 subnets
O E2     201.174.65.50 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.109.0/32 is subnetted, 1 subnets
O E2     201.174.109.106 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.111.0/32 is subnetted, 2 subnets
O E2     201.174.111.50 [110/1] via 172.27.189.139, 1d22h, Vlan901
O E2     201.174.111.58 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.123.0/32 is subnetted, 1 subnets
O E2     201.174.123.10 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.134.0/32 is subnetted, 1 subnets
O E2     201.174.134.254 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.136.0/32 is subnetted, 1 subnets
O E2     201.174.136.74 [110/1] via 172.27.189.139, 1d22h, Vlan901
      201.174.147.0/32 is subnetted, 1 subnets
O E2     201.174.147.50 [110/1] via 172.27.189.139, 1d22h, Vlan901
      204.79.197.0/32 is subnetted, 1 subnets
S        204.79.197.215 [1/0] via 172.27.189.139
S     206.18.172.0/24 [1/0] via 172.27.189.139
      206.51.26.0/32 is subnetted, 1 subnets
S        206.51.26.33 [1/0] via 172.27.189.134
      207.44.167.0/32 is subnetted, 1 subnets
S        207.44.167.210 [1/0] via 172.27.189.139
      208.95.94.0/32 is subnetted, 1 subnets
O E2     208.95.94.90 [110/1] via 172.27.189.139, 1d22h, Vlan901
      209.203.72.0/32 is subnetted, 1 subnets
O E2     209.203.72.158 [110/1] via 172.27.189.139, 1d22h, Vlan901
      210.173.216.0/32 is subnetted, 1 subnets
S        210.173.216.40 [1/0] via 172.27.189.139
      212.49.145.0/32 is subnetted, 4 subnets
S        212.49.145.8 [1/0] via 172.27.189.139
S        212.49.145.11 [1/0] via 172.27.189.134
S        212.49.145.39 [1/0] via 172.27.189.139
S        212.49.145.70 [1/0] via 172.27.189.139
      212.63.73.0/32 is subnetted, 1 subnets
S        212.63.73.203 [1/0] via 172.27.189.139
O E2  212.108.4.0/24 [110/1] via 172.27.189.139, 1d22h, Vlan901
      212.185.180.0/28 is subnetted, 1 subnets
S        212.185.180.160 [1/0] via 172.27.189.139
      212.247.59.0/27 is subnetted, 1 subnets
O E2     212.247.59.0 [110/1] via 172.27.189.139, 1d22h, Vlan901
      213.236.72.0/32 is subnetted, 1 subnets
O E2     213.236.72.156 [110/1] via 172.27.189.139, 1d22h, Vlan901
      216.59.201.0/32 is subnetted, 1 subnets
O E2     216.59.201.210 [110/1] via 172.27.189.139, 1d22h, Vlan901
      216.59.204.0/32 is subnetted, 1 subnets
O E2     216.59.204.82 [110/1] via 172.27.189.139, 1d22h, Vlan901
      217.58.138.0/32 is subnetted, 1 subnets
S        217.58.138.0 [1/0] via 172.27.189.139
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh access-list
Extended IP access list ACL_35MBPS
    10 permit ip host 172.27.188.86 host 172.27.157.47
Extended IP access list Guest
    10 permit tcp 172.27.181.224 0.0.0.31 any eq www
    20 permit tcp 172.27.181.224 0.0.0.31 any eq 443
    30 permit tcp 172.27.181.224 0.0.0.31 any eq pop2
    40 permit tcp 172.27.181.224 0.0.0.31 any eq pop3
    50 permit tcp 172.27.181.224 0.0.0.31 any eq smtp
    60 permit tcp 172.27.181.224 0.0.0.31 any eq echo
Extended IP access list IP-Adm-V4-Int-ACL-global
    10 permit tcp any any eq www
    20 permit tcp any any eq 443
Extended IP access list SNMP
    10 permit ip host 172.27.188.13 any
    20 permit ip any host 172.27.188.13
    30 permit ip host 172.27.188.34 any
    40 permit ip any host 172.27.188.34
Extended IP access list UCS_ControlAccess
    10 permit tcp 172.27.0.0 0.0.255.255 host 172.27.188.220
Extended IP access list VOZ_IN
    10 permit ip 172.27.190.0 0.0.0.255 any log (948753515 matches)
    20 permit ip any any (1536 matches)
    30 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group VLANsVoice
    40 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group ServersVoice
    50 permit object-group TCP_Voz 172.27.190.0 0.0.0.255 object-group VlansIpComunicator
    60 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group VLANsVoice
    70 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group ServersVoice
    80 permit object-group UDP_Voz 172.27.190.0 0.0.0.255 object-group VlansIpComunicator
Extended IP access list VOZ_OUT
    10 permit object-group TCP_Voz object-group VLANsVoice 172.27.190.0 0.0.0.255
    20 permit object-group TCP_Voz object-group ServersVoice 172.27.190.0 0.0.0.255
    30 permit object-group TCP_Voz object-group VlansIpComunicator 172.27.190.0 0.0.0.255
    40 permit object-group UDP_Voz object-group VLANsVoice 172.27.190.0 0.0.0.255
    50 permit object-group UDP_Voz object-group ServersVoice 172.27.190.0 0.0.0.255
    60 permit object-group UDP_Voz object-group VlansIpComunicator 172.27.190.0 0.0.0.255
    70 permit ip any 172.27.190.0 0.0.0.255 log (625092944 matches)
Extended IP access list cos2
    10 permit udp host 172.27.188.13 eq snmp any
    20 permit udp any eq ntp any
    30 permit udp any eq time any
    40 permit tcp any eq 37 any
    50 permit udp host 172.27.188.15 eq domain any
    60 permit udp host 172.27.188.16 eq domain any
    70 permit tcp host 172.27.188.15 eq domain any
    80 permit tcp host 172.27.188.16 eq domain any
    90 permit tcp host 172.27.188.16 eq 67 any
    100 permit tcp host 172.27.188.16 eq 68 any
    110 permit udp host 172.27.188.16 eq bootpc any
    120 permit udp host 172.27.188.16 eq bootps any
    130 permit udp host 172.27.188.16 eq 445 any
    140 permit udp host 172.27.188.15 eq 445 any
    150 permit tcp host 172.27.188.15 eq 445 any
    160 permit tcp host 172.27.188.16 eq 445 any
    170 permit tcp any eq 389 any
    180 permit udp any eq 389 any
    190 permit udp any eq nameserver any
    200 permit tcp host 172.27.188.19 eq 81 any
    210 permit udp host 172.27.188.19 eq 81 any
    220 permit udp host 172.27.188.20 eq 3101 any
    230 permit tcp host 172.27.188.20 eq 3101 any
    240 permit tcp any eq 3101 any
    250 permit udp any eq 3101 any
    260 permit udp host 172.27.188.17 eq 135 any
    270 permit udp host 172.27.188.17 eq 136 any
    280 permit udp host 172.27.188.17 eq netbios-ns any
    290 permit udp host 172.27.188.17 eq netbios-dgm any
    300 permit udp host 172.27.188.17 eq netbios-ss any
    310 permit tcp host 172.27.188.17 eq msrpc any
    320 permit tcp host 172.27.188.17 eq 136 any
    330 permit tcp host 172.27.188.17 eq 137 any
    340 permit tcp host 172.27.188.17 eq 138 any
    350 permit tcp host 172.27.188.17 eq 139 any
    360 permit tcp any eq www any
    370 permit tcp any eq 443 any
    380 permit tcp any eq 8080 any
    390 permit tcp any eq 8088 any
    400 permit tcp host 172.27.188.21 range 1500 1520 any
    410 permit tcp host 172.27.188.22 range 1500 1520 any
    420 permit udp host 172.27.188.22 range 1500 1520 any
    430 permit udp host 172.27.188.21 range 1500 1520 any
    440 permit udp host 172.27.188.21 eq 1580 any
    450 permit tcp host 172.27.188.21 eq 1580 any
    460 permit tcp host 172.27.188.22 eq 1580 any
    470 permit udp host 172.27.188.22 eq 1580 any
Extended IP access list cos3
    10 permit tcp host 172.27.188.22 eq smtp any
    20 permit tcp host 172.27.188.22 eq pop3 any
    30 permit udp host 172.27.188.22 eq 110 any
    40 permit udp host 172.27.188.22 eq 25 any
    50 permit udp host 172.27.188.24 eq 21 any
    60 permit udp host 172.27.188.24 eq 80 any
    70 permit tcp host 172.27.188.24 eq www any
    80 permit tcp host 172.27.188.24 eq ftp any
    90 permit tcp host 172.27.188.23 eq www any
    100 permit udp host 172.27.188.23 eq 80 any
    110 permit udp host 172.27.188.23 eq 443 any
    120 permit tcp host 172.27.188.23 eq 443 any
    130 permit tcp host 172.27.188.17 eq 1433 any
    140 permit udp host 172.27.188.17 eq 1433 any
    150 permit udp host 172.27.188.17 eq 1434 any
    160 permit tcp host 172.27.188.17 eq 1434 any
    170 permit tcp any range 1128 1129 any
    180 permit udp any range 1128 1129 any
    190 permit tcp any range 1520 1529 any
    200 permit udp any range 1520 1529 any
    210 permit tcp any range 3200 3299 any
    220 permit udp any range 3200 3299 any
    230 permit tcp any range 3300 3399 any
    240 permit udp any range 3300 3399 any
    250 permit tcp any range 3600 3699 any
    260 permit udp any range 3600 3699 any
    270 permit tcp any range 3900 3999 any
    280 permit udp any range 3900 3999 any
    290 permit udp any range 4238 4241 any
    300 permit tcp any range 4238 4241 any
    310 permit tcp any range 4700 4799 any
    320 permit tcp any range 4800 4899 any
    330 permit udp any range 4700 4799 any
    340 permit udp any range 4800 4899 any
    350 permit udp any range 8000 8099 any
    360 permit tcp any range 8000 8099 any
    370 permit tcp any range 21212 21213 any
    380 permit udp any range 21212 21213 any
    390 permit udp any range 44300 44399 any
    400 permit tcp any range 44300 44399 any
    410 permit tcp any range 44400 44499 any
    420 permit udp any range 44400 44499 any
    430 permit udp any range 50000 50099 any
    440 permit tcp any range 50000 50099 any
    450 permit tcp any eq smtp any
    460 permit udp any eq 25 any
    470 permit udp any eq 515 any
    480 permit tcp any eq lpd any
Extended IP access list implicit_deny
    10 deny ip any any
Extended IP access list implicit_permit
    10 permit ip any any
Extended IP access list preauth_v4
    10 permit udp any any eq domain
    20 permit tcp any any eq domain
    30 permit udp any eq bootps any
    40 permit udp any any eq bootpc
    50 permit udp any eq bootpc any
    60 deny ip any any
Extended IP access list vty
    10 permit ip 172.27.186.0 0.0.1.255 any log
    20 permit ip 172.27.174.1 0.0.0.254 any
    30 permit ip 172.27.136.9 0.0.0.2 any
    40 permit ip 172.27.188.128 0.0.0.63 any log
    50 permit ip host 172.27.188.45 any log
    60 permit ip 172.27.131.0 0.0.0.255 any log
    70 permit ip host 172.27.188.34 any log
    80 permit ip host 172.27.166.52 any log
IPv6 access list implicit_deny_v6
    deny ipv6 any any sequence 10
IPv6 access list implicit_permit_v6
    permit ipv6 any any sequence 10
IPv6 access list preauth_v6
    permit udp any any eq domain sequence 10
    permit tcp any any eq domain sequence 20
    permit icmp any any nd-ns sequence 30
    permit icmp any any nd-na sequence 40
    permit icmp any any router-solicitation sequence 50
    permit icmp any any router-advertisement sequence 60
    permit icmp any any redirect sequence 70
    permit udp any eq 547 any eq 546 sequence 80
    permit udp any eq 546 any eq 547 sequence 90
    deny ipv6 any any sequence 100
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh switch
Switch/Stack Mac Address : 00b1.e35a.cb00 - Local Mac Address
Mac persistency wait time: Indefinite
                                             H/W   Current
Switch#   Role    Mac Address     Priority Version  State 
-------------------------------------------------------------------------------------
*1       Active   00b1.e35a.cb00     15     V01     Ready                
 2       Standby  00b1.e35a.ad00     10     V01     Ready                



MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh switch stack-ports
                                 ^
% Invalid input detected at '^' marker.

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 4096
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh inventory
NAME: "c95xx Stack", DESCR: "c95xx Stack"
PID: C9500-16X         , VID: V01  , SN: FCW2233A6B9

NAME: "Switch 1", DESCR: "C9500-16X"
PID: C9500-16X         , VID: V01  , SN: FCW2233A6B9

NAME: "Switch 1 - Power Supply A", DESCR: "Switch 1 - Power Supply A"
PID: PWR-C4-950WAC-R   , VID: 000  , SN: APS222801SU

NAME: "Switch 1 - Power Supply B", DESCR: "Switch 1 - Power Supply B"
PID: PWR-C4-950WAC-R   , VID: 000  , SN: APS222801ST

NAME: "Switch 1 FRU Uplink Module 1", DESCR: "8x10G Uplink Module"
PID: C9500-NM-8X       , VID: V01  , SN: FOC222459T8

NAME: "Te1/1/1", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AGD163441RE     

NAME: "Te1/1/2", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD1816A2WK     

NAME: "Te1/1/3", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD1816A2ZY     

NAME: "Te1/1/4", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD1816A30W     

NAME: "Te1/1/5", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AGD16344ADE     

NAME: "Te1/1/6", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS214013JN     

NAME: "Te1/0/1", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390SN5     

NAME: "Te1/0/2", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390PYM     

NAME: "Te1/0/3", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390SMM     

NAME: "Te1/0/4", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390L42     

NAME: "Te1/0/5", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390LFE     

NAME: "Te1/0/6", DESCR: "1000BaseLX SFP"
PID: GLC-LH-SMD          , VID: V01  , SN: AVJ213231K7     

NAME: "Te1/0/7", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390SAN     

NAME: "Te1/0/8", DESCR: "1000BaseLX SFP"
PID: GLC-LH-SMD          , VID: V01  , SN: AVJ213231K3     

NAME: "Te1/0/9", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS17101MYH     

NAME: "Te1/0/10", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179L9X     

NAME: "Te1/0/11", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179L8L     

NAME: "Te1/0/12", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179L9N     

NAME: "Te1/0/13", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179L9W     

NAME: "Te1/0/14", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179R9C     

NAME: "Te1/0/15", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179BPA     

NAME: "Te1/0/16", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD2220971C     

NAME: "Switch 2", DESCR: "C9500-16X"
PID: C9500-16X         , VID: V01  , SN: FCW2233A61A

NAME: "Switch 2 - Power Supply A", DESCR: "Switch 2 - Power Supply A"
PID: PWR-C4-950WAC-R   , VID: 000  , SN: APS222801SQ

NAME: "Switch 2 - Power Supply B", DESCR: "Switch 2 - Power Supply B"
PID: PWR-C4-950WAC-R   , VID: 000  , SN: APS222801SP

NAME: "Switch 2 FRU Uplink Module 1", DESCR: "8x10G Uplink Module"
PID: C9500-NM-8X       , VID: V01  , SN: FOC22132JV5

NAME: "Te2/1/1", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD1816A30V     

NAME: "Te2/1/2", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AGD163444S7     

NAME: "Te2/1/3", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD1816A2ZX     

NAME: "Te2/1/4", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AGD163449E3     

NAME: "Te2/1/5", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: FNS162919CM     

NAME: "Te2/1/6", DESCR: "1000BaseLX SFP"
PID: GLC-LH-SMD          , VID: V01  , SN: OPM213101BT     

NAME: "Te2/0/1", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390XPM     

NAME: "Te2/0/2", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS2139031B     

NAME: "Te2/0/3", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS213906FK     

NAME: "Te2/0/4", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390ETJ     

NAME: "Te2/0/5", DESCR: "1000BaseLX SFP"
PID: GLC-LH-SMD          , VID: V01  , SN: AVJ213231ST     

NAME: "Te2/0/6", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: FNS21390Y6Q     

NAME: "Te2/0/7", DESCR: "1000BaseLX SFP"
PID: GLC-LH-SMD          , VID: V01  , SN: AVJ213233DE     

NAME: "Te2/0/8", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: AGJ1816RD07     

NAME: "Te2/0/9", DESCR: "1000BaseSX SFP"
PID: GLC-SX-MMD          , VID: V01  , SN: AGA1738RH2C     

NAME: "Te2/0/10", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179BRD     

NAME: "Te2/0/11", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179R8W     

NAME: "Te2/0/12", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD2220971D     

NAME: "Te2/0/13", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179BNP     

NAME: "Te2/0/14", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179R8V     

NAME: "Te2/0/15", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179L9S     

NAME: "Te2/0/16", DESCR: "SFP-10GBase-SR"
PID: SFP-10G-SR          , VID: V03  , SN: AVD22179R9D     


MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh hardware
Cisco IOS XE Software, Version 16.09.03
Cisco IOS Software [Fuji], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 16.9.3, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Wed 20-Mar-19 08:02 by mcpre


Cisco IOS-XE software, Copyright (c) 2005-2019 by cisco Systems, Inc.
All rights reserved.  Certain components of Cisco IOS-XE software are
licensed under the GNU General Public License ("GPL") Version 2.0.  The
software code licensed under GPL Version 2.0 is free software that comes
with ABSOLUTELY NO WARRANTY.  You can redistribute and/or modify such
GPL code under the terms of GPL Version 2.0.  For more details, see the
documentation or "License Notice" file accompanying the IOS-XE software,
or the applicable URL provided on the flyer accompanying the IOS-XE
software.


ROM: IOS-XE ROMMON
BOOTLDR: System Bootstrap, Version 16.9.1r [FC2], RELEASE SOFTWARE (P)

MX-LRS1-S951.group.wan uptime is 16 weeks, 2 days, 21 hours, 16 minutes
Uptime for this control processor is 16 weeks, 2 days, 21 hours, 19 minutes
System returned to ROM by Reload Command at 09:36:02 CDT Sun Oct 6 2019
System restarted at 14:40:38 CDT Sun Oct 6 2019
System image file is "flash:packages.conf"
Last reload reason: Reload Command



This product contains cryptographic features and is subject to United
States and local country laws governing import, export, transfer and
use. Delivery of Cisco cryptographic products does not imply
third-party authority to import, export, distribute or use encryption.
Importers, exporters, distributors and users are responsible for
compliance with U.S. and local country laws. By using this product you
agree to comply with applicable laws and regulations. If you are unable
to comply with U.S. and local laws, return this product immediately.

A summary of U.S. laws governing Cisco cryptographic products may be found at:
http://www.cisco.com/wwl/export/crypto/tool/stqrg.html

If you require further assistance please contact us by sending email to
export@cisco.com.


Technology Package License Information: 

------------------------------------------------------------------------------
Technology-package                                     Technology-package
Current                        Type                       Next reboot  
------------------------------------------------------------------------------
network-advantage   	Smart License                 	 network-advantage   
dna-advantage       	Subscription Smart License    	 dna-advantage                 


Smart Licensing Status: UNREGISTERED/EVAL EXPIRED

cisco C9500-16X (X86) processor with 1419496K/6147K bytes of memory.
Processor board ID FCW2233A6B9
24 Virtual Ethernet interfaces
48 Ten Gigabit Ethernet interfaces
4 Forty Gigabit Ethernet interfaces
2048K bytes of non-volatile configuration memory.
16777216K bytes of physical memory.
1638400K bytes of Crash Files at crashinfo:.
1638400K bytes of Crash Files at crashinfo-2:.
11264000K bytes of Flash at flash:.
11264000K bytes of Flash at flash-2:.
0K bytes of WebUI ODM Files at webui:.

Base Ethernet MAC Address          : 00:b1:e3:5a:cb:00
Motherboard Assembly Number        : 73-18709-01
Motherboard Serial Number          : FOC222842XY
Model Revision Number              : A0
Motherboard Revision Number        : A0
Model Number                       : C9500-16X
System Serial Number               : FCW2233A6B9


Switch Ports Model              SW Version        SW Image              Mode   
------ ----- -----              ----------        ----------            ----   
*    1 26    C9500-16X          16.9.3            CAT9K_IOSXE           INSTALL
     2 26    C9500-16X          16.9.3            CAT9K_IOSXE           INSTALL


Switch 02
---------
Switch uptime                      : 16 weeks, 2 days, 21 hours, 17 minutes 

Base Ethernet MAC Address          : 00:b1:e3:5a:ad:00
Motherboard Assembly Number        : 73-18709-01
Motherboard Serial Number          : FOC22300GK1
Model Revision Number              : A0
Motherboard Revision Number        : A0
Model Number                       : C9500-16X
System Serial Number               : FCW2233A61A

Configuration register is 0x102

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh vtp status
VTP Version capable             : 1 to 3
VTP version running             : 1
VTP Domain Name                 : 
VTP Pruning Mode                : Disabled
VTP Traps Generation            : Enabled
Device ID                       : 00b1.e35a.cb00
Configuration last modified by 172.27.172.1 at 10-7-19 03:53:20
Local updater ID is 172.27.188.129 on interface Vl3 (lowest numbered VLAN interface found)

Feature VLAN:
--------------
VTP Operating Mode                : Server
Maximum VLANs supported locally   : 1005
Number of existing VLANs          : 31
Configuration Revision            : 28
MD5 digest                        : 0xF5 0xDA 0xD8 0x8D 0x60 0xF4 0xF7 0x6C 
                                    0xAE 0x9A 0x91 0x18 0x5B 0x75 0x1D 0x82 
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh snmp user

User name: networking
Engine ID: 80000009030000B1E35ACB00
storage-type: nonvolatile	 active
Authentication Protocol: MD5
Privacy Protocol: None
Group-name: ReadOnlyLAN

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh snmp group
groupname: ReadOnlyLAN                      security model:v3 auth 
contextname: <no context specified>         storage-type: nonvolatile
readview : Read                             writeview: <no writeview specified>        
notifyview: <no notifyview specified>       
row status: active	access-list: snmp-list

groupname: ise_comunity                     security model:v1 
contextname: <no context specified>         storage-type: permanent
readview : <no readview specified>          writeview: <no writeview specified>        
notifyview: *tv.FFFFFFFF.FFFFFFFF.FFFFFFFF.F
row status: active

groupname: ise_comunity                     security model:v2c 
contextname: <no context specified>         storage-type: permanent
readview : <no readview specified>          writeview: <no writeview specified>        
notifyview: *tv.FFFFFFFF.FFFFFFFF.FFFFFFFF.F
row status: active

groupname: SMURFITKAPPARO                   security model:v1 
contextname: <no context specified>         storage-type: permanent
readview : v1default                        writeview: <no writeview specified>        
notifyview: <no notifyview specified>       
row status: active

groupname: SMURFITKAPPARO                   security model:v2c 
contextname: <no context specified>         storage-type: permanent
readview : v1default                        writeview: <no writeview specified>        
notifyview: <no notifyview specified>       
row status: active

groupname: skm-rotelecomIT                  security model:v1 
contextname: <no context specified>         storage-type: permanent
readview : v1default                        writeview: <no writeview specified>        
notifyview: <no notifyview specified>       
row status: active	access-list: 10

groupname: skm-rotelecomIT                  security model:v2c 
contextname: <no context specified>         storage-type: permanent
readview : v1default                        writeview: <no writeview specified>        
notifyview: <no notifyview specified>       
row status: active	access-list: 10

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh snmp host
Notification host: 172.27.158.17	udp-port: 162	type: trap
user: ise_comunity	security model: v2c

Notification host: 172.27.188.194	udp-port: 162	type: trap
user: ise_comunity	security model: v2c

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh ntp config
 ntp server 172.27.188.1 prefer
 ntp server 172.27.157.2
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh env all
Switch 1 FAN 1 is OK
Switch 1 FAN 2 is OK
Switch 1 FAN 3 is OK
Switch 1 FAN 4 is OK
Switch 1 FAN 5 is OK
FAN PS-1 is OK
FAN PS-2 is OK
Switch 2 FAN 1 is OK
Switch 2 FAN 2 is OK
Switch 2 FAN 3 is OK
Switch 2 FAN 4 is OK
Switch 2 FAN 5 is OK
FAN PS-1 is OK
FAN PS-2 is OK
Switch 1: SYSTEM TEMPERATURE is OK
Inlet Temperature Value: 20 Degree Celsius
Temperature State: GREEN
Yellow Threshold : 46 Degree Celsius
Red Threshold    : 56 Degree Celsius

Hotspot Temperature Value: 50 Degree Celsius
Temperature State: GREEN
Yellow Threshold : 105 Degree Celsius
Red Threshold    : 125 Degree Celsius
Switch 2: SYSTEM TEMPERATURE is OK
Inlet Temperature Value: 21 Degree Celsius
Temperature State: GREEN
Yellow Threshold : 46 Degree Celsius
Red Threshold    : 56 Degree Celsius

Hotspot Temperature Value: 46 Degree Celsius
Temperature State: GREEN
Yellow Threshold : 105 Degree Celsius
Red Threshold    : 125 Degree Celsius
SW  PID                 Serial#     Status           Sys Pwr  PoE Pwr  Watts
--  ------------------  ----------  ---------------  -------  -------  -----
1A  PWR-C4-950WAC-R     APS222801SU  OK              Good     Good     950 
1B  PWR-C4-950WAC-R     APS222801ST  OK              Good     Good     950 
2A  PWR-C4-950WAC-R     APS222801SQ  OK              Good     Good     950 
2B  PWR-C4-950WAC-R     APS222801SP  OK              Good     Good     950 

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh  processes cpu
CPU utilization for five seconds: 5%/3%; one minute: 4%; five minutes: 4%
 PID Runtime(ms)     Invoked      uSecs   5Sec   1Min   5Min TTY Process 
   1           2         106         18  0.00%  0.00%  0.00%   0 Chunk Manager    
   2      497147     1985236        250  0.00%  0.00%  0.00%   0 Load Meter       
   3           0           1          0  0.00%  0.00%  0.00%   0 PKI Trustpool    
   4          94         256        367  0.00%  0.00%  0.00%   0 RF Slave Main Th 
   5           0         113          0  0.00%  0.00%  0.00%   0 Retransmission o 
   6           0           2          0  0.00%  0.00%  0.00%   0 IPC ISSU Dispatc 
   7           6         138         43  0.00%  0.00%  0.00%   0 RO Notify Timers 
   8        1202      330522          3  0.00%  0.00%  0.00%   0 VIDB BACKGD MGR  
   9     3319127     1342131       2473  0.00%  0.02%  0.00%   0 Check heaps      
  10        9787      165467         59  0.00%  0.00%  0.00%   0 Pool Manager     
  11           0           1          0  0.00%  0.00%  0.00%   0 DiscardQ Backgro 
  12           0           2          0  0.00%  0.00%  0.00%   0 Timers           
  13          31        6747          4  0.00%  0.00%  0.00%   0 WATCH_AFS        
  14       46967     4895168          9  0.00%  0.00%  0.00%   0 IOSXE heartbeat  
  15      177641     5990616         29  0.07%  0.00%  0.00%   0 DB Lock Manager  
  16           0           1          0  0.00%  0.00%  0.00%   0 DB Notification  
  17           0          14          0  0.00%  0.00%  0.00%   0 ifIndex Receive  
  18           0           1          0  0.00%  0.00%  0.00%   0 Graceful Reload  
  19           0           1          0  0.00%  0.00%  0.00%   0 GR Dummy Client  
  20           0           1          0  0.00%  0.00%  0.00%   0 CEF MIB API      
  21           0          44          0  0.00%  0.00%  0.00%   0 IPC Apps Task    
  22        7077     1973762          3  0.00%  0.00%  0.00%   0 IPC Event Notifi 
  23       85673     9449629          9  0.00%  0.00%  0.00%   0 IPC Mcast Pendin 
  24           0           1          0  0.00%  0.00%  0.00%   0 Platform appsess 
  25         546      165343          3  0.00%  0.00%  0.00%   0 IPC Dynamic Cach 
  26        9287     1973773          4  0.00%  0.00%  0.00%   0 IPC Service NonC 
  27           0           1          0  0.00%  0.00%  0.00%   0 IPC Zone Manager 
  28       98372     9449634         10  0.00%  0.00%  0.00%   0 IPC Periodic Tim 
  29       79040     9449628          8  0.00%  0.00%  0.00%   0 IPC Deferred Por 
  30           0           1          0  0.00%  0.00%  0.00%   0 IPC Process leve 
  31      116941     2485827         47  0.00%  0.00%  0.00%   0 IPC Seat Manager 
  32        3983      566219          7  0.00%  0.00%  0.00%   0 IPC Check Queue  
  33          51         673         75  0.00%  0.00%  0.00%   0 IPC Seat RX Cont 
  34           0           1          0  0.00%  0.00%  0.00%   0 IPC Seat TX Cont 
  35       37989      992619         38  0.00%  0.00%  0.00%   0 IPC Keep Alive M 
  36      121073     1973910         61  0.00%  0.00%  0.00%   0 IPC Loadometer   
  37           0           1          0  0.00%  0.00%  0.00%   0 IPC Session Deta 
  38           0           1          0  0.00%  0.00%  0.00%   0 SENSOR-MGR event 
  39    11379644   175961649         64  0.07%  0.14%  0.15%   0 ARP Input        
  40      249584    10073557         24  0.00%  0.00%  0.00%   0 ARP Background   
  41           0           2          0  0.00%  0.00%  0.00%   0 ATM Idle Timer   
  42           0           1          0  0.00%  0.00%  0.00%   0 ATM ASYNC PROC   
  43           0           2          0  0.00%  0.00%  0.00%   0 DDR Timers       
  44         160         672        238  0.00%  0.00%  0.00%   0 Entity MIB API   
  45           0           1          0  0.00%  0.00%  0.00%   0 IFS Agent Manage 
  46           4          12        333  0.00%  0.00%  0.00%   0 PrstVbl          
  47           0           2          0  0.00%  0.00%  0.00%   0 Serial Backgroun 
  48           0           1          0  0.00%  0.00%  0.00%   0 RMI RM Notify Wa 
  49           0           1          0  0.00%  0.00%  0.00%   0 AAA_SERVER_DEADT 
  50           0           1          0  0.00%  0.00%  0.00%   0 Policy Manager   
  51           0           1          0  0.00%  0.00%  0.00%   0 IOSXE signals IO 
  52       98243     9926176          9  0.00%  0.00%  0.00%   0 GraphIt          
  53       60681     9668714          6  0.00%  0.00%  0.00%   0 Dynamic ARP Insp 
  54           0           1          0  0.00%  0.00%  0.00%   0 ARP Snoop        
  55           0          24          0  0.00%  0.00%  0.00%   0 SL Platform Back 
  56           0           1          0  0.00%  0.00%  0.00%   0 License IPC stat 
  57           0           1          0  0.00%  0.00%  0.00%   0 License IPC serv 
  58          29        2860         10  0.00%  0.00%  0.00%   0 Net Input        
  59           1           6        166  0.00%  0.00%  0.00%   0 client_entity_se 
  60           0           1          0  0.00%  0.00%  0.00%   0 SERIAL A'detect  
  61           0           2          0  0.00%  0.00%  0.00%   0 XML Proxy Client 
  62          41         954         42  0.00%  0.00%  0.00%   0 crypto sw pk pro 
  63           0           2          0  0.00%  0.00%  0.00%   0 License Client N 
  64         309        1196        258  0.00%  0.00%  0.00%   0 SAStorage        
  65           1           4        250  0.00%  0.00%  0.00%   0 SASConnect       
  66       73137     5078799         14  0.00%  0.00%  0.00%   0 SASRcvWQ         
  67           1           5        200  0.00%  0.00%  0.00%   0 SACConnect       
  68       40188     5083838          7  0.00%  0.00%  0.00%   0 SACRcvWQ         
  69           0           1          0  0.00%  0.00%  0.00%   0 Image License br 
  70       10605      330686         32  0.00%  0.00%  0.00%   0 Licensing Auto U 
  71           0           1          0  0.00%  0.00%  0.00%   0 License HA Consi 
  72           0           1          0  0.00%  0.00%  0.00%   0 Critical Bkgnd   
  73      204613     2195038         93  0.00%  0.00%  0.00%   0 Net Background   
  74           0          28          0  0.00%  0.00%  0.00%   0 IDB Work         
  75      351736    17909181         19  0.00%  0.00%  0.00%   0 Logger           
  76      781422     9673601         80  0.00%  0.00%  0.00%   0 TTY Background   
  77           0           2          0  0.00%  0.00%  0.00%   0 NGWC SGT-Cache p 
  78           2           9        222  0.00%  0.00%  0.00%   0 CTS CORE         
  79           1           6        166  0.00%  0.00%  0.00%   0 SASRcvWQWrk1     
  80           9         196         45  0.00%  0.00%  0.00%   0 SXP CORE         
  81     4501200    73496200         61  0.07%  0.05%  0.06%   0 IOSD ipc task    
  82       27955     2513432         11  0.00%  0.00%  0.00%   0 IOSD chasfs task 
  83           0          18          0  0.00%  0.00%  0.00%   0 NGWC OIR CC Proc 
  84           0           3          0  0.00%  0.00%  0.00%   0 MATM SPI client  
  85           5           9        555  0.00%  0.00%  0.00%   0 NGWC_OIRSHIM_TAS 
  86           0           1          0  0.00%  0.00%  0.00%   0 Ng3k envvar sync 
  87       14063     1412053          9  0.00%  0.00%  0.00%   0 REDUNDANCY FSM   
  88           0           1          0  0.00%  0.00%  0.00%   0 Punt FP Stats Du 
  89      351778     4895117         71  0.00%  0.00%  0.00%   0 PuntInject Keepa 
  90           4          45         88  0.00%  0.00%  0.00%   0 IF-MGR control p 
  91          71        2619         27  0.00%  0.00%  0.00%   0 IF-MGR event pro 
  92           0           7          0  0.00%  0.00%  0.00%   0 CTS HA           
  93           0          19          0  0.00%  0.00%  0.00%   0 CTS HA IPC flow  
  94           0           1          0  0.00%  0.00%  0.00%   0 CTS HA operation 
  95       54100     9668717          5  0.00%  0.00%  0.00%   0 Environmental Mo 
  96       61887     9668715          6  0.00%  0.00%  0.00%   0 RP HA Periodic   
  97           1          20         50  0.00%  0.00%  0.00%   0 cpf_msg_holdq_pr 
  98         227        4170         54  0.00%  0.00%  0.00%   0 cpf_msg_rcvq_pro 
  99      878615    16435096         53  0.00%  0.00%  0.00%   0 cpf_process_tpQ  
 100           0           1          0  0.00%  0.00%  0.00%   0 CONSOLE helper p 
 101        4270      931225          4  0.00%  0.00%  0.00%   0 EM_SHIM_TASK     
 102           2         105         19  0.00%  0.00%  0.00%   0 EM_SHIM_NOTIFY_T 
 103           0           1          0  0.00%  0.00%  0.00%   0 NGWC Configurati 
 104          31          18       1722  0.00%  0.00%  0.00%   0 MATM CF Flow Con 
 105       12278     1223172         10  0.00%  0.00%  0.00%   0 NGWC Learning Pr 
 106           0          18          0  0.00%  0.00%  0.00%   0 ACCESS_TUNNEL CF 
 107           0           1          0  0.00%  0.00%  0.00%   0 DHCP Snooping cl 
 108        3412       82720         41  0.00%  0.00%  0.00%   0 DHCP Snooping    
 109           0           1          0  0.00%  0.00%  0.00%   0 DHCP Snooping db 
 110           0           2          0  0.00%  0.00%  0.00%   0 DHCP Snooping HA 
 111           3           5        600  0.00%  0.00%  0.00%   0 STACK MGR Proces 
 112      530668     8756251         60  0.00%  0.00%  0.00%   0 ARP HA           
 113    21013882     5857439       3587  0.07%  0.25%  0.21%   0 Crimson flush tr 
 114         930        5287        175  0.00%  0.00%  0.00%   0 DBAL EVENTS      
 115     5208810    58356608         89  0.07%  0.08%  0.07%   0 Crimson config p 
 116           0          18          0  0.00%  0.00%  0.00%   0 NGWC ILP CF Proc 
 117           0           3          0  0.00%  0.00%  0.00%   0 CEF RRP RF waite 
 118      506553    12630506         40  0.00%  0.00%  0.00%   0 REDUNDANCY peer  
 119      960728    88602783         10  0.00%  0.00%  0.00%   0 100ms check      
 120           0           2          0  0.00%  0.00%  0.00%   0 Network-rf Notif 
 121           0           1          0  0.00%  0.00%  0.00%   0 XDR RRP RF waite 
 122     6928141   161489211         42  0.00%  0.09%  0.08%   0 IOSXE-RP Punt Se 
 123           0           1          0  0.00%  0.00%  0.00%   0 IOSXE-RP Punt IP 
 124    19101457  1310093620         14  0.95%  0.69%  0.61%   0 ACL Log Punt Ser 
 125           0           1          0  0.00%  0.00%  0.00%   0 ACL deny punt se 
 126           0           1          0  0.00%  0.00%  0.00%   0 OFSDN Punject Pr 
 127           1           4        250  0.00%  0.00%  0.00%   0 IOSXE-RP SPA TSM 
 128           1           3        333  0.00%  0.00%  0.00%   0 CWAN APS HA Proc 
 129           0           7          0  0.00%  0.00%  0.00%   0 DHCPD HA         
 130           2           7        285  0.00%  0.00%  0.00%   0 DHCPC HA         
 131           0           8          0  0.00%  0.00%  0.00%   0 DHCPv6 Relay HA  
 132           0           8          0  0.00%  0.00%  0.00%   0 DHCPv6 Server HA 
 133           0           3          0  0.00%  0.00%  0.00%   0 SISF HA Process  
 134           0           1          0  0.00%  0.00%  0.00%   0 Crypto PKI-HA    
 135       32260     2481458         13  0.07%  0.00%  0.00%   0 RF Master Main T 
 136      228377     2463760         92  0.00%  0.00%  0.00%   0 RF Master Status 
 137           1           7        142  0.00%  0.00%  0.00%   0 SACRcvWQWrk1     
 138           2           9        222  0.00%  0.00%  0.00%   0 SACRcvWQWrk2     
 139        1243      189638          6  0.00%  0.00%  0.00%   0 SACRcvWQWrk3     
 140        6396      189594         33  0.00%  0.00%  0.00%   0 SASRcvWQWrk2     
 141          45        9413          4  0.00%  0.00%  0.00%   0 BACK CHECK       
 142           0           1          0  0.00%  0.00%  0.00%   0 IPv6 Access Cont 
 143          40         156        256  0.00%  0.00%  0.00%   0 SAMsgThread      
 144      229932      992627        231  0.00%  0.00%  0.00%   0 Compute load avg 
 145     1166854      330873       3526  0.00%  0.01%  0.00%   0 Per-minute Jobs  
 146      298029     9926212         30  0.00%  0.00%  0.00%   0 Per-Second Jobs  
 147           0           1          0  0.00%  0.00%  0.00%   0 AggMgr Process   
 148           9         125         72  0.00%  0.00%  0.00%   0 ACT2 Crypto Engi 
 149           3         250         12  0.00%  0.00%  0.00%   0 Transport Port A 
 150      312087     2296291        135  0.00%  0.00%  0.00%   0 SFF8472          
 151           0           1          0  0.00%  0.00%  0.00%   0 IOSXE-RP FastPat 
 152           0           7          0  0.00%  0.00%  0.00%   0 NG Backup Interf 
 153           0           1          0  0.00%  0.00%  0.00%   0 ngpm main proces 
 154      898400    17284071         51  0.00%  0.00%  0.00%   0 PLFM-MGR IPC pro 
 155      979012     2481563        394  0.00%  0.00%  0.00%   0 FEP background p 
 156           0           1          0  0.00%  0.00%  0.00%   0 NGWC diag proces 
 157      274733      664013        413  0.00%  0.00%  0.00%   0 NGWC L2M         
 158      426464     2481564        171  0.00%  0.00%  0.00%   0 Thermal backgrou 
 159         451      165343          2  0.00%  0.00%  0.00%   0 MMN bkgrd proces 
 160           0           1          0  0.00%  0.00%  0.00%   0 NGWC DOT1X Proce 
 161           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Identity  
 162           0           1          0  0.00%  0.00%  0.00%   0 Remote Console P 
 163           0           1          0  0.00%  0.00%  0.00%   0 ACL Logging Proc 
 164           0           1          0  0.00%  0.00%  0.00%   0 NGWC_MACSEC_IOSD 
 165           0           1          0  0.00%  0.00%  0.00%   0 Inline Power     
 166           0           1          0  0.00%  0.00%  0.00%   0 Src Fltr backgro 
 167           0           1          0  0.00%  0.00%  0.00%   0 FPD Management P 
 168           0           1          0  0.00%  0.00%  0.00%   0 FPD Action Proce 
 169       80689     9449665          8  0.00%  0.00%  0.00%   0 fanrp_l2fib      
 170           0           4          0  0.00%  0.00%  0.00%   0 EEM ED MAT       
 171       91188     7150132         12  0.00%  0.00%  0.00%   0 EEM ED ND        
 172       46732     1973655         23  0.00%  0.00%  0.00%   0 MGMTE stats Proc 
 173           0           1          0  0.00%  0.00%  0.00%   0 Inter Chassis Pr 
 174      295753     1890988        156  0.00%  0.00%  0.00%   0 MATM RP Shim Pro 
 175           8           2       4000  0.00%  0.00%  0.00%   0 CWAN OIR Handler 
 176           0           1          0  0.00%  0.00%  0.00%   0 TP CUTOVER EVENT 
 177           0           1          0  0.00%  0.00%  0.00%   0 RTTYS Process    
 178           0           2          0  0.00%  0.00%  0.00%   0 Ethernet OAM Pro 
 179           0           2          0  0.00%  0.00%  0.00%   0 Ethernet CFM     
 180       10034      989639         10  0.00%  0.00%  0.00%   0 Ethchnl          
 181           0           2          0  0.00%  0.00%  0.00%   0 MVRP Process     
 182       45716      249723        183  0.00%  0.00%  0.00%   0 mcprp callhome p 
 183           0           1          0  0.00%  0.00%  0.00%   0 Switch Backup In 
 184           0           2          0  0.00%  0.00%  0.00%   0 SNMP DBAL WRAPPE 
 185           0           1          0  0.00%  0.00%  0.00%   0 SNMP DBAL Cache  
 186           0           1          0  0.00%  0.00%  0.00%   0 SNMP DBAL Cache  
 187           0          19          0  0.00%  0.00%  0.00%   0 EPM IAL Process  
 188           0           2          0  0.00%  0.00%  0.00%   0 REP Topology cha 
 189           0           2          0  0.00%  0.00%  0.00%   0 FMANRP REP Helpe 
 190           0           2          0  0.00%  0.00%  0.00%   0 REP LSL Hello Pr 
 191           2           2       1000  0.00%  0.00%  0.00%   0 SKA IPC process  
 192           0           3          0  0.00%  0.00%  0.00%   0 NGWC led process 
 193       34510      660420         52  0.00%  0.00%  0.00%   0 PI MATM Aging Pr 
 194        1363      165438          8  0.00%  0.00%  0.00%   0 Port-Security    
 195      382868     7428652         51  0.00%  0.00%  0.00%   0 DTP Protocol     
 196        1755        5167        339  0.00%  0.00%  0.00%   0 802.1x switch    
 197           0           1          0  0.00%  0.00%  0.00%   0 L2LISP XID Proce 
 198           0           2          0  0.00%  0.00%  0.00%   0 CDP Forward Proc 
 199      156925     9668714         16  0.00%  0.00%  0.00%   0 FHRP Main thread 
 200           0           1          0  0.00%  0.00%  0.00%   0 TRACK Main threa 
 201           0           1          0  0.00%  0.00%  0.00%   0 TRACK Client thr 
 202           0           1          0  0.00%  0.00%  0.00%   0 VRRP Main thread 
 203     1960008   136511982         14  0.00%  0.02%  0.00%   0 VRRS Main thread 
 204           3           1       3000  0.00%  0.00%  0.00%   0 PKI/SSL IPC proc 
 205           0           2          0  0.00%  0.00%  0.00%   0 PKI_SSL LSC Enro 
 206           0           2          0  0.00%  0.00%  0.00%   0 Dual-Active Reco 
 207           0           1          0  0.00%  0.00%  0.00%   0 PPCP RP Stats Ba 
 208           0           2          0  0.00%  0.00%  0.00%   0 CEF switching ba 
 209           1           1       1000  0.00%  0.00%  0.00%   0 ADJ NSF process  
 210           0           2          0  0.00%  0.00%  0.00%   0 MKA              
 211           0           1          0  0.00%  0.00%  0.00%   0 Spanning Tree St 
 212           5           2       2500  0.00%  0.00%  0.00%   0 PTP protocol eng 
 213           0           1          0  0.00%  0.00%  0.00%   0 L2FIB Timer Disp 
 214           1           1       1000  0.00%  0.00%  0.00%   0 MLRIB L2 Msg Thr 
 215           0           2          0  0.00%  0.00%  0.00%   0 NVE main thread  
 216      141125     9588956         14  0.00%  0.00%  0.00%   0 L2RIB Thread     
 217           0           1          0  0.00%  0.00%  0.00%   0 HQF TARGET DYNAM 
 218           0           2          0  0.00%  0.00%  0.00%   0 IPAM/ODAP Events 
 219     2448260   267422394          9  0.00%  0.01%  0.00%   0 IPAM Manager     
 220           0           2          0  0.00%  0.00%  0.00%   0 IPAM Events      
 221       30764      240146        128  0.00%  0.00%  0.00%   0 IP ARP Adjacency 
 222     2278847   267421933          8  0.07%  0.02%  0.00%   0 IP ARP Retry Age 
 223     1683085    26566302         63  0.15%  0.02%  0.00%   0 IP Input         
 224           0           1          0  0.00%  0.00%  0.00%   0 ICMP event handl 
 225       83540     9668750          8  0.00%  0.00%  0.00%   0 mDNS             
 226           0           1          0  0.00%  0.00%  0.00%   0 WSMA_NOTIFY      
 227           0           1          0  0.00%  0.00%  0.00%   0 AAA EPD HANDLER  
 228           0           1          0  0.00%  0.00%  0.00%   0 PM EPD API       
 229           0           2          0  0.00%  0.00%  0.00%   0 PPP SIP          
 230           0           2          0  0.00%  0.00%  0.00%   0 PPP Bind         
 231           0           2          0  0.00%  0.00%  0.00%   0 PPP IP Route     
 232          32        2664         12  0.00%  0.00%  0.00%   0 SSM connection m 
 233         534      165343          3  0.00%  0.00%  0.00%   0 SSS Manager      
 234           0           1          0  0.00%  0.00%  0.00%   0 SSS Policy Manag 
 235           0           1          0  0.00%  0.00%  0.00%   0 SSS Feature Mana 
 236      398931    36704448         10  0.00%  0.00%  0.00%   0 SSS Feature Time 
 237    33816009   180041876        187  0.23%  0.34%  0.29%   0 Spanning Tree    
 238         164        2574         63  0.00%  0.00%  0.00%   0 SpanTree Msg     
 239     2523725    89042277         28  0.07%  0.01%  0.00%   0 UDLD             
 240           0           1          0  0.00%  0.00%  0.00%   0 VRRS             
 241           0           1          0  0.00%  0.00%  0.00%   0 O-UNI Client Msg 
 242           0           2          0  0.00%  0.00%  0.00%   0 ATM OAM Input    
 243           0           2          0  0.00%  0.00%  0.00%   0 ATM OAM TIMER    
 244           2          11        181  0.00%  0.00%  0.00%   0 CCM              
 245           0          19          0  0.00%  0.00%  0.00%   0 CCM IPC flow con 
 246           0           1          0  0.00%  0.00%  0.00%   0 Timer handler fo 
 247           0           1          0  0.00%  0.00%  0.00%   0 Prepaid response 
 248           0           1          0  0.00%  0.00%  0.00%   0 Timed Policy act 
 249           0           1          0  0.00%  0.00%  0.00%   0 AAA response han 
 250           0           2          0  0.00%  0.00%  0.00%   0 Maintenance mode 
 251     2240880    10292067        217  0.00%  0.02%  0.00%   0 CDP Protocol     
 252      130437     2721408         47  0.00%  0.00%  0.00%   0 DiagCard2/-1     
 253        2691      165345         16  0.00%  0.00%  0.00%   0 Call Home Timer  
 254           0           1          0  0.00%  0.00%  0.00%   0 radius radsec cl 
 255          28        1622         17  0.00%  0.00%  0.00%   0 AAA Server       
 256           1           1       1000  0.00%  0.00%  0.00%   0 AAA ACCT Proc    
 257           0           1          0  0.00%  0.00%  0.00%   0 ACCT Periodic Pr 
 258           0           2          0  0.00%  0.00%  0.00%   0 AAA Dictionary R 
 259        3377        6701        503  0.00%  0.00%  0.00%   0 SpanTree Helper  
 260           0           5          0  0.00%  0.00%  0.00%   0 TurboACL         
 261           0           2          0  0.00%  0.00%  0.00%   0 TurboACL chunk   
 262           0           1          0  0.00%  0.00%  0.00%   0 IPv6 ping proces 
 263           0           1          0  0.00%  0.00%  0.00%   0 st_pw_oam        
 264           0           1          0  0.00%  0.00%  0.00%   0 AC Switch        
 265           0           1          0  0.00%  0.00%  0.00%   0 LSP Tunnel FRR   
 266           0           3          0  0.00%  0.00%  0.00%   0 MPLS Auto-Tunnel 
 267           0           2          0  0.00%  0.00%  0.00%   0 OCE punted Pkts  
 268           0           3          0  0.00%  0.00%  0.00%   0 PIM register asy 
 269           0           2          0  0.00%  0.00%  0.00%   0 RIB LM VALIDATE  
 270           0           1          0  0.00%  0.00%  0.00%   0 XDR background p 
 271       33383      461092         72  0.00%  0.00%  0.00%   0 XDR mcast        
 272           0           3          0  0.00%  0.00%  0.00%   0 XDR RP Ping Back 
 273        2677       27375         97  0.00%  0.00%  0.00%   0 XDR receive      
 274           0           4          0  0.00%  0.00%  0.00%   0 IPC LC Message H 
 275           0           1          0  0.00%  0.00%  0.00%   0 XDR RP Test Back 
 276         497      165343          3  0.00%  0.00%  0.00%   0 FRR Background P 
 277       24868      767801         32  0.00%  0.00%  0.00%   0 CEF background p 
 278           0           1          0  0.00%  0.00%  0.00%   0 fib_fib_bfd_sb e 
 279           0           1          0  0.00%  0.00%  0.00%   0 IP IRDP          
 280           1           2        500  0.00%  0.00%  0.00%   0 LSD HA Proc      
 281           2          27         74  0.00%  0.00%  0.00%   0 CEF RP Backgroun 
 282           0           2          0  0.00%  0.00%  0.00%   0 Routing Topology 
 283       11669       33375        349  0.00%  0.00%  0.00%   0 IP RIB Update    
 284        1076      167962          6  0.00%  0.00%  0.00%   0 IP Background    
 285          94        3196         29  0.00%  0.00%  0.00%   0 IP Connected Rou 
 286           0           2          0  0.00%  0.00%  0.00%   0 SPA XCVR SENSOR  
 287           0           2          0  0.00%  0.00%  0.00%   0 EFP Errd         
 288           0           2          0  0.00%  0.00%  0.00%   0 Ether EFP Proces 
 289       70156     9668708          7  0.00%  0.00%  0.00%   0 nbar-graph-sende 
 290       19570     1973763          9  0.07%  0.00%  0.00%   0 nbar-custom-prot 
 291        1745      165344         10  0.00%  0.00%  0.00%   0 nbar-auto-custom 
 292      124445     9668708         12  0.00%  0.00%  0.00%   0 nbar-sdavc-sched 
 293        1186      165344          7  0.00%  0.00%  0.00%   0 nbar-pp-monitor  
 294           0           2          0  0.00%  0.00%  0.00%   0 Ether Infra RP   
 295           2          19        105  0.00%  0.00%  0.00%   0 CFM HA IPC messa 
 296           0           1          0  0.00%  0.00%  0.00%   0 Ethernet PM Proc 
 297           0           2          0  0.00%  0.00%  0.00%   0 Ethernet PM Soft 
 298       19729     2541672          7  0.00%  0.00%  0.00%   0 Ethernet PM Moni 
 299           0           2          0  0.00%  0.00%  0.00%   0 AUTO LAG Protoco 
 300         446        8441         52  0.00%  0.00%  0.00%   0 IGMPSN L2MCM     
 301        1671       76049         21  0.00%  0.00%  0.00%   0 IGMPSN MRD       
 302      272507     4995993         54  0.00%  0.00%  0.00%   0 IGMPSN           
 303        2355      292054          8  0.00%  0.00%  0.00%   0 TCP Timer        
 304         231        2281        101  0.00%  0.00%  0.00%   0 TCP Protocols    
 305           0           1          0  0.00%  0.00%  0.00%   0 TCP HA PROC      
 306           0           2          0  0.00%  0.00%  0.00%   0 REP LSL Proc     
 307           0           2          0  0.00%  0.00%  0.00%   0 REP BPA/EPA Proc 
 308           1           2        500  0.00%  0.00%  0.00%   0 AN               
 309           0           1          0  0.00%  0.00%  0.00%   0 AN HELPER        
 310           0           1          0  0.00%  0.00%  0.00%   0 AN CONFIG DOWNLO 
 311       76275      466166        163  0.00%  0.00%  0.00%   0 NGWC stack power 
 312           0           7          0  0.00%  0.00%  0.00%   0 Socket Timers    
 313         406       33965         11  0.00%  0.00%  0.00%   0 HTTP CORE        
 314         110        8438         13  0.00%  0.00%  0.00%   0 MLDSN L2MCM      
 315           0           1          0  0.00%  0.00%  0.00%   0 MRD              
 316           0           1          0  0.00%  0.00%  0.00%   0 MLD_SNOOP        
 317           0           1          0  0.00%  0.00%  0.00%   0 IGMPQR           
 318           0           5          0  0.00%  0.00%  0.00%   0 IGMPSN-HA        
 319           0           4          0  0.00%  0.00%  0.00%   0 MLDSN-HA         
 320       32180       13406       2400  0.00%  0.00%  0.00%   0 VMATM Callback   
 321           0           1          0  0.00%  0.00%  0.00%   0 DAI Packet Proce 
 322          38        2706         14  0.00%  0.00%  0.00%   0 Flow Exporter Ti 
 323           0           2          0  0.00%  0.00%  0.00%   0 Flow Exporter Pa 
 324           0           1          0  0.00%  0.00%  0.00%   0 Tunnel FIB       
 325           0           1          0  0.00%  0.00%  0.00%   0 RETRY_REPOPULATE 
 326           0           1          0  0.00%  0.00%  0.00%   0 SEP_webui_wsma_h 
 327           0           3          0  0.00%  0.00%  0.00%   0 L2TRACE SERVER   
 328           0           2          0  0.00%  0.00%  0.00%   0 NEXTHOP_MAC_PROC 
 329           0           2          0  0.00%  0.00%  0.00%   0 Ewlc copy mqipc  
 330           0           2          0  0.00%  0.00%  0.00%   0 Ewlc cli mqipc h 
 331           0           2          0  0.00%  0.00%  0.00%   0 Tunnel           
 332       46101     4893816          9  0.00%  0.00%  0.00%   0 CEF: IPv4 proces 
 333           6          91         65  0.00%  0.00%  0.00%   0 ADJ background   
 334          12         171         70  0.00%  0.00%  0.00%   0 Collection proce 
 335       44281     4893845          9  0.00%  0.00%  0.00%   0 CEF: IPv6 proces 
 336           0           4          0  0.00%  0.00%  0.00%   0 XDR FOF process  
 337           0           5          0  0.00%  0.00%  0.00%   0 ADJ resolve proc 
 338           0          15          0  0.00%  0.00%  0.00%   0 HSRP HA          
 339           0          18          0  0.00%  0.00%  0.00%   0 SNMP Timers      
 340           0           1          0  0.00%  0.00%  0.00%   0 IP Traceroute    
 341           1           4        250  0.00%  0.00%  0.00%   0 BFD HA           
 342           0          19          0  0.00%  0.00%  0.00%   0 STP HA IPC flow  
 343           0           2          0  0.00%  0.00%  0.00%   0 L2FIB Event Disp 
 344           0           1          0  0.00%  0.00%  0.00%   0 EVPN main thread 
 345           0           1          0  0.00%  0.00%  0.00%   0 NVE MGR RWATCH T 
 346         107        7786         13  0.00%  0.00%  0.00%   0 EVPN Mgr Thread  
 347           0           3          0  0.00%  0.00%  0.00%   0 EVPN HA Bulk Syn 
 348           0           1          0  0.00%  0.00%  0.00%   0 SISF Switcher Th 
 349      605490    36709207         16  0.07%  0.01%  0.00%   0 SISF Main Thread 
 350           0           1          0  0.00%  0.00%  0.00%   0 COPS             
 351           0           3          0  0.00%  0.00%  0.00%   0 Service Routing  
 352          24        3402          7  0.00%  0.00%  0.00%   0 SR CapMan Proces 
 353          35        2657         13  0.00%  0.00%  0.00%   0 DNS Service      
 354          79        3546         22  0.00%  0.00%  0.00%   0 static           
 355           0           1          0  0.00%  0.00%  0.00%   0 IPv6 RIB Cleanup 
 356           0           3          0  0.00%  0.00%  0.00%   0 IPv6 RIB Event H 
 357           0           2          0  0.00%  0.00%  0.00%   0 SCTP Main Proces 
 358           0           2          0  0.00%  0.00%  0.00%   0 VPWS Thread      
 359           0           4          0  0.00%  0.00%  0.00%   0 EVPN Thread      
 360           0           1          0  0.00%  0.00%  0.00%   0 ac_atm_state_eve 
 361           0           1          0  0.00%  0.00%  0.00%   0 ac_atm_mraps_hsp 
 362           0           2          0  0.00%  0.00%  0.00%   0 VFI HA Bulk Sync 
 363           0           1          0  0.00%  0.00%  0.00%   0 AC HA Bulk Sync  
 364           0           3          0  0.00%  0.00%  0.00%   0 XC RIB MGR       
 365           0           3          0  0.00%  0.00%  0.00%   0 XC RIB HA Bulk S 
 366           0           3          0  0.00%  0.00%  0.00%   0 XC BGP SIG RIB H 
 367          42        1406         29  0.00%  0.00%  0.00%   0 PIM Snooping Pro 
 368           0           1          0  0.00%  0.00%  0.00%   0 NGWC-PIMSN Proce 
 369           0           1          0  0.00%  0.00%  0.00%   0 AAA System Acct  
 370           0           2          0  0.00%  0.00%  0.00%   0 DHCPv6 LQ client 
 371           0           1          0  0.00%  0.00%  0.00%   0 IPv6 Static Hand 
 372      104291      989644        105  0.00%  0.00%  0.00%   0 QoS stats proces 
 373           0           1          0  0.00%  0.00%  0.00%   0 AToM HA Bulk Syn 
 374           0          18          0  0.00%  0.00%  0.00%   0 AToM MGR HA IPC  
 375           0          21          0  0.00%  0.00%  0.00%   0 LDP HA           
 376           0           8          0  0.00%  0.00%  0.00%   0 TSPTUN HA        
 377           0           3          0  0.00%  0.00%  0.00%   0 RSVP HA Services 
 378           0           2          0  0.00%  0.00%  0.00%   0 TE NSR OOS DB Pr 
 379           1          18         55  0.00%  0.00%  0.00%   0 RSVP SYNC        
 380           0           2          0  0.00%  0.00%  0.00%   0 LFDp Input Proc  
 381           0           1          0  0.00%  0.00%  0.00%   0 MFI Comm RP Proc 
 382           0           2          0  0.00%  0.00%  0.00%   0 LFD Label Block  
 383         143        2627         54  0.00%  0.00%  0.00%   0 MVPN Mgr Process 
 384           0           2          0  0.00%  0.00%  0.00%   0 Multicast Offloa 
 385           0           1          0  0.00%  0.00%  0.00%   0 OFSDN IPC        
 386           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 387           0           7          0  0.00%  0.00%  0.00%   0 MRIB RP PROCESS  
 388           0           1          0  0.00%  0.00%  0.00%   0 MRIB RP Proxy    
 389         441       32546         13  0.00%  0.00%  0.00%   0 MFIB Master back 
 390           0           3          0  0.00%  0.00%  0.00%   0 MPLS VPN HA Clie 
 391      208391    18557324         11  0.00%  0.00%  0.00%   0 TLSCLIENT_PROCES 
 392           0           1          0  0.00%  0.00%  0.00%   0 DynCmd Package P 
 393           0           1          0  0.00%  0.00%  0.00%   0 mDNS snooping    
 394           0           1          0  0.00%  0.00%  0.00%   0 EEM MLANG MANAGE 
 395     1941971   136511904         14  0.00%  0.01%  0.00%   0 MMA DB TIMER     
 396           0           1          0  0.00%  0.00%  0.00%   0 EM Background Pr 
 397           0           1          0  0.00%  0.00%  0.00%   0 IDMGR CORE       
 398           0           1          0  0.00%  0.00%  0.00%   0 Key chain liveke 
 399           0           1          0  0.00%  0.00%  0.00%   0 SEP_webui_wsma_h 
 400           0           3          0  0.00%  0.00%  0.00%   0 EST Client       
 401           0           2          0  0.00%  0.00%  0.00%   0 AAA Cached Serve 
 402           0           2          0  0.00%  0.00%  0.00%   0 ENABLE AAA       
 403           0           3          0  0.00%  0.00%  0.00%   0 LDAP process     
 404           0           2          0  0.00%  0.00%  0.00%   0 LINE AAA         
 405         425       49125          8  0.00%  0.00%  0.00%   0 LOCAL AAA        
 406           0           2          0  0.00%  0.00%  0.00%   0 TPLUS            
 407           0           7          0  0.00%  0.00%  0.00%   0 MPLS Auto Mesh P 
 408           0           1          0  0.00%  0.00%  0.00%   0 Opaque Database  
 409      211127    18557323         11  0.00%  0.00%  0.00%   0 Timer Library    
 410           0           2          0  0.00%  0.00%  0.00%   0 Crypto Support   
 411           0           1          0  0.00%  0.00%  0.00%   0 IPSECv6 PS Proc  
 412         935        2758        339  0.00%  0.00%  0.00%   0 NIST rng proc    
 413           0           2          0  0.00%  0.00%  0.00%   0 MVRP SWITCH Proc 
 414           0          19          0  0.00%  0.00%  0.00%   0 COND_DEBUG HA IP 
 415           2           2       1000  0.00%  0.00%  0.00%   0 AAA Proxy        
 416           0           2          0  0.00%  0.00%  0.00%   0 REP Switch Helpe 
 417           6          11        545  0.00%  0.00%  0.00%   0 crypto engine pr 
 418           0           1          0  0.00%  0.00%  0.00%   0 RSA background p 
 419         127       11057         11  0.00%  0.00%  0.00%   0 Crypto CA        
 420           0           3          0  0.00%  0.00%  0.00%   0 Crypto PKI-CRL   
 421           0           1          0  0.00%  0.00%  0.00%   0 Crypto PKI-Rev-P 
 422           0           1          0  0.00%  0.00%  0.00%   0 PKI OCSP         
 423           0           1          0  0.00%  0.00%  0.00%   0 PKI Revocation   
 424           0           1          0  0.00%  0.00%  0.00%   0 pki_app          
 425           5           6        833  0.00%  0.00%  0.00%   0 TPS IPC Process  
 426           0           2          0  0.00%  0.00%  0.00%   0 MMON PROCESS     
 427     1930158   136512057         14  0.00%  0.02%  0.00%   0 MMA DP TIMER     
 428     1766589   291701325          6  0.00%  0.01%  0.00%   0 MMON MENG        
 429           0           1          0  0.00%  0.00%  0.00%   0 EWLC/AAA IPC pro 
 430           0           1          0  0.00%  0.00%  0.00%   0 EWLC/AAA IPC pro 
 431           0           2          0  0.00%  0.00%  0.00%   0 File Copy Handle 
 432           0           2          0  0.00%  0.00%  0.00%   0 Lawful Intercept 
 433           0           2          0  0.00%  0.00%  0.00%   0 LI messaging     
 434           0          24          0  0.00%  0.00%  0.00%   0 PIM HA           
 435           0           1          0  0.00%  0.00%  0.00%   0 encrypt proc     
 436      242373    10072346         24  0.00%  0.00%  0.00%   0 Crypto IKEv2     
 437           0           1          0  0.00%  0.00%  0.00%   0 IKEv2 AAA handle 
 438           0           3          0  0.00%  0.00%  0.00%   0 CRYPTO MAP FREE  
 439           0           1          0  0.00%  0.00%  0.00%   0 Crypto INT       
 440           0           2          0  0.00%  0.00%  0.00%   0 Crypto IKE Dispa 
 441           0           3          0  0.00%  0.00%  0.00%   0 Crypto IKMP      
 442           0           1          0  0.00%  0.00%  0.00%   0 IPSEC manual key 
 443           0           1          0  0.00%  0.00%  0.00%   0 IPsec vesen keym 
 444        5763      495545         11  0.00%  0.00%  0.00%   0 IPSEC key engine 
 445           0           1          0  0.00%  0.00%  0.00%   0 Crypto ACL       
 446           0           1          0  0.00%  0.00%  0.00%   0 Crypto PAS Proc  
 447           0           1          0  0.00%  0.00%  0.00%   0 IPSec background 
 448          63        2511         25  0.00%  0.00%  0.00%   0 VTP Trap Process 
 449     1569515    14868048        105  0.07%  0.02%  0.00%   0 LLDP Protocol    
 450           0           1          0  0.00%  0.00%  0.00%   0 Connection Mgr   
 451        7132      792169          9  0.00%  0.00%  0.00%   0 dhcp snooping sw 
 452          16        1501         10  0.00%  0.00%  0.00%   0 ASP Process Crea 
 453           0           1          0  0.00%  0.00%  0.00%   0 Licensing MIB pr 
 454           0           4          0  0.00%  0.00%  0.00%   0 EEM GED BINOS PR 
 455           1           2        500  0.00%  0.00%  0.00%   0 AUTO DEPLOY PROC 
 456           0           2          0  0.00%  0.00%  0.00%   0 DHCP Security He 
 457         930       11996         77  0.00%  0.00%  0.00%   0 SpanTree Flush   
 458           0           1          0  0.00%  0.00%  0.00%   0 NCD Process      
 459           0           1          0  0.00%  0.00%  0.00%   0 Online Diag EEM  
 460         887      165343          5  0.00%  0.00%  0.00%   0 SPA ENTITY Proce 
 461           0           1          0  0.00%  0.00%  0.00%   0 dcm_cli_engine   
 462           0           3          0  0.00%  0.00%  0.00%   0 dcm_cli_provider 
 463           0           4          0  0.00%  0.00%  0.00%   0 DCM Core Thread  
 464      328067    20341957         16  0.00%  0.00%  0.00%   0 EEM ED Syslog    
 465           2           4        500  0.00%  0.00%  0.00%   0 EEM ED Generic   
 466           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Routing   
 467      196297       63445       3093  0.00%  0.00%  0.00%   0 Syslog Traps     
 468           0           4          0  0.00%  0.00%  0.00%   0 Process to do EH 
 469           5         771          6  0.00%  0.00%  0.00%   0 AAA SEND STOP EV 
 470           0           1          0  0.00%  0.00%  0.00%   0 Test AAA Client  
 471           0           1          0  0.00%  0.00%  0.00%   0 IKEv2 FlexVPN Pr 
 472           0           1          0  0.00%  0.00%  0.00%   0 IKEv2 Cluster Lo 
 473           0           1          0  0.00%  0.00%  0.00%   0 ISSU Utility Pro 
 474      756598    44223310         17  0.00%  0.00%  0.00%   0 PM Callback      
 475           0          14          0  0.00%  0.00%  0.00%   0 SPAN switch      
 476           1           3        333  0.00%  0.00%  0.00%   0 WSMAN Process    
 477         990      165343          5  0.00%  0.00%  0.00%   0 FNF background p 
 478         435        1273        341  0.07%  0.26%  0.09%   1 SSH Process      
 479           0           1          0  0.00%  0.00%  0.00%   0 LICENSE AGENT    
 480          40        1375         29  0.00%  0.00%  0.00%   0 EEM Server       
 481           0           4          0  0.00%  0.00%  0.00%   0 EEM ED RPC       
 482           0           1          0  0.00%  0.00%  0.00%   0 IP SLAs ICMP Jit 
 483           0           1          0  0.00%  0.00%  0.00%   0 EPC WS PI suppor 
 484           0           1          0  0.00%  0.00%  0.00%   0 EPC WS PD suppor 
 485         434        1069        405  0.00%  0.00%  0.00%   0 DCM snmp dp Thre 
 486           0           3          0  0.00%  0.00%  0.00%   0 snmp dcm ma shim 
 487       12862      989647         12  0.00%  0.00%  0.00%   0 Bulkstat-Client  
 488           1          25         40  0.00%  0.00%  0.00%   0 Call Home proces 
 489           0           1          0  0.00%  0.00%  0.00%   0 Call Home DS     
 490           0           1          0  0.00%  0.00%  0.00%   0 Call Home DSfile 
 491           0           3          0  0.00%  0.00%  0.00%   0 EEM Policy Direc 
 492           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 493           0           4          0  0.00%  0.00%  0.00%   0 EEM ED CLI       
 494           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Counter   
 495           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Interface 
 496           0           4          0  0.00%  0.00%  0.00%   0 EEM ED IOSWD     
 497           0           4          0  0.00%  0.00%  0.00%   0 EEM ED None      
 498           0          10          0  0.00%  0.00%  0.00%   0 EEM ED OIR       
 499           1          18         55  0.00%  0.00%  0.00%   0 EEM ED RF        
 500           0           4          0  0.00%  0.00%  0.00%   0 EEM ED SNMP      
 501           0           4          0  0.00%  0.00%  0.00%   0 EEM ED SNMP Obje 
 502           0           4          0  0.00%  0.00%  0.00%   0 EEM ED SNMP Noti 
 503        4791      176779         27  0.00%  0.00%  0.00%   0 EEM ED Timer     
 504           0           6          0  0.00%  0.00%  0.00%   0 EEM ED Test      
 505           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Config    
 506           0           6          0  0.00%  0.00%  0.00%   0 EEM ED Env       
 507           0           4          0  0.00%  0.00%  0.00%   0 EEM ED DS        
 508           0           6          0  0.00%  0.00%  0.00%   0 EEM ED CRASH     
 509           0           6          0  0.00%  0.00%  0.00%   0 EM ED GOLD       
 510      159237     8006022         19  0.00%  0.00%  0.00%   0 Syslog           
 511           1           8        125  0.00%  0.00%  0.00%   0 RFS server proce 
 512           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Nf        
 513           0           4          0  0.00%  0.00%  0.00%   0 EEM ED Ipsla     
 514           0           1          0  0.00%  0.00%  0.00%   0 NGWCRP_VPLS_MW   
 515           0          45          0  0.00%  0.00%  0.00%   0 RBM CORE         
 516           0           3          0  0.00%  0.00%  0.00%   0 CTS credentials  
 517          30        1257         23  0.00%  0.00%  0.00%   0 VLAN Manager     
 518           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 519           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 520           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 521           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 522           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 523           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 524           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 525           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 526           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 527           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 528           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 529           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 530           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 531           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 532           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 533           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 534           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 535           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 536           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 537           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 538           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 539           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 540           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 541           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 542           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 543           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 544           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 545           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 546           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 547           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 548           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 549           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 550           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 551           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 552           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 553           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 554           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 555           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 556           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 557           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 558           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 559           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 560           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 561           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 562           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 563           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 564           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 565           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 566           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 567           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 568           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 569           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 570           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 571           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 572           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 573           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 574           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 575           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 576           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 577           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 578           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 579           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 580           0           1          0  0.00%  0.00%  0.00%   0 IPC ISSU Version 
 581       80276      604753        132  0.00%  0.00%  0.00%   0 mdns Timer Proce 
 582           0           2          0  0.00%  0.00%  0.00%   0 MRIB Process     
 583          48        1639         29  0.00%  0.00%  0.00%   0 EEM Helper Threa 
 584           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 585           0           1          0  0.00%  0.00%  0.00%   0 HA-IDB-SYNC      
 586      362070    36206577         10  0.00%  0.00%  0.00%   0 CCM Subscriber P 
 587           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 588           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 589           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 590           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 591           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch po 
 592           0           1          0  0.00%  0.00%  0.00%   0 ONEP Dispatch XD 
 593           0           1          0  0.00%  0.00%  0.00%   0 XOS async sync X 
 594     1193436    45570237         26  0.07%  0.01%  0.00%   0 ONEP Network Ele 
 595          27        2760          9  0.00%  0.00%  0.00%   0 SSH Event handle 
 596     1922641    37897569         50  0.00%  0.02%  0.00%   0 LACP Protocol    
 597      202905     4659438         43  0.00%  0.00%  0.00%   0 DHCPD Receive    
 599           2          36         55  0.00%  0.00%  0.00%   0 KEYSTORE HA IPC  
 600    13537273   127942353        105  0.00%  0.02%  0.01%   0 IP SNMP          
 601     1904762    63303596         30  0.00%  0.01%  0.00%   0 PDU DISPATCHER   
 602    17073972    64573715        264  0.00%  0.03%  0.04%   0 SNMP ENGINE      
 603           0           2          0  0.00%  0.00%  0.00%   0 IP SNMPV6        
 604           0           1          0  0.00%  0.00%  0.00%   0 SNMP ConfCopyPro 
 605      211946      224840        942  0.00%  0.00%  0.00%   0 SNMP Traps       
 606      716856    16364086         43  0.00%  0.00%  0.00%   0 NTP              
 607        1247       85445         14  0.00%  0.00%  0.00%   0 DHCPD Timer      
 608         655      165343          3  0.00%  0.00%  0.00%   0 DHCPD Database   
 609      389001    10644165         36  0.00%  0.00%  0.00%   0 OSPF-1 Router    
 611      221186     6069298         36  0.00%  0.00%  0.00%   0 OSPF-1 Hello     
 612      118406     2811510         42  0.00%  0.00%  0.00%   0 DiagCard1/-1     
 616       55023     9668542          5  0.00%  0.00%  0.00%   0 Inline power inc
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh flash
                            ^
% Invalid input detected at '^' marker.

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh clock
.10:57:02.242 CDT Wed Jan 29 2020
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh logg
Syslog logging: enabled (0 messages dropped, 68 messages rate-limited, 4143964 flushes, 0 overruns, xml disabled, filtering disabled)

No Active Message Discriminator.



No Inactive Message Discriminator.


    Console logging: level debugging, 104865132 messages logged, xml disabled,
                     filtering disabled
    Monitor logging: level debugging, 26712 messages logged, xml disabled,
                     filtering disabled
    Buffer logging:  level debugging, 61280610 messages logged, xml disabled,
                    filtering disabled
    Exception Logging: size (4096 bytes)
    Count and timestamp logging messages: disabled
    File logging: disabled
    Persistent logging: disabled

No active filter modules.

    Trap logging: level informational, 8345472 message lines logged
        Logging Source-Interface:       VRF Name:

Log Buffer (4096 bytes):
LOGP: list VOZ_IN permitted tcp 172.27.190.24(49257) -> 172.27.179.140(5060), 1 packet 
.Jan 29 16:56:24.652: %SEC-6-IPACCESSLOGP: list VOZ_OUT permitted udp 172.27.136.216(31255) -> 172.27.190.5(37687), 1 packet 
.Jan 29 16:56:25.656: %SEC-6-IPACCESSLOGP: list VOZ_OUT permitted udp 172.27.179.10(60389) -> 172.27.190.151(34369), 1 packet 
.Jan 29 16:56:26.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:27.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:28.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:29.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:30.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:31.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:32.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:34.806: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.4(12127) -> 172.27.179.12(16845), 1 packet 
.Jan 29 16:56:35.811: %SEC-6-IPACCESSLOGP: list VOZ_OUT permitted udp 172.27.179.10(36588) -> 172.27.190.252(44026), 1 packet 
.Jan 29 16:56:36.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:37.902: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.159(37509) -> 172.27.179.10(69), 1 packet 
.Jan 29 16:56:38.987: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.59(51870) -> 172.27.179.140(5060), 1 packet 
.Jan 29 16:56:41.202: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.252(35085) -> 172.27.179.10(69), 1 packet 
.Jan 29 16:56:42.722: %SEC-6-IPACCESSLOGRL: access-list logging rate-limited or missed 23132 packets
.Jan 29 16:56:42.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:43.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:44.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:46.106: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.4(12127) -> 172.27.179.12(16845), 1 packet 
.Jan 29 16:56:47.224: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.151(46062) -> 172.27.179.10(69), 1 packet 
.Jan 29 16:56:48.750: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.159(41566) -> 172.27.179.10(69), 1 packet 
.Jan 29 16:56:49.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:50.812: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:51.819: %SEC-6-IPACCESSLOGDP: list VOZ_OUT permitted icmp 172.27.179.12 -> 172.27.190.4 (3/3), 1 packet 
.Jan 29 16:56:52.820: %SEC-6-IPACCESSLOGP: list VOZ_OUT permitted udp 172.27.179.10(51045) -> 172.27.190.151(36443), 1 packet 
.Jan 29 16:56:53.953: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.218(50326) -> 172.27.179.10(5060), 1 packet 
.Jan 29 16:56:55.267: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.5(41924) -> 172.27.179.10(1720), 1 packet 
.Jan 29 16:56:56.565: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.64(51367) -> 172.27.179.10(5060), 1 packet 
.Jan 29 16:56:57.568: %SEC-6-IPACCESSLOGP: list VOZ_OUT permitted udp 172.27.179.10(53959) -> 172.27.190.252(56700), 1 packet 
.Jan 29 16:56:58.595: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.57(50406) -> 172.27.179.140(5060), 1 packet 
.Jan 29 16:56:59.606: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted udp 172.27.190.159(45022) -> 172.27.179.10(69), 1 packet 
.Jan 29 16:57:00.661: %SEC-6-IPACCESSLOGP: list VOZ_IN permitted tcp 172.27.190.4(58704) -> 172.27.179.11(2000), 1 packet 
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#sh ver
Cisco IOS XE Software, Version 16.09.03
Cisco IOS Software [Fuji], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 16.9.3, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Wed 20-Mar-19 08:02 by mcpre


Cisco IOS-XE software, Copyright (c) 2005-2019 by cisco Systems, Inc.
All rights reserved.  Certain components of Cisco IOS-XE software are
licensed under the GNU General Public License ("GPL") Version 2.0.  The
software code licensed under GPL Version 2.0 is free software that comes
with ABSOLUTELY NO WARRANTY.  You can redistribute and/or modify such
GPL code under the terms of GPL Version 2.0.  For more details, see the
documentation or "License Notice" file accompanying the IOS-XE software,
or the applicable URL provided on the flyer accompanying the IOS-XE
software.


ROM: IOS-XE ROMMON
BOOTLDR: System Bootstrap, Version 16.9.1r [FC2], RELEASE SOFTWARE (P)

MX-LRS1-S951.group.wan uptime is 16 weeks, 2 days, 21 hours, 16 minutes
Uptime for this control processor is 16 weeks, 2 days, 21 hours, 19 minutes
System returned to ROM by Reload Command at 09:36:02 CDT Sun Oct 6 2019
System restarted at 14:40:38 CDT Sun Oct 6 2019
System image file is "flash:packages.conf"
Last reload reason: Reload Command



This product contains cryptographic features and is subject to United
States and local country laws governing import, export, transfer and
use. Delivery of Cisco cryptographic products does not imply
third-party authority to import, export, distribute or use encryption.
Importers, exporters, distributors and users are responsible for
compliance with U.S. and local country laws. By using this product you
agree to comply with applicable laws and regulations. If you are unable
to comply with U.S. and local laws, return this product immediately.

A summary of U.S. laws governing Cisco cryptographic products may be found at:
http://www.cisco.com/wwl/export/crypto/tool/stqrg.html

If you require further assistance please contact us by sending email to
export@cisco.com.


Technology Package License Information: 

------------------------------------------------------------------------------
Technology-package                                     Technology-package
Current                        Type                       Next reboot  
------------------------------------------------------------------------------
network-advantage   	Smart License                 	 network-advantage   
dna-advantage       	Subscription Smart License    	 dna-advantage                 


Smart Licensing Status: UNREGISTERED/EVAL EXPIRED

cisco C9500-16X (X86) processor with 1419496K/6147K bytes of memory.
Processor board ID FCW2233A6B9
24 Virtual Ethernet interfaces
48 Ten Gigabit Ethernet interfaces
4 Forty Gigabit Ethernet interfaces
2048K bytes of non-volatile configuration memory.
16777216K bytes of physical memory.
1638400K bytes of Crash Files at crashinfo:.
1638400K bytes of Crash Files at crashinfo-2:.
11264000K bytes of Flash at flash:.
11264000K bytes of Flash at flash-2:.
0K bytes of WebUI ODM Files at webui:.

Base Ethernet MAC Address          : 00:b1:e3:5a:cb:00
Motherboard Assembly Number        : 73-18709-01
Motherboard Serial Number          : FOC222842XY
Model Revision Number              : A0
Motherboard Revision Number        : A0
Model Number                       : C9500-16X
System Serial Number               : FCW2233A6B9


Switch Ports Model              SW Version        SW Image              Mode   
------ ----- -----              ----------        ----------            ----   
*    1 26    C9500-16X          16.9.3            CAT9K_IOSXE           INSTALL
     2 26    C9500-16X          16.9.3            CAT9K_IOSXE           INSTALL


Switch 02
---------
Switch uptime                      : 16 weeks, 2 days, 21 hours, 17 minutes 

Base Ethernet MAC Address          : 00:b1:e3:5a:ad:00
Motherboard Assembly Number        : 73-18709-01
Motherboard Serial Number          : FOC22300GK1
Model Revision Number              : A0
Motherboard Revision Number        : A0
Model Number                       : C9500-16X
System Serial Number               : FCW2233A61A

Configuration register is 0x102

MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#
MX-LRS1-S951.group.wan#exit


[SSH] INFO: DISCONNECT

***
*** DISCONNECT
*** time 10:56:51
***
