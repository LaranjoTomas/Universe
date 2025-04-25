# router_e1

configure terminal
interface FastEthernet 0/1
description To-LB1
ip address 192.168.1.3 255.255.255.0
no shutdown
exit

interface FastEthernet 0/0
 description To-Stateless-FW1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

interface FastEthernet 1/0
 description To-Stateless-FW2
 ip address 192.168.3.2 255.255.255.0
 no shutdown
exit

ip route 200.0.0.0 255.255.255.0 192.168.1.2
ip route 10.0.0.0 255.0.0.0 192.168.1.2
exit
### Security ACLs
configure terminal
ip access-list extended INTERNET-IN
 permit tcp any 200.0.0.0 0.0.0.255 eq 443
 permit tcp any 200.0.0.0 0.0.0.255 eq 25
 permit tcp any 200.0.0.0 0.0.0.255 eq 993
 permit udp any 200.0.0.0 0.0.0.255 eq 53
 deny ip any any log
 
exit
interface FastEthernet 0/0
 ip access-group INTERNET-IN in
end
write

# router_e2

interface FastEthernet 0/0
 description To-Stateless-FW2
 ip address 192.168.4.1 255.255.255.0
 no shutdown
exit

interface FastEthernet 1/0
 description To-Stateless-FW1
 ip address 192.168.2.2 255.255.255.0
 no shutdown
exit

interface FastEthernet 0/1
 description To-LB1
 ip address 192.168.2.3 255.255.255.0
 no shutdown
exit

ip route 200.0.0.0 255.255.255.0 192.168.2.2
ip route 10.0.0.0 255.0.0.0 192.168.2.2
exit
### Security ACLs

ip access-list extended INTERNET-IN
 permit tcp any 200.0.0.0 0.0.0.255 eq 443
 permit tcp any 200.0.0.0 0.0.0.255 eq 25
 permit tcp any 200.0.0.0 0.0.0.255 eq 993
 permit udp any 200.0.0.0 0.0.0.255 eq 53
 deny ip any any log
exit

interface FastEthernet 0/0
 ip access-group INTERNET-IN in
end
write
# Stateless Firewall 1
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot

configure
 set system host-name SFW1
 ## Interfaces
set interfaces ethernet eth0 address 100.0.0.1/24    
set interfaces ethernet eth1 address 192.168.1.2/24
set interfaces ethernet eth2 address 192.168.2.2/24
commit; exit
## Static routes
configure
set protocols static route 0.0.0.0/0 next-hop 100.0.0.254
set protocols static route 200.0.0.0/24 next-hop 100.0.0.254
set protocols static route 10.0.0.0/8 next-hop 100.0.0.254

 commit; exit
 save

# Stateless Firewall 2

sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot

configure
 set system host-name SFW2
 set interfaces ethernet eth0 address 100.0.0.2/24    
 set interfaces ethernet eth1 address 192.168.2.2/24   
 set interfaces ethernet eth2 address 192.168.1.2/24
 commit
configure
 set protocols static route 0.0.0.0/0 next-hop 100.0.0.254
set protocols static route 200.0.0.0/24 next-hop 100.0.0.254
set protocols static route 10.0.0.0/8 next-hop 100.0.0.254
 commit
 save

