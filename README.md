# CCNA-Project
**Requirements for Network:
1. Use Cisco Packet Tracer to design and implement the network solution.
2. Use hierarchical model providing redundancy at every layer i.e. two routers and two multilayer switches are expected to be used to provide redundancy.
3. The network is also expected to connect to at least two ISPs to provide redundancy and each router to the connected to the two ISPs.
4. Each department is required to have a wireless network for the users.
5. Each department is required to have a wireless network for the users.
6. Provide department should be in a different VLAN and in different subnetwork.
7. The company network is connected to the static, public IP addresses (Internet Protocol) 195.136.17.0/30 , 195.136.17.4/30 , 195.136.17.8/30 & 195.136.17.12/30 connected to the two Internet Providers.
8. Configure basic device settings such as hostnames, console password, enable password, banner messages, disable IP domain lookup.
9. Devices in all the departments are required to communicate with each other with the respective multilayer witch configured for inter-Vlan routing.
10.The Multilayer switches are expected to carry out both routing and switching functionalities thus will be assigned IP addresses.
11. All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located at the server room.
12. Devices in the server room are to be allocated IP address statically.

**Configure Steps:
1. Basic settings to all devices plus ssh on the routers and 13 switches.
2. VLANs assignment plus all access and trunk ports on 12 & 13 switches.
3. switchport security to finance department.
4. Subnetting IP addressing.
5. OSPF on the routers and 13 switches.
6. Static IP address to server-room devices.
7.DHCP server device configuration.
8. Inter-VLAN routing on the 13 switches plus ip dhcp helper addresses.
9. Wireless network configurations.
10. PAT(Port Address Translation) + Access Control List (ACL)
11. Verifying and testing configurations.


1.SALES SWITHCH:

en
config t
hostname Sales-SW
banner motd #NO Unauthorized Access!!!#
no ip domain lookup
line console 0
passw cisco 
login
ex
exit
enable password cisco
service password-encryption
ex

2.HR SWITCH:

en
config t

hostname HR-SW
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

3.FINANCE SWITCH:

en
config t

hostname FINANCE-SW
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

4. ADMIN SWITCH:

en
config t

hostname ADMIN-SW
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

5.ICT SWITCH:

en
config t

hostname ICT-SW
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

6. SERVER-ROOM SWITCH:

en
config t

hostname SERVERROOM-SW
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

7.MULTILAYER SWITCH 1:

en
config t

hostname MLT-SW1
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

ip domain name cisco.net
username admin password cisco
crypto key generate rsa 
1024
line vty o 15
login local
transport input ssh

ex

ip ssh version 2

int range gig1/0/3-8
switchport mode trunk

vlan 10
name Sales
vlan 20
name HR
vlan 30
name Finance
vlan 40
name Admin
vlan 50
name ICT
vlan 60
name Serverroom

exit

ip routing
router ospf 10
router-id 2.2.2.2
network 172.16.1.0 0.0.0.127 area 0	
network 172.16.1.128 0.0.0.127 area 0
network 172.16.2.0 0.0.0.127 area 0
network 172.16.2.128 0.0.0.127 area 0
network 172.16.3.0 0.0.0.127 area 0
network 172.16.3.128 0.0.0.15 area 0

network 172.16.3.144 0.0.0.3 area 0
network 172.16.3.148 0.0.0.3 area 0
exit

do wr


8.MULTILAYER SWITCH 2:

en
config t

hostname MLT-SW2
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

ip domain name cisco.net
username admin password cisco
crypto key generate rsa 
1024
line vty o 15
login local
transport input ssh

ex

ip ssh version 2

int range gig1/0/3-8
switchport mode trunk

vlan 10
name Sales
vlan 20
name HR
vlan 30
name Finance
vlan 40
name Admin
vlan 50
name ICT
vlan 60
name Serverroom

exit

int range gig1/0/1-1
no switchport

int gig1/0/1
ip address 172.168.3.153 255.255.255.252
no sh
exit

int gig1/0/2
ip address 172.168.3.157 255.225.255.252
no sh
exit

