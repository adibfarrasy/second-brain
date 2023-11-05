#software #network #devops 

resource:
[https://www.youtube.com/watch?v=bKFMS5C4CG0](https://www.youtube.com/watch?v=bKFMS5C4CG0)


# Default Bridge

- Docker creates a new network interface.

```bash
$ ip address show
....
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:d6:f1:4a:65 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:d6ff:fef1:4a65/64 scope link
       valid_lft forever preferred_lft forever
....
```

- 3 default network types (`DRIVER`s).

```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
0190482f5ece   bridge    bridge    local
8f714be1afdb   host      host      local
edf768651bf4   none      null      local
```

- All the containers are connected to the same `docker0` network interface and each create a virtual ethernet interface.

```bash
$ docker run -itd --rm --name aloha hello-world
$ docker run -itd --rm --name hola hello-world
$ ip address show
....
31: veth5fb2d58@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 7e:57:89:82:01:1b brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::7c57:89ff:fe82:11b/64 scope link
       valid_lft forever preferred_lft forever
33: veth17f80b1@if32: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 22:ed:f0:1e:fb:f4 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::20ed:f0ff:fe1e:fbf4/64 scope link
       valid_lft forever preferred_lft forever
$ bridge link
31: veth5fb2d58@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master docker0 state forwarding priority 32 cost 2
33: veth17f80b1@if32: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master docker0 state forwarding priority 32 cost 2
```

- Each docker container has its own IP address in the same `docker0` network. Docker has its own DHCP, DNS and /etc/resolv.conf from the host machine.

```bash
$ docker inspect bridge
....
"Containers": {
            "9d944c2b2ce7626b2f76379d367599d0f99d470fb4b17aeafe4d88e058268777": {
                "Name": "hola",
                "EndpointID": "6a80da5bfbf2b1412e72969f4fd010ece80475c2dba3c7192944879dc2e3507f",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "eee573f10373db5aa61f910991bbb47eee398599bf714f76e8d4028c4fbd86cb": {
                "Name": "aloha",
                "EndpointID": "6ea623636b36edf09564d6ea18d77323d8c81c0e4966eeb5904812f78eb349c3",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
....
```

- Containers can talk to each other and to the internet. Their default gateway is `docker0`, and they connect to the internet via NAT masquerading.

```bash
$ docker exec -it aloha sh
/ # ping google.com
PING google.com (142.250.4.102): 56 data bytes
64 bytes from 142.250.4.102: seq=0 ttl=101 time=25.454 ms
64 bytes from 142.250.4.102: seq=1 ttl=101 time=22.223 ms
64 bytes from 142.250.4.102: seq=2 ttl=101 time=21.698 ms
/ # ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.055 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.041 ms
64 bytes from 172.17.0.2: seq=2 ttl=64 time=0.039 ms
/ # ip route
default via 172.17.0.1 dev eth0
172.17.0.0/16 dev eth0 scope link  src 172.17.0.3
```

- Bridge network cannot be accessed from the external world (e.g. from the host machine) unless it’s specifically exposed.

```bash
$ docker run -itd --rm -p 80:80 --name ciao nginx
```

on the browser, go to `[localhost](<http://localhost>)` and find the nginx server running.

# User-defined Bridge

Docker creators encourage their users to use this bridge instead of the Default Bridge.

- Create a user-defined bridge

```bash
$ docker network create greet
$ ip address show
....
36: br-ec36da1899e0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:d6:4d:be:49 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-ec36da1899e0
       valid_lft forever preferred_lft forever
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
0190482f5ece   bridge    bridge    local
ec36da1899e0   greet     bridge    local << new bridge
edf768651bf4   none      null      local
```

- Create containers and inspect them

```bash
$ docker run -itd --rm --network greet --name nihao busybox
$ docker run -itd --rm --network greet --name konnichiwa busybox
$ ip address show
.... # New virtual ethernet interfaces
44: veth791e697@if43: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-ec36da1899e0 state UP group default
    link/ether ca:4f:cf:df:dc:d9 brd ff:ff:ff:ff:ff:ff link-netnsid 3
    inet6 fe80::c84f:cfff:fedf:dcd9/64 scope link
       valid_lft forever preferred_lft forever
46: vethd39079e@if45: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-ec36da1899e0 state UP group default
    link/ether 2a:00:36:44:ca:e6 brd ff:ff:ff:ff:ff:ff link-netnsid 4
    inet6 fe80::2800:36ff:fe44:cae6/64 scope link
       valid_lft forever preferred_lft forever
$ bridge link
# new virtual interfaces tied to the user-defined bridge
44: veth791e697@if43: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master br-ec36da1899e0 state forwarding priority 32 cost 2
46: vethd39079e@if45: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master br-ec36da1899e0 state forwarding priority 32 cost 2
$ docker inspect greet
.... # the containers are assigned IP addresses
"Containers": {
            "a9badf6abc5058b75d05224bf30867b93a4bea0c655c3c79fe182331ab5b828c": {
                "Name": "nihao",
                "EndpointID": "9ee9a8af9126c6517a1ff2838253726eaf7116263feb97ba61b0c8ee650e752c",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "b032fa09c6fb73fae8e40907059fb41d3c770a32d5c1b79f6f6f81dd80e3161e": {
                "Name": "konnichiwa",
                "EndpointID": "507a0381c61a3f3e579fd70fbe7e6c9032635d0a4302b5b790648e1c12033119",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
```

- The container is isolated from the outside network

```bash
$ docker exec -it nihao sh
/ # ping 172.17.0.2 
# pinging hola container doesn't work
```

- User-defined bridge automatically resolve DNS based on container names

```bash
$ docker exec -it nihao sh
/ # ping konnichiwa
PING konnichiwa (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.078 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.075 ms
64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.132 ms
64 bytes from 172.18.0.3: seq=3 ttl=64 time=0.122 ms
```

# Host

Docker will create containers not linked to Default or User-defined bridge.

- Create a container in the host network.

```bash
$ docker stop ciao
$ docker run -itd --rm --network host --name ciao nginx
```

On the browser, go to `[localhost](<http://localhost>)` and find the nginx server running.

A major downside of host network is no isolation, it’s connected to whatever the host is connected to.

# MACVLAN

- Create a MACVLAN network

```bash
$ ip r | grep default
default via 192.168.1.1 dev wlp0s20f3 proto dhcp src 192.168.1.6 metric 600
# gateway is 192.168.1.1
$ ip address show
....
3: wlp0s20f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether f0:9e:4a:84:8e:06 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.6/24 brd 192.168.1.255 scope global dynamic noprefixroute wlp0s20f3
       valid_lft 179220sec preferred_lft 179220sec
    inet6 fe80::6c9e:277f:b8b5:93d1/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
....
# wlp0s20f3 is the host network default interface
# 192.168.1.6/24 is the subnet
$ docker network create -d macvlan \\
--subnet 192.168.1.6/24 \\
--gateway 192.168.1.1 \\
-o parent=wlp0s20f3 \\
newgreet

$ docker network ls
NETWORK ID     NAME       DRIVER    SCOPE
0190482f5ece   bridge     bridge    local
ec36da1899e0   greet      bridge    local
8f714be1afdb   host       host      local
37c4776e8509   newgreet   macvlan   local << new network
edf768651bf4   none       null      local
```

- Create a container inside the macvlan network

```bash
$ docker stop aloha hola
$ docker run -itd --rm --network newgreet \\
--ip 192.168.1.50 --name hola busybox
$ docker exec -it hola sh
/ # ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
47: eth0@if3: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue
    link/ether 02:42:c0:a8:01:32 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.50/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # ping google.com
# doesn't work!
```

- Since the host and the new macvlan network is connected to the same router switchport, it has to be configured differently for the macvlan network to connect to the internet. The default configuration doesn’t allow it due to security reasons.

```bash
# set promiscuous mode
$ sudo ip link set wlp0s20f3 promisc on
```

# MACVLAN 802.1q

- Create a sub-interface within the parent network interface, and link a container to the new network sub-interface.

```bash
$ docker network create -d macvlan \\
--subnet 192.168.1.6/24 \\
--gateway 192.168.1.1 \\
-o parent=wlp0s20f3.20 \\
newgreet20
$ ip address show
....
7: wlp0s20f3.20@wlp0s20f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether f0:9e:4a:84:8e:06 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::f29e:4aff:fe84:8e06/64 scope link 
       valid_lft forever preferred_lft forever
$ docker run -itd --rm --network newgreet20 \\
--ip 192.168.1.60 --name aloha busybox
```

<aside> 💡 **Why do we need sub-interface?** If we have one Router with one physical interface, but needed to have the router connected to two IP networks to route traffic between two routers, we can create two sub interfaces within the physical interface, assign each sub interface an IP address within each subnet and then route the data between two subnets.

</aside>

# IPVLAN L2

It’s like the MACVLAN, but without the promiscuous setup.

- Create an IPVAN network (default L2), and link a container to the network.

```bash
$ docker network create -d ipvlan \\
--subnet 192.168.1.6/24 \\
--gateway 192.168.1.1 \\
-o parent=wlp0s20f3 \\
newgreet
$ docker run -itd --rm --network newgreet \\
--ip 192.168.1.60 --name aloha busybox
$ docker exec -it aloha
/ # ping 192.168.1.1
PING 192.168.1.1 (192.168.1.1): 56 data bytes
64 bytes from 192.168.1.1: seq=0 ttl=64 time=41.878 ms
64 bytes from 192.168.1.1: seq=1 ttl=64 time=30.979 ms
64 bytes from 192.168.1.1: seq=2 ttl=64 time=9.245 ms
# ping gateway success
/ # ping google.com
PING google.com (142.250.4.101): 56 data bytes
64 bytes from 142.250.4.101: seq=0 ttl=58 time=24.195 ms
64 bytes from 142.250.4.101: seq=1 ttl=58 time=20.133 ms
64 bytes from 142.250.4.101: seq=2 ttl=58 time=20.419 ms
# connected to the internet
```

- Host to IPVLAN network is restricted for security.

```bash
$ ping 192.168.1.60
PING 192.168.1.60 (192.168.1.60) 56(84) bytes of data.
From 192.168.1.6 icmp_seq=1 Destination Host Unreachable
From 192.168.1.6 icmp_seq=2 Destination Host Unreachable
From 192.168.1.6 icmp_seq=3 Destination Host Unreachable
```

# IPVLAN L3

It’s like IPVLAN L2, but it works in the L3 layer of the OSI model. It essentially behaves like a router to the host machine, connecting directly to the network interface. There will be no broadcast traffic, and it will not respond to outside requests. The containers inside this network can only talk to one another.

- Create an IPVLAN L3 network with multiple subnets

```bash
$ docker network create -d ipvlan \\
--subnet 192.168.69.0/24 \\
-o parent=wlp0s20f3 -o ipvlan_mode=l3 \\
--subnet 192.168.96.0/24 \\
newgreet
$ docker run -itd --rm --network newgreet \\
--ip 192.168.69.6 --name nihao busybox
$ docker run -itd --rm --network newgreet \\
--ip 192.168.69.7 --name konnichiwa busybox
$ docker run -itd --rm --network newgreet \\
--ip 192.168.96.6 --name hej busybox
$ docker run -itd --rm --network newgreet \\
--ip 192.168.96.7 --name bonjour busybox
$ docker inspect newgreet
"Containers": {
            "213ffc09c3625778c6cf70c635e2f3c7b9b1ebe3f9bb856979155190a4d3d682": {
                "Name": "konnichiwa",
                "EndpointID": "a284c571af627b527ef29b039ef11eeddef364e9bb9299d5353f2814da41a643",
                "MacAddress": "",
                "IPv4Address": "192.168.69.7/24",
                "IPv6Address": ""
            },
            "8851ec0b359ea3cc071b461096202760e5c1ef2962f076768042859af5b8a45e": {
                "Name": "bonjour",
                "EndpointID": "b56dfaf2976053a7ec833f33d1fc1bd079d8a4c16b70ad7bad0372139bdc1ade",
                "MacAddress": "",
                "IPv4Address": "192.168.96.7/24",
                "IPv6Address": ""
            },
            "af8fdeb5094e3ead342dc82efae86512b9243dc9a6d2654523b5f1c7517b9b62": {
                "Name": "hej",
                "EndpointID": "3b969e578f5786d0692fcb2a24dc2c4dfefcb6826576e8f1d2376e5575e4136c",
                "MacAddress": "",
                "IPv4Address": "192.168.96.6/24",
                "IPv6Address": ""
            },
            "bbdcdd7efb0c9cff06e6d7f4304998816cf42f4d5c694c144ae55bdbad1ae798": {
                "Name": "nihao",
                "EndpointID": "d06799374da00203f55e278d8167d1d91548378ee16f676ed7ad7f9d0750f43a",
                "MacAddress": "",
                "IPv4Address": "192.168.69.6/24",
                "IPv6Address": ""
            }
```

- Verify that the container cannot reach the internet.

```bash
$ docker exec -it bonjour sh
/ # ping google.com
# doesn't work!
/ # ip route
default dev eth0 scope link 
192.168.96.0/24 dev eth0 scope link  src 192.168.96.7
# it actually is connected to the interface
/ # ping nihao
PING nihao (192.168.69.6): 56 data bytes
64 bytes from 192.168.69.6: seq=0 ttl=64 time=0.100 ms
64 bytes from 192.168.69.6: seq=1 ttl=64 time=0.096 ms
64 bytes from 192.168.69.6: seq=2 ttl=64 time=0.104 ms
/ # ping hej
PING hej (192.168.96.6): 56 data bytes
64 bytes from 192.168.96.6: seq=0 ttl=64 time=0.084 ms
64 bytes from 192.168.96.6: seq=1 ttl=64 time=0.094 ms
64 bytes from 192.168.96.6: seq=2 ttl=64 time=0.106 ms
```

# Overlay

Used for Docker Swarm, similar to Kubernetes. It abstracts the host networks talking to one another and set the rules on how they can communicate.

# None

- Create a container and link it to the none network. Probably just use it for testing.

```bash
$ docker run -itd --rm --network none --name halo busybox
$ docker exec -it halo
/ # ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
# no other network interface exists
```