# LB1
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot
### Interfaces
configure
set system host-name LB1
set interfaces ethernet eth0 address 172.16.1.2/24
set interfaces ethernet eth0 description 'To-FW1'
set interfaces ethernet eth1 address 172.16.1.3/24
set interfaces ethernet eth1 description 'To-FW2'
set interfaces ethernet eth2 address 192.168.1.2/24
set interfaces ethernet eth2 description 'To-Router-E1'
set interfaces ethernet eth3 address 192.168.2.2/24
set interfaces ethernet eth3 description 'To-Router-E2'
set interfaces ethernet eth4 description 'HA-Link'
commit
### Load Balancing Configuration
set load-balancing wan interface-health eth1 nexthop 192.168.10.2
set load-balancing wan interface-health eth2 nexthop 192.168.12.2
set load-balancing wan rule 1 inbound-interface eth0
set load-balancing wan rule 1 interface eth1 weight 1
set load-balancing wan rule 1 interface eth2 weight 1
set load-balancing wan sticky-connections inbound
commit
### High Availability Configuration
set high-availability vrrp group LB-Cluster vrid 10
set high-availability vrrp group LB-Cluster interface eth0
set high-availability vrrp group LB-Cluster virtual-address 192.168.1.10/24
set high-availability vrrp group LB-Cluster priority 200
set high-availability vrrp sync-group LB-Cluster member LB-Cluster
commit
### NAT Configuration
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 10.0.0.0/8
set nat source rule 100 translation address masquerade
commit
### Static Routes
set protocols static route 0.0.0.0/0 next-hop 192.168.1.1
set protocols static route 200.0.0.0/24 next-hop 192.168.10.2
set protocols static route 10.0.0.0/8 next-hop 192.168.10.2
commit;save;exit

# LB2
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot
### Interfaces
configure
set system host-name LB2
set interfaces ethernet eth0 address 192.168.31.1/24
set interfaces ethernet eth0 description "To-FW1"
set interfaces ethernet eth1 address 192.168.11.1/24
set interfaces ethernet eth1 description "To-FW2"
set interfaces ethernet eth2 address 192.168.30.1/24
set interfaces ethernet eth2 description "To-SWL3‑C1"
set interfaces ethernet eth3 address 192.168.31.1/24
set interfaces ethernet eth3 description "To-SWL3‑C2"
commit
### Load Balancing Configuration
set load-balancing wan interface-health eth1 nexthop 192.168.11.2
set load-balancing wan interface-health eth2 nexthop 192.168.13.2
set load-balancing wan rule 1 inbound-interface eth0
set load-balancing wan rule 1 interface eth1 weight 1
set load-balancing wan rule 1 interface eth2 weight 1
set load-balancing wan sticky-connections inbound
commit
### High Availability Configuration
set high-availability vrrp group LB-Cluster vrid 10
set high-availability vrrp group LB-Cluster interface eth0
set high-availability vrrp group LB-Cluster virtual-address 192.168.2.10/24
set high-availability vrrp group LB-Cluster priority 100
set high-availability vrrp sync-group LB-Cluster member LB-Cluster
commit
### NAT Configuration
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 10.0.0.0/8
set nat source rule 100 translation address masquerade
commit
### Static Routes
set protocols static route 0.0.0.0/0 next-hop 192.168.2.1
set protocols static route 200.0.0.0/24 next-hop 192.168.11.2
set protocols static route 10.0.0.0/8 next-hop 192.168.11.2
commit; save; exit
# LB3
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot

configure
 set system host-name LB3
 ## From FW1 & FW2
 set interfaces ethernet eth0 address 192.168.21.1/24
 set interfaces ethernet eth0 description 'To-FW1'
 set interfaces ethernet eth1 address 192.168.21.2/24
 set interfaces ethernet eth1 description 'To-FW2'
 ## To DMZ switch
 set interfaces ethernet eth2 address 200.0.0.1/24
 set interfaces ethernet eth2 description 'To-DMZ'
 commit; save; exit

# FW1
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot

configure
set system host-name FW1
### Interfaces
set interfaces ethernet eth0 address 192.168.10.2/24
set interfaces ethernet eth0 description 'To-LB1'
set interfaces ethernet eth1 address 192.168.12.2/24
set interfaces ethernet eth1 description 'To-LB2'
set interfaces ethernet eth2 address 192.168.21.1/24
set interfaces ethernet eth2 description 'To-LB3'
set interfaces ethernet eth5 address 172.16.2.1/24
commit
exit
### Security Zones
set zone-policy zone OUTSIDE description 'Internet Zone'
set zone-policy zone OUTSIDE interface eth0
set zone-policy zone OUTSIDE interface eth1
set zone-policy zone DMZ description 'DMZ Zone'
set zone-policy zone DMZ interface eth2
set zone-policy zone INSIDE description 'Internal Zone'
set zone-policy zone INSIDE interface eth3
set zone-policy zone INSIDE interface eth4
commit
exit
### Firewall Rules - OUTSIDE to DMZ
set firewall name OUTSIDE-DMZ rule 10 action accept
set firewall name OUTSIDE-DMZ rule 10 description "Allow HTTPS to DMZ"
set firewall name OUTSIDE-DMZ rule 10 destination port 443
set firewall name OUTSIDE-DMZ rule 10 protocol tcp
set firewall name OUTSIDE-DMZ rule 20 action accept
set firewall name OUTSIDE-DMZ rule 20 description "Allow SMTP to DMZ"
set firewall name OUTSIDE-DMZ rule 20 destination port 25
set firewall name OUTSIDE-DMZ rule 20 protocol tcp
set firewall name OUTSIDE-DMZ rule 30 action accept
set firewall name OUTSIDE-DMZ rule 30 description "Allow IMAP to DMZ"
set firewall name OUTSIDE-DMZ rule 30 destination port 993
set firewall name OUTSIDE-DMZ rule 30 protocol tcp
set firewall name OUTSIDE-DMZ rule 40 action accept
set firewall name OUTSIDE-DMZ rule 40 description "Allow DNS to DMZ"
set firewall name OUTSIDE-DMZ rule 40 destination port 53
set firewall name OUTSIDE-DMZ rule 40 protocol udp
set firewall name OUTSIDE-DMZ rule 999 action drop
set firewall name OUTSIDE-DMZ rule 999 description "Drop all other traffic"
commit
exit
### Firewall Rules - DMZ to OUTSIDE
set firewall name DMZ-OUTSIDE rule 10 action accept
set firewall name DMZ-OUTSIDE rule 10 description "Allow established connections"
set firewall name DMZ-OUTSIDE rule 10 state established enable
set firewall name DMZ-OUTSIDE rule 10 state related enable
set firewall name DMZ-OUTSIDE rule 999 action drop
set firewall name DMZ-OUTSIDE rule 999 description "Drop all other traffic"
commit
exit
### Firewall Rules - INSIDE to DMZ
set firewall name INSIDE-DMZ rule 10 action accept
set firewall name INSIDE-DMZ rule 10 description "Allow all from INSIDE to DMZ"
commit
exit
### Firewall Rules - DMZ to INSIDE
set firewall name DMZ-INSIDE rule 10 action accept
set firewall name DMZ-INSIDE rule 10 description "Allow established connections"
set firewall name DMZ-INSIDE rule 10 state established enable
set firewall name DMZ-INSIDE rule 10 state related enable
set firewall name DMZ-INSIDE rule 999 action drop
set firewall name DMZ-INSIDE rule 999 description "Drop all other traffic"
commit
exit
### Firewall Rules - INSIDE to OUTSIDE
set firewall name INSIDE-OUTSIDE rule 10 action accept
set firewall name INSIDE-OUTSIDE rule 10 description "Allow HTTP/HTTPS to Internet"
set firewall name INSIDE-OUTSIDE rule 10 destination port 80,443
set firewall name INSIDE-OUTSIDE rule 10 protocol tcp
set firewall name INSIDE-OUTSIDE rule 999 action drop
set firewall name INSIDE-OUTSIDE rule 999 description "Drop all other traffic"
commit
exit
### Apply Zone Policies
set zone-policy zone OUTSIDE from DMZ firewall name DMZ-OUTSIDE
set zone-policy zone DMZ from OUTSIDE firewall name OUTSIDE-DMZ
set zone-policy zone DMZ from INSIDE firewall name INSIDE-DMZ
set zone-policy zone INSIDE from DMZ firewall name DMZ-INSIDE
set zone-policy zone OUTSIDE from INSIDE firewall name INSIDE-OUTSIDE
commit
exit
### High Availability Configuration
set high-availability vrrp group FW-Cluster vrid 20
set high-availability vrrp group FW-Cluster interface eth0
set high-availability vrrp group FW-Cluster virtual-address 192.168.10.10/24
set high-availability vrrp group FW-Cluster priority 200
set high-availability vrrp sync-group FW-Cluster member FW-Cluster
commit
exit
### Connection Tracking Synchronization
set service conntrack-sync accept-protocol 'tcp,udp,icmp'
set service conntrack-sync failover-mechanism vrrp sync-group FW-Cluster
set service conntrack-sync interface eth5
set service conntrack-sync mcast-group 225.0.0.50
set service conntrack-sync disable-external-cache
commit
exit
### NAT Configuration
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 10.0.0.0/8
set nat source rule 100 translation address masquerade
commit
exit
### Static Routes
set protocols static route 0.0.0.0/0 next-hop 192.168.10.1
set protocols static route 10.10.0.0/24 next-hop 192.168.30.2
set protocols static route 10.20.0.0/24 next-hop 192.168.30.2
set protocols static route 10.100.0.0/16 next-hop 192.168.31.2
commit
exit
save