ip routing
router ospf 10
router-id 1.1.1.1
network 172.16.1.0 0.0.0.127 area 0	
network 172.16.1.128 0.0.0.127 area 0
network 172.16.2.0 0.0.0.127 area 0
network 172.16.2.128 0.0.0.127 area 0
network 172.16.3.0 0.0.0.127 area 0
network 172.16.3.128 0.0.0.15
network 172.16.3.152 0.0.0.3 area 0
network 172.16.3.156 0.0.0.3 area 0
exit

do sh ip ospf neighbor
do clear ip ospf process

do wr


9. CORE ROUTER 1:

en
config t

hostname CORE-R1
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

ip domain name cisco.net
username admin password cisco
crypto key generate rsa 
1024
line vty o 15
login local
transport input ssh

ex

ip ssh version 2

interface GigabitEthernet0/0
ip add 172.16.3.146 255.255.255.252
no sh
ex

interface GigabitEthernet0/1 
ip add 172.16.3.154 255.255.255.252
no sh
ex

int se0/2/0
clock rate 64000
ip add 195.136.17.5 255.255.255.252
no sh
ex

int se0/2/1  
clock rate 64000  
ip add 195.136.17.5 255.255.255.252
no sh
ex

router ospf 10
router-id 3.3.3.3

network 172.16.3.144 0.0.0.3 area 0
network 172.16.3.152 0.0.0.3 area 0
network 195.136.17.0 0.0.0.3 area 0
network 195.136.17.4 0.0.0.3 area 0

do wr

10.CORE ROUTER 2:

en
config t

hostname CORE-R2
line console 0
password cisco
login
exit

enable password cisco
no ip domain-lookup
banner motd #No Unauthorised Access!!!#
service password-encryption

do wr

--"Configure Manually, Sometimes after it shows errors"--

ip domain name cisco.net
username admin password cisco
crypto key generate rsa 
1024
line vty o 15
login local
transport input ssh

ex

ip ssh version 2

interface GigabitEthernet0/0
ip add 172.16.3.150 255.255.255.252
no sh
ex

interface GigabitEthernet0/1
ip add 172.16.3.158 255.255.255.252
no sh
ex

int se0/2/0
ip add 195.136.17.9 255.255.255.252
no sh
clock rate 64000
ex

int se0/2/1
ip add 195.136.17.13 255.255.255.252
no sh
clock rate 64000
ex

interface GigabitEthernet0/0
router ospf 10
router-id 4.4.4.4

network 172.16.3.148 0.0.0.3 area 0
network 172.16.3.156 0.0.0.3 area 0
network 195.136.17.8 0.0.0.3 area 0
network 195.136.17.12 0.0.0.3 area 0

do wr

11. ISP router 1:

int se0/3/0
ip add 195.136.17.2 255.255.255.252
ex

int se0/3/1
ip add 195.136.17.10 255.255.255.252
ex

router ospf 10
router-id 5.5.5.5
network 195.136.17.8 0.0.0.3 area 0
network 195.136.17.0 0.0.0.3 area 0

do wr

12.ISP router 2:

int se0/3/0
ip add 195.136.17.6 255.255.255.252
ex

int se0/3/1
ip add 195.136.17.14 255.255.255.252
ex

router ospf 10
router-id 6.6.6.6
network 195.136.17.4 0.0.0.3 area 0
network 195.136.17.12 0.0.0.3 area 0

do wr


13.SALES SWITHCH:

cisco

en
cisco
conf t
int range fa0/1-2
switchport mode trunk
exit

vlan 10
name Sales
exit

int range fa0/3-24
switchport mode access
switchport access vlan 10
exit 

do wr

vlan 99 
name BlackHole
exit
 
int range gig0/1-2
switchport mode access
switchport access vlan 99
shutdown
ex

do wr

14.HR SWITCH:

int range fa0/1-2
switchport mode trunk
exit

vlan 20
name HR 
vlan 99
name Blackhole
exit

int range fa0/3-24
switchport mode access
switchport access vlan 20
exit

int range gig0/1-2
switchport mode access
switchport access vlan 99
exit

do wr

15. FINANCE SWITCH:

int range fa0/1-2
switchport mode trunk
exit

vlan 30
name FINANCE
vlan 99
name Blackhole
exit

int range fa0/3-24
switchport mode access
switchport access vlan 30
exit

int range gig0/1-2
switchport mode access
switchport access vlan 99
exit

int range fa0/3-24
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

 

do wr



16.ADMIN SWITCH:


int range fa0/1-2
switchport mode trunk
exit

