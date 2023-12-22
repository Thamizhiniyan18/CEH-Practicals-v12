# Service Enumeration

## FTP - 21

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/pentesting-ftp" %}

### Using hydra

Bruteforcing Credentials

```bash
hydra -L usernames.txt -P passwords.txt <IP> ftp
```

***

## Telnet - 23

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/pentesting-telnet" %}

```bash
telnet <IP> <PORT>
```

***

## NetBios - 137, 138, 139

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/137-138-139-pentesting-netbios" %}

### Ports

| Port | Protocol | Service                        |
| ---- | -------- | ------------------------------ |
| 137  | UDP      | NetBIOS Name Service (NBNS)    |
| 138  | UDP      | NetBIOS Datagram Service (NDS) |
| 139  | TCP      | NetBIOS over TCP/IP (NBT)      |

### Using Nbtstat - Windows

Windows Command Line Utility

```bash
nbtstat -a 192.168.18.100
```

Check for local cache

```bash
nbtstat -c
```

### Using nmap

{% embed url="https://nmap.org/nsedoc/scripts/nbstat.html" %}

```bash
nmap -sV -v --script nbtstat.nse 192.168.18.110
```

```bash
nmap -sU -p 137 --script nbtstat.nse 192.168.18.110
```

* `-sV` - Version Enumeration
* `-sU` - UDP scan

***

## SMB - 139, 445

Server Message Block

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb" %}

{% embed url="https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html" %}

### Methodology

Look out for

* Network File Shares
* Logged in User Details
* Workgroups
* Security Level Information
* Domains and Services

### Ports

| Port | Protocol | Service                                           |
| ---- | -------- | ------------------------------------------------- |
| 137  | UDP      | NetBIOS Name Service (NBNS) by SMB                |
| 138  | UDP      | NetBIOS Datagram Service (NDS) by SMB             |
| 139  | TCP      | SMB in conjunction with NetBIOS over TCP/IP (NBT) |
| 445  | TCP      | Primary Port                                      |

### Services Examples

* netbios-ssn
* microsoft-ds

### Using enum4linux

```bash
enum4linux -a 10.10.10.10
```

### Using nmap

#### To List All Scripts for SMB

To list all scripts by Nmap for SMB enumeration

```bash
cd /usr/share/nmap/scripts; ls | grep smb 
```

#### OS Discovery

{% embed url="https://nmap.org/nsedoc/scripts/smb-os-discovery.html" %}

```bash
sudo nmap --script smb-os-discovery.nse 192.168.18.110
```

#### Enumerating Shares

{% embed url="https://nmap.org/nsedoc/scripts/smb-enum-shares.html" %}

{% code fullWidth="false" %}
```bash
nmap --script smb-enum-shares.nse -p445 <host>
```
{% endcode %}

```bash
nmap -sU -p 445 --script=smb-enum-shares <target>
```

#### Enumerating Users

{% embed url="https://nmap.org/nsedoc/scripts/smb-enum-users.html" %}

```bash
nmap --script smb-enum-users.nse -p445 <host>
```

```bash
nmap -sU -p 445 --script=smb-enum-users <target>
```

#### Enumerating Groups

{% embed url="https://nmap.org/nsedoc/scripts/smb-enum-groups.html" %}

```bash
nmap -sU -p 445 --script=smb-enum-groups <target>
```

#### Enumerating Services

{% embed url="https://nmap.org/nsedoc/scripts/smb-enum-services.html" %}

```bash
nmap --script smb-enum-services.nse -p445 <host>
```

```bash
nmap -sU -p 445 --script=smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_77 <target>
```

#### Enumerating Security Level

```bash
nmap -sC -sV -A -T4 -p 445 <target> 
```

### Exploitation

{% embed url="https://github.com/irgoncalves/smbclient_cheatsheet" %}

#### Using smbclient

* `smbclient -L` - List shares on a machine using NULL Session
* `smbclient -L <target_IP> -U username%password` - List shares on a machine using a valid username + password
* `smbclient //<target>/<share$> -U username%password` - Connect to a valid share with username + password

***

## RDP - 3389

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/pentesting-rdp" %}

### Methodology

1. Check for running services on the target and confirm if RDP is running on any open port.
2. Use Metasploit to confirm the services running is RDP.
3. Use hydra to brute force the login credentials.
4. Use RDP tools to login into the victim's machine.

### Using Metasploit

#### Confirming RDP

```bash
msfconsole
> use auxiliary/scanner/rdp/rdp_scanner
```

### Using Hydra

Bruteforcing login credentials

```bash
hydra -L usernames.txt -P passwords.txt rdp://<target> -s <port>
```

### Using xfreerdp

Creating RDP session with `xfreerdp`

```bash
xfreerdp /u:administrator /p:qwertyuip /v:IP:PORT
```

***

## SNMP - 161, 162, 10161, 10162

{% embed url="https://book.hacktricks.xyz/network-services-pentesting/pentesting-snmp" %}

### Methodology

1. Look out for default UDP ports used by SNMP: 161, 162, 10161, 10162.
2. Identify the processes running on the target machine using nmap scripts.
3. List valid community strings of the server using nmap scripts.
4. List valid community strings of the server by using snmp\_login Metasploit Module.
5. List all the interfaces of the machine. Use appropriate nmap Script.

### Using snmp-check

```bash
snmp-check <IP>
```

### Using nmap

#### Identifying Processes

{% embed url="https://nmap.org/nsedoc/scripts/snmp-processes.html" %}

```bash
nmap -sU -p 161 --script=snmp-processes <target>
```

#### Identifying Interfaces

{% embed url="https://nmap.org/nsedoc/scripts/snmp-interfaces.html" %}

```
nmap -sU -p 161 --script=snmp-interfaces <target>
```

### Using Metasploit

#### Identifying Valid Community Strings

```bash
msfconsole
> use auxiliary/scanner/snmp/snmp_login
```

