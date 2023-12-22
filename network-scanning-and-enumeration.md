# Network Scanning and Enumeration

## Automated Way

### Using enum4linux

Network Enumeration Tool

{% embed url="https://github.com/CiscoCXSecurity/enum4linux" %}

```bash
enum4linux -a <target_IP>
```

***

## Host Discovery - to find other hosts/machines in the network

### Using netdisocver

Used to scan for live hosts on the network

```bash
netdiscover -i eth0 # ( netdiscover -i <interface_name> )
```

### Using nmap

| Description                                                                | Command                                                           |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Ping Sweep** \[ Scanning Network for Live Hosts ]                        | `nmap -sP 192.168.18.1/24`                                        |
| **ARP Scan** \[ Scanning for Live Hosts without port scan in same subnet ] | `nmap -sn -PR 192.168.18.0-255` or `nmap -sn -PR 192.168.18.1/24` |
| **UDP ping scan**                                                          | `nmap -sn -PU 192.168.18.110`                                     |
| **ICMP echo ping scan**                                                    | `nmap -sn -PE 192.168.18.1-255`                                   |
| **Mask ping scan** ( use if ICMP is blocked )                              | `nmap -sn -PM 192.168.18.1-255`                                   |
| **ICMP timestamp scan**                                                    | `nmap -sn -PP 192.168.18.1-255`                                   |
| **TCP SYN Ping scan**                                                      | `nmap -sn -PS 192.168.18.1-255`                                   |
| **IP Protocol scan** ( uses different protocols to test connectivity )     | `nmap -sn -PP 192.168.18.1-255`                                   |

### Using Angry IP Scanner

{% embed url="https://angryip.org/" %}

* Preference → Pinging Method → Combined UDP + TCP
* Display → only live hosts
* Start

***

## Service Discovery - To identify open ports and services running on target

### Using nmap

| Description           | Command                       |
| --------------------- | ----------------------------- |
| **All Open Ports**    | `nmap -p- 192.168.18.1`       |
| **Specific Port**     | `nmap -p <PORT> 192.168.18.1` |
| **Service + Version** | `nmap -sS -sV 192.168.18.1`   |
| **Scripts + Version** | `nmap -sC -sV 192.168.18.1`   |

### Using hping3

```
hping3 -S 192.168.18.110 -p 80 -c 5
```

* `-S` - TCP Stealth Scan

***

## OS Discovery - Identify the OS running on the target

### Using nmap

Services + OS Discovery

```bash
sudo nmap -sS -O 192.168.18.1
```

Using nmap nse scripts

```bash
sudo nmap --script smb-os-discovery.nse 192.168.18.110
```

### Banner Grabbing

Based the `ttl` value present the ping response.

* `ping 192.168.18.110`

| Operating System | Time To Live ( ttl ) | TCP Window Size      |
| ---------------- | -------------------- | -------------------- |
| Linux            | 64                   | 5840                 |
| FreeBSD          | 64                   | 65535                |
| OpenBSD          | 255                  | 16384                |
| Windows          | 128                  | 65,535 bytes to 1 GB |
| Cisco Routers    | 255                  | 4128                 |
| Solaris          | 255                  | 8760                 |
| AIX              | 255                  | 16384                |

***

## Aggressive Scanning

```bash
nmap -A -v -T4 192.168.18.1
```

***

## Comprehensive Scanning

```bash
nmap -sC -sV -p- -A -v -T4 192.168.18.1
```