vlan 40
name ADMIN
vlan 99
name Blackhole
exit

int range fa0/3-24
switchport mode access
switchport access vlan 40
exit

int range gig0/1-2
switchport mode access
switchport access vlan 99
exit

do wr

17.ICT SWITCH:

int range fa0/1-2
switchport mode trunk
exit

vlan 50
name ICT
vlan 99
name Blackhole
exit

int range fa0/3-24
switchport mode access
switchport access vlan 50
exit

int range gig0/1-2
switchport mode access
switchport access vlan 99
exit

do wr

18.SERVERROOM SWITCH:

int range fa0/1-2
switchport mode trunk
exit

vlan 60
name SERVERROOM
vlan 99
name Blackhole
exit

int range fa0/3-24
switchport mode access
switchport access vlan 60
exit

int range gig0/1-2
switchport mode access
switchport access vlan 99
exit

do wr

19.IP Addressing:

Base Network:172.16.1.0
		
				First Floor
-------------------------------------------------------------------------------
Department   Network       Subnet Mask           Host Address     Broadcast
	     Address       Range	                          Address
-------------------------------------------------------------------------------
Sales &      172.16.1.0    255.255.255.128/25    172.16.1.1 to	  172.16.1.127
Marketing					 172.16.1.126

HR & 	     172.16.1.128  255.255.255.128/25	 172.16.1.129 to  172.16..1.255
Logistics					 172.16.1.254
-------------------------------------------------------------------------------

				
			        Second Floor
--------------------------------------------------------------------------------
Department   Network	   Subnet Mask		Host Address	  Broadcast
	     Address				Range		  Address
--------------------------------------------------------------------------------
Finance &    172.16.2.0	   255.255.255.128.25   172.16.2.1 to     172.16.2.127
Accounts					172.16.2.126

Admin &	     172.16.2.128  255.255.255.128/25   172.16.2.129 to	  172.16.2.255
Public						172.16.2.254
Relations
--------------------------------------------------------------------------------

			
				Third Floor
--------------------------------------------------------------------------------
Department   Network	   Subnet Mask		Host Address	  Broadcast
	     Address				Range		  Address
--------------------------------------------------------------------------------
ICT	     172.16.3.0	   255.255.255.128/25   172.16.3.1 to	  172.16.3.127
						172.16.3.126

Server 	     172.16.3.128  255.255.255.240/28   172.16.3.129 to   172.16.3.143
Room						172.16.1.142
--------------------------------------------------------------------------------

Between the Routers & Layer-3 Switches

--------------------------------------------------------------------------------
No.	    Network	   Subnet Mask		Host Address	  Broadcast 
	    Address				Range		  Address
--------------------------------------------------------------------------------
R1-MLSW1    172.16.3.144   255.255.255.252      172.16.3.145 to	  172.16.3.147
						172.16.3.146

R1-MLSW2    172.16.3.148   255.255.255.252      172.16.3.149 to	  172.16.3.151
						172.16.3.149

R2-MLSW1    172.16.3.152   255.255.255.252	172.16.3.153 to	  172.16.3.155
						172.16.3.154

R2-MLSW2    172.16.3.156   255.255.255.252      172.16.3.157 to	  172.16.3.159
						172.16.3.158
---------------------------------------------------------------------------------

= Between the Routers & ISPs

Publix IP Addresses 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30 & 195.136.17.12/30


20.DHCP Server:

IPV4 Address    - 172.16.3.130
Subnet Mask     - 255.255.255.240
Default Gateway - 172.16.3.129
DNS Server      - 172.16.3.131

21.SysAdmin-PC:

IPV4 Address    - 172.16.3.132
Subnet Mask     - 255.255.255.240
Default Gateway - 172.16.3.129
DNS Server      - 172.16.3.131

22.DNS Server:

IPV4 Address    - 172.16.3.131
Subnet Mask     - 255.255.255.240
Default Gateway - 172.16.3.129

23.Configuring DHCP-Server:

Services = DHCP = On 

Pool Name               - SalesPool
Default Gateway         - 172.168.1.1
DNS Server 		- 172.16.3.131
Start IP Address 	- 172.16.1.6
Subnet Mask 		- 255.255.255.128
Maximum Number of Users - 120

Add..