# FW2
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot
configure
set system host-name FW2

### Interfaces
set interfaces ethernet eth0 address 192.168.12.2/24
set interfaces ethernet eth0 description 'To-LB1'
set interfaces ethernet eth1 address 192.168.13.2/24
set interfaces ethernet eth1 description 'To-LB2'
set interfaces ethernet eth2 address 192.168.21.2/24
set interfaces ethernet eth2 description 'To-LB3'
set interfaces ethernet eth5 address 172.16.3.1/24
commit
### Security Zones
set zone-policy zone OUTSIDE description 'Internet Zone'
set zone-policy zone OUTSIDE interface eth0
set zone-policy zone OUTSIDE interface eth1
set zone-policy zone DMZ description 'DMZ Zone'
set zone-policy zone DMZ interface eth2
set zone-policy zone INSIDE description 'Internal Zone'
set zone-policy zone INSIDE interface eth3
set zone-policy zone INSIDE interface eth4
commit
### Firewall Rules - OUTSIDE to DMZ
set firewall name OUTSIDE-DMZ rule 10 action accept
set firewall name OUTSIDE-DMZ rule 10 description "Allow HTTPS to DMZ"
set firewall name OUTSIDE-DMZ rule 10 destination port 443
set firewall name OUTSIDE-DMZ rule 10 protocol tcp
set firewall name OUTSIDE-DMZ rule 20 action accept
set firewall name OUTSIDE-DMZ rule 20 description "Allow SMTP to DMZ"
set firewall name OUTSIDE-DMZ rule 20 destination port 25
set firewall name OUTSIDE-DMZ rule 20 protocol tcp
set firewall name OUTSIDE-DMZ rule 30 action accept
set firewall name OUTSIDE-DMZ rule 30 description "Allow IMAP to DMZ"
set firewall name OUTSIDE-DMZ rule 30 destination port 993
set firewall name OUTSIDE-DMZ rule 30 protocol tcp
set firewall name OUTSIDE-DMZ rule 40 action accept
set firewall name OUTSIDE-DMZ rule 40 description "Allow DNS to DMZ"
set firewall name OUTSIDE-DMZ rule 40 destination port 53
set firewall name OUTSIDE-DMZ rule 40 protocol udp
set firewall name OUTSIDE-DMZ rule 999 action drop
set firewall name OUTSIDE-DMZ rule 999 description "Drop all other traffic"
commit
### Firewall Rules - DMZ to OUTSIDE
set firewall name DMZ-OUTSIDE rule 10 action accept
set firewall name DMZ-OUTSIDE rule 10 description "Allow established connections"
set firewall name DMZ-OUTSIDE rule 10 state established enable
set firewall name DMZ-OUTSIDE rule 10 state related enable
set firewall name DMZ-OUTSIDE rule 999 action drop
set firewall name DMZ-OUTSIDE rule 999 description "Drop all other traffic"
commit
### Firewall Rules - INSIDE to DMZ
set firewall name INSIDE-DMZ rule 10 action accept
set firewall name INSIDE-DMZ rule 10 description "Allow all from INSIDE to DMZ"
commit
### Firewall Rules - DMZ to INSIDE
set firewall name DMZ-INSIDE rule 10 action accept
set firewall name DMZ-INSIDE rule 10 description "Allow established connections"
set firewall name DMZ-INSIDE rule 10 state established enable
set firewall name DMZ-INSIDE rule 10 state related enable
set firewall name DMZ-INSIDE rule 999 action drop
set firewall name DMZ-INSIDE rule 999 description "Drop all other traffic"
commit
### Firewall Rules - INSIDE to OUTSIDE
set firewall name INSIDE-OUTSIDE rule 10 action accept
set firewall name INSIDE-OUTSIDE rule 10 description 'Allow HTTP/HTTPS to Internet'
set firewall name INSIDE-OUTSIDE rule 10 destination port 80,443
set firewall name INSIDE-OUTSIDE rule 10 protocol tcp
set firewall name INSIDE-OUTSIDE rule 999 action drop
set firewall name INSIDE-OUTSIDE rule 999 description 'Drop all other traffic'
commit
### Apply Zone Policies
set zone-policy zone OUTSIDE from DMZ firewall name DMZ-OUTSIDE
set zone-policy zone DMZ from OUTSIDE firewall name OUTSIDE-DMZ
set zone-policy zone DMZ from INSIDE firewall name INSIDE-DMZ
set zone-policy zone INSIDE from DMZ firewall name DMZ-INSIDE
set zone-policy zone OUTSIDE from INSIDE firewall name INSIDE-OUTSIDE
commit
### High Availability Configuration
set high-availability vrrp group FW-Cluster vrid 20
set high-availability vrrp group FW-Cluster interface eth0
set high-availability vrrp group FW-Cluster virtual-address 192.168.12.10/24
set high-availability vrrp group FW-Cluster priority 100
set high-availability vrrp sync-group FW-Cluster member FW-Cluster
commit
### Connection Tracking Synchronization
set service conntrack-sync accept-protocol 'tcp,udp,icmp'
set service conntrack-sync failover-mechanism vrrp sync-group FW-Cluster
set service conntrack-sync interface eth5
set service conntrack-sync mcast-group 225.0.0.50
set service conntrack-sync disable-external-cache
commit
### NAT Configuration
set nat source rule 100 outbound-interface eth0
set nat source rule 100 source address 10.0.0.0/8
set nat source rule 100 translation address masquerade
commit
### Static Routes
set protocols static route 0.0.0.0/0 next-hop 192.168.12.1
set protocols static route 10.10.0.0/24 next-hop 192.168.30.2
set protocols static route 10.20.0.0/24 next-hop 192.168.30.2
set protocols static route 10.100.0.0/16 next-hop 192.168.31.2
commit
save
# FW3
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot
configure
set system host-name FW3

