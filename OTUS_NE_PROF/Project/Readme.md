net 49.5042.0000.0000.0000.0015.00
net 49.5042.0000.0000.0000.0016.00
net 49.5042.0000.0000.0000.0017.00
net 49.2405.0000.0000.0000.0018.00

int lo0
ip router isis 1

isis circuit-type level-1-2


int e0/1
ip router isis 1
isis circuit-type level-2
int e0/0
ip router isis 1
isis circuit-type level-2



neighbor 1.1.1.5 remote-as 65262
neighbor 1.1.1.6 remote-as 65262
neighbor 1.1.1.7 remote-as 65262
neighbor 1.1.1.8 remote-as 65262

neighbor 1.1.1.5 update-source lo0
neighbor 1.1.1.6 update-source lo0
neighbor 1.1.1.7 update-source lo0
neighbor 1.1.1.8 update-source lo0


router bgp 65041
 neighbor RR peer-group
 neighbor RR remote-as 65042
 neighbor RR update-source Loopback0
 neighbor RR next-hop-self
 neighbor 1.1.1.15 peer-group RR
 neighbor 1.1.1.17 peer-group RR
 neighbor 1.1.1.18 peer-group RR
 neighbor 13.13.13.1 remote-as 301
 neighbor 111.111.111.2 remote-as 2042

router bgp 65042
neighbor 1.1.1.16 remote-as 65042
neighbor 1.1.1.16 update-source Loopback0
neighbor 1.1.1.16 next-hop-self


int e0/0
ip ospf 1 a 0

en
conf t
router ospf 1
net 1.1.1. 0.0.0.0 a 0



router bgp 65262

neighbor 1.1.1.5 next-hop-self
neighbor 1.1.1.6 next-hop-self
neighbor 1.1.1.7 next-hop-self
neighbor 1.1.1.8 next-hop-self


interface range e0/2-3
channel-protocol lacp
channel-group 1 mode passive
exit

interface port-channel 1 
switchport trunk encapsulation dot1q
switchport mode trunk