Pool Name               - HRPool
Default Gateway         - 172.168.1.129
DNS Server 		- 172.16.3.131
Start IP Address 	- 172.16.1.134
Subnet Mask 		- 255.255.255.128
Maximum Number of Users - 120

Add..


Pool Name               - FinancePool
Default Gateway         - 172.168.2.1
DNS Server 		- 172.16.1.131
Start IP Address 	- 172.16.2.6
Subnet Mask 		- 255.255.255.128
Maximum Number of Users - 120

Add..

Pool Name               - AdminPool
Default Gateway         - 172.168.2.129
DNS Server 		- 172.16.3.131
Start IP Address 	- 172.16.2.134
Subnet Mask 		- 255.255.255.128
Maximum Number of Users - 120

Add..

Pool Name               - ICTPool
Default Gateway         - 172.168.3.1
DNS Server 		- 172.16.3.131
Start IP Address 	- 172.16.3.6
Subnet Mask 		- 255.255.255.128
Maximum Number of Users - 120

Add..


24.Configuring DNS-Server:

Services = DNS = ON

Name	- www.networld.com
Address - 172.16.3.131


25. Configuring Inter-VLAN Routing on two Multilayer Switches:

int vlan 10
no sh
ip add 172.16.1.1 255.255.255.128
ip helper-address 172.16.3.130
ex

int vlan 20
no sh
ip add 172.16.1.129 255.255.255.128
ip helper-address 172.16.3.130
ex

int vlan 30
no sh
ip add 172.16.2.1 255.255.255.128
ip helper-address 172.16.3.130
ex

int vlan 40
no sh
ip add 172.16.2.129 255.255.255.128
ip helper-address 172.16.3.130
ex

int vlan 50
no sh
ip add 172.16.3.1 255.255.255.128
ip helper-address 172.16.3.130
ex

int vlan 60
no sh
ip add 172.16.3.129 255.255.255.240
ip helper-address 172.16.3.130
ex

ip route 0.0.0.0 0.0.0.0 gig1/0/1
ip route 0.0.0.0 0.0.0.0 gig1/0/2 70

do wr

26.Wireless Configuration:

Access Point = Config = Port 1 

SSID	- Sales-AP
Copy Display name = port 1 = SSID = [Paste name] Authentication - WPA2-PSK PSK Pass Phrase Sales-AP%135

shutdown Laptop, Unplug by default module of laptop & plug new WPC300N = PC Wireless = Connect = Select name of AP = Set password & Connect

Tablet = Config = Wireless 0 = SSID of AP, Authentication - WPA2-PSK & set password

#Do the above steps steps of AP, Laptops & tabs for all others.

27.Configure NAT on Core-Routers 1 & 2:

=Core-Router 1:

ip nat inside source list 1 int se0/2/0 overload
ip nat inside source list 1 int se0/2/1 overload
access-list 1 permit 172.16.1.0 0.0.0.127 
access-list 1 permit 172.16.1.128 0.0.0.127 
access-list 1 permit 172.16.2.0 0.0.0.127 
access-list 1 permit 172.16.2.128 0.0.0.127 
access-list 1 permit 172.16.3.0 0.0.0.127 
access-list 1 permit 172.16.3.128 0.0.0.15 

int range gig0/0-1
ip nat inside
ex

int se0/2/0
ip nat outside
int se0/2/1
ip nat outside 
ex

ip route 0.0.0.0 0.0.0.0 se0/2/0
ip route 0.0.0.0 0.0.0.0 se0/2/1 70

do wr

=Core-Router 2:

ip nat inside source list 1 int se0/2/0 overload
ip nat inside source list 1 int se0/2/1 overload
access-list 1 permit 172.16.1.0 0.0.0.127 
access-list 1 permit 172.16.1.128 0.0.0.127 
access-list 1 permit 172.16.2.0 0.0.0.127 
access-list 1 permit 172.16.2.128 0.0.0.127 
access-list 1 permit 172.16.3.0 0.0.0.127 
access-list 1 permit 172.16.3.128 0.0.0.15 

int range gig0/0-1
ip nat inside
ex

int se0/2/0
ip nat outside
int se0/2/1
ip nat outside
ex

ip route 0.0.0.0 0.0.0.0 se0/2/0
ip route 0.0.0.0 0.0.0.0 se0/2/1 70

do wr