## Interfaces
set interfaces ethernet eth0 address 192.168.30.1/24
set interfaces ethernet eth0 description 'To-SWL3-C1'
set interfaces ethernet eth1 address 192.168.40.1/24
set interfaces ethernet eth1 description 'To-SWL3-1'

## Security Zones
set zone-policy zone CORE description "Core (LB2/SWL3-C1)"
set zone-policy zone CORE interface eth0

set zone-policy zone ACCESS description "Building A Zone"
set zone-policy zone ACCESS interface eth1

## Zone Policies
set zone-policy zone CORE from ACCESS firewall name ACCESS-CORE
set zone-policy zone ACCESS from CORE firewall name CORE-ACCESS

## Rules
set firewall name CORE-ACCESS rule 10 action accept
set firewall name CORE-ACCESS rule 10 description "Allow core access to building A"
set firewall name CORE-ACCESS rule 10 state established enable
set firewall name CORE-ACCESS rule 10 state related enable
set firewall name CORE-ACCESS rule 999 action drop
set firewall name CORE-ACCESS rule 999 description "Default deny from CORE to ACCESS"

set firewall name ACCESS-CORE rule 10 action accept
set firewall name ACCESS-CORE rule 10 description "Allow building A to core (HTTP)"
set firewall name ACCESS-CORE rule 10 protocol tcp
set firewall name ACCESS-CORE rule 10 destination port 80,443
set firewall name ACCESS-CORE rule 999 action drop
set firewall name ACCESS-CORE rule 999 description "Default deny from ACCESS to CORE"

