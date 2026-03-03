# Networking

## Connectivity Check

```bash
ping <ip-address>
```

## DNS

the file `hosts` file: `/etc/hosts`

to the host name:

```bash
hostname
```

> Name resolution: translating a name to an IP address

To manage all the IP <-> name mapping centrally we use `DNS Servers`, then all the hosts are pointed to use that DNS Server to look for an IP address.

* To point a host to a DNS Server add an entry to: `/etc/resolv.conf`

```bash
nameserver          <the dns server IP address>
```

* On conflicting hosts and remote DNS server, the local hosts file takes priority, by default

* The priority can be changed by updating: `/etc/nsswitch.conf`


### Domain name

* Top level domains (.com, .net, .edu, etc.)
* Domain name: (google)
* Subdomain: (www, drive, mail, etc.)

### DNS Chains

when looking up domain names, one DNS server could point to another:

```bash
Org DNS -> Root DNS -> .com DNS -> Google DNS
```

Org DNS may cache IPs to speed up lookups

### The `search` entry
Allows skipping specifying the subdomains:

should be added to `/etc/resolv.conf`

```bash
search          mycompany.com   prod.mycompany.com
```

### Record Types

* `A` record: mapping IPv4
* `AAAA` record: mapping IPv6
* `CNAME` record: mapping a name to another name

### Tools to test DNS resolution

* `nslookup` - does not consider the entries in the `/etc/hosts` file

```bash
nslookup www.google.com
```

* `dig` - also doesn't take into account local `hosts` file

```bash
dig www.google.com
```

## Network Basics

### Switching

Two hosts connected to the same swithc create a `network`.
Once the hosts have their IP configured they can communicate with each other.

* to display the link of the host

```bash
ip link
```

* to assign an ip address to the host

```bash
ip addr add 192.168.1.10/24 dev eth0
```

* to turn "UP" a network interface

```bash
sudo ip link set dev eth0 u
````

changes are only persisted until system restart. to perist them they must be added to `/etc/network/interfaces` file

### Routing

* Routers helps connect two networks together
* It's an intelligent device
* when it connects to two networks, it gets assigned two IPs

To see the routing config on a system

```bash
route
```

### Gateway

* acts like an door

To add a routing from one network to another:

```bash
ip route add 192.168.2.0/24 via 192.168.1.1
```

this basically says: you can reach `192.168.2.0/24` via the gateway `192.168.1.1`

#### Default gateway

To configure the default gateway:

```bash
ip route add default via 192.168.2.1
```

or

```bash
ip route add default via 0.0.0.0
```

## Troubleshooting

### Check the host interfaces and connectivity

* Ensure the primary interface is up

```bash
ip link
```

### Check DNS resolution

* Ensure the name is resolved to an IP address

```bash
nslookup caleston-repo-01
```

### Check the connectivity to the remote host

```bash
ping caleston-repo-01
```

### Check the route

* show the number of hops between the source and the remote server

```bash
traceroute 192.168.2.5
```

### Troubleshoot the remote server

* check if the proccess is running on port 80

```bash
netstat -an | grep 80 | grep -i LISTEN
```

* check the interfaces on the server

```bash
ip link
```

* if the interface is down, bring it up by

```bash
ip link set dev enp1s0f1 up
```