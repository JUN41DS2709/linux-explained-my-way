# Essential Networking Commands for Linux

Linux provides a large number of networking utilities for troubleshooting, reconnaissance, monitoring, and system administration.

---

## Network Interface Commands

### `ip addr show`

Displays information about network interfaces and their IP addresses.

```bash id="ip1"
ip addr show
```

---

### `ip a`

A shorter version of `ip addr show`.

```bash id="ip2"
ip a
```

---

### `ip route`

Displays the system's routing table.

```bash id="ip3"
ip route
```

---

## Bringing Network Interfaces Up or Down

### Bring an Interface Up

```bash id="ip4"
sudo ip link set eth0 up
```

---

### Bring an Interface Down

```bash id="ip5"
sudo ip link set eth0 down
```

These commands are used to enable or disable a network interface.

---

## Connectivity Testing

### `ping`

Used to test connectivity and measure latency between systems.

Example:

```bash id="ping1"
ping google.com
```

Useful for:

* Testing connectivity
* Measuring latency
* Verifying whether a host is reachable

---

## Route Tracing

### `traceroute`

Shows the path packets take to reach a destination.

Example:

```bash id="trace1"
traceroute google.com
```

If it is not installed:

```bash id="trace2"
sudo apt install traceroute
```

Useful for:

* Network mapping
* Identifying filtering devices
* Troubleshooting routing issues

---

## Socket and Connection Monitoring

### `ss`

A command-line utility used to display socket statistics and monitor network connections on Linux systems.

Example:

```bash id="ss1"
ss -tuln
```

Common options:

* `-t` → TCP sockets
* `-u` → UDP sockets
* `-l` → Listening sockets
* `-n` → Display numerical addresses instead of resolving names

---

### `netstat`

An older networking utility used to display:

* Network connections
* Routing tables
* Interface statistics
* Listening ports

Example:

```bash id="net1"
netstat -tuln
```

On many modern Linux distributions, `ss` is preferred over `netstat`.

---

## Open Files and Network Connections

### `lsof`

Lists open files and network connections.

Example:

```bash id="lsof1"
lsof
```

---

### `lsof -i`

Displays network-related files and connections.

```bash id="lsof2"
lsof -i
```

---

## DNS Utilities

### `nslookup`

A DNS query utility commonly used to retrieve IP addresses and perform DNS lookups.

Example:

```bash id="dns1"
nslookup example.com
```

Useful for:

* DNS reconnaissance
* DNS troubleshooting
* Name resolution verification

---

### `dig`

A more advanced and preferred DNS investigation tool.

Example:

```bash id="dns2"
dig example.com
```

---

### `host`

A quick DNS lookup utility.

Example:

```bash id="dns3"
host example.com
```

---

## Netcat (`nc`)

`nc` (Netcat) is often called the **Swiss Army Knife of Networking** because of its flexibility.

---

### Listen on a Port

```bash id="nc1"
nc -lvnp 4444
```

---

### Connect to a Port

```bash id="nc2"
nc 192.168.1.10 80
```

---

### Banner Grabbing

```bash id="nc3"
nc target.com 22
```

This is commonly used to identify services and software versions running on a port.

---

### File Transfer

Receiver:

```bash id="nc4"
nc -lvnp 4444 > file.txt
```

Sender:

```bash id="nc5"
nc 192.168.1.10 4444 < file.txt
```

---

## Packet Capture

### `tcpdump`

A command-line packet capture utility.

Example:

```bash id="tcp1"
sudo tcpdump -i eth0
```

Useful for:

* Traffic analysis
* Troubleshooting network issues
* Protocol analysis
* Incident response

---

## Wireless Networking

### `iwconfig`

Displays wireless interface information.

```bash id="wifi1"
iwconfig
```

---

### `iw dev`

Displays information about wireless devices and interfaces.

```bash id="wifi2"
iw dev
```

---

## Host Information

### `hostname`

Displays the system hostname.

```bash id="host1"
hostname
```

---

## Domain Information

### `whois`

Displays domain registration information.

Example:

```bash id="whois1"
whois example.com
```

Useful for:

* Domain reconnaissance
* Ownership information
* Registration dates
* Registrar information