commit; save; exit

# FW4
sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot
reboot
configure
set system host-name FW4

## Interfaces
set interfaces ethernet eth0 address 192.168.31.3/24
set interfaces ethernet eth0 description 'To-SWL3-C2'
set interfaces ethernet eth1 address 192.168.42.1/24
set interfaces ethernet eth1 description 'To-SWL3-2'

## Security Zones
set zone-policy zone CORE description "LB/Dist Zone"
set zone-policy zone CORE interface eth0
set zone-policy zone DATACENTER description "DC Zone"
set zone-policy zone DATACENTER interface eth1

## Zone Policies
set zone-policy zone CORE from DATACENTER firewall name DC-CORE
set zone-policy zone DATACENTER from CORE firewall name CORE-DC

## Rules
set firewall name CORE-DC rule 10 action accept
set firewall name CORE-DC rule 10 description "Allow core to datacenter"
set firewall name CORE-DC rule 10 state established enable
set firewall name CORE-DC rule 10 state related enable
set firewall name CORE-DC rule 999 action drop

set firewall name DC-CORE rule 10 action accept
set firewall name DC-CORE rule 10 description "Allow DC to core (SSH, HTTPS)"
set firewall name DC-CORE rule 10 protocol tcp
set firewall name DC-CORE rule 10 destination port 22,443
set firewall name DC-CORE rule 999 action drop

commit; save; exit
# SWL3 C1

configure terminal
hostname SWL3-C1

### Interfaces
interface FastEthernet 0/1
 description routed link to LB2
 ip address 192.168.30.2 255.255.255.0
 no shutdown
exit
interface FastEthernet0/0
 description To-FW3
 ip address 192.168.32.1 255.255.255.0
 no shutdown
exit
interface FastEthernet 1/0
 description Management
 switchport mode access
 switchport access vlan 1
 no shutdown
exit
### VLAN Interfaces
interface Vlan10
 description VLAN10-Network
 ip address 10.10.0.1 255.255.255.0
 standby 10 ip 10.10.0.254
 standby 10 priority 200
 standby 10 preempt
 no shutdown
exit
interface Vlan20
 description VLAN20-Network
 ip address 10.20.0.1 255.255.255.0
 standby 20 ip 10.20.0.254
 standby 20 priority 200
 standby 20 preempt
 no shutdown
exit
interface Vlan100
 description Datacenter-Network
 ip address 10.100.0.1 255.255.0.0
 standby 100 ip 10.100.0.254
 standby 100 priority 200
 standby 100 preempt
 no shutdown
exit
interface Vlan1
 description Management-Network
 ip address 20.0.0.1 255.255.255.0
 standby 1 ip 10.0.0.254
 standby 1 priority 200
 standby 1 preempt
 no shutdown
exit

### Routing
ip routing
ip route 0.0.0.0 0.0.0.0 192.168.30.1
ip route 200.0.0.0 255.255.255.0 192.168.30.1
exit
write
### Inter-VLAN ACLs
ip access-list extended VLAN10-TO-VLAN20
 permit udp 10.10.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 5060
 deny ip 10.10.0.0 0.0.0.255 10.20.0.0 0.0.0.255
 permit ip any any
exit
ip access-list extended VLAN20-TO-VLAN10
 permit udp 10.20.0.0 0.0.0.255 10.10.0.0 0.0.0.255 eq 5060
 deny ip 10.20.0.0 0.0.0.255 10.10.0.0 0.0.0.255
 permit ip any any
exit
### Apply ACLs
interface Vlan10
 ip access-group VLAN10-TO-VLAN20 out
exit
interface Vlan20
 ip access-group VLAN20-TO-VLAN10 out
exit
### Tunnel Configuration for VLAN Traffic
interface Tunnel10
 description Secure-Tunnel-VLAN10
 ip address 172.16.10.1 255.255.255.0
 tunnel source Vlan10
 tunnel destination 192.168.30.1
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile VLAN10-IPSEC
exit
interface Tunnel20
 description Secure-Tunnel-VLAN20
 ip address 172.16.20.1 255.255.255.0
 tunnel source Vlan20
 tunnel destination 192.168.30.1
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile VLAN20-IPSEC
exit
### IPsec Profiles
crypto ipsec transform-set TSET esp-aes esp-sha-hmac
crypto ipsec profile VLAN10-IPSEC
 set transform-set TSET
crypto ipsec profile VLAN20-IPSEC
 set transform-set TSET
exit

# SWL3 C2

### Core switch with VRRP for high availability
configure terminal
hostname SWL3-C2
### Interfaces
interface FastEthernet 0/1
 description To-LB2
 ip address 192.168.31.2 255.255.255.0
 no shutdown
exit
interface FastEthernet0/0
 description To-FW4
 ip address 192.168.33.1 255.255.255.0
 no shutdown
exit
interface FastEthernet 1/0
 description Management
 switchport mode access
 switchport access vlan 1
 no shutdown
exit
### Routing
ip routing
ip route 0.0.0.0 0.0.0.0 192.168.31.1
ip route 200.0.0.0 255.255.255.0 192.168.31.1
exit

end
write
# vlan10_vpc

set pcname VLAN10-Cli
ip 10.10.0.10 255.255.255.0 10.10.0.254
save

# vlan20_vpc
set pcname VLAN20-Cli
ip 10.20.0.10 255.255.255.0 10.20.0.254
save
# DMZ "server"
set pcname DMZ-vpc
ip 200.0.0.10 255.255.255.0 200.0.0.1
save
# Internet "server"
set pcname Internet-VPC
ip 100.0.0.10 255.255.255.0 100.0.0.254
save
# Datacenter "server"
set pcname DC-vpc
ip 10.100.0.10 255.255.255.0 10.100.0.1
save

# SWL3-1

configure terminal
hostname SWL3-1

interface FastEthernet 0/1.10
 encapsulation dot1Q 10
 ip address 10.10.0.1 255.255.255.0
 no shutdown
exit

interface FastEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.20.0.1 255.255.255.0
 no shutdown
exit

interface FastEthernet 0/1
 no shutdown
exit

interface FastEthernet 0/0
 description To-FW3
 ip address 192.168.41.2 255.255.255.0
 no shutdown
exit

interface FastEthernet 1/0
 description Management
 switchport mode access
 switchport access vlan 1
 no shutdown
exit

ip routing
ip route 0.0.0.0 0.0.0.0 192.168.41.1
exit
write
# SWL3-2

configure terminal
hostname SWL3-2

interface FastEthernet 0/0.100
 encapsulation dot1Q 100
 ip address 10.100.0.1 255.255.0.0
 no shutdown
exit

interface FastEthernet 0/0
 no shutdown
exit

interface FastEthernet 0/1
 description To-FW4
 ip address 192.168.42.2 255.255.255.0
 no shutdown
exit

interface FastEthernet 1/0
 description Management
 no shutdown
exit

ip routing
ip route 0.0.0.0 0.0.0.0 192.168.42.1
exit
write
# BUilding A (Router)

configure terminal

hostname Router-BuildingA

interface FastEthernet 0/0
 description Trunk to Building-A Switch
 ip address 192.168.50.2 255.255.255.0
 no shutdown
exit

interface FastEthernet 0/1
 description VLAN10 clients
 ip address 10.10.0.254 255.255.255.0
exit

interface FastEthernet 1/0
 description VLAN20 clients
 ip address 10.20.0.254 255.255.255.0
exit

interface FastEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 10.10.0.254 255.255.255.0
 no shutdown

interface FastEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 10.20.0.254 255.255.255.0
 no shutdown

ip routing
ip route 0.0.0.0 0.0.0.0 192.168.50.1
end
write