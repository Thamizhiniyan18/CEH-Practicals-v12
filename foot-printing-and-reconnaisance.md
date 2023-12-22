# Foot Printing and Reconnaisance

## Directory Enumeration - Finding  Directories in a Website

### Using Gobuster

```bash
gobuster dir -u http://10.10.10.1/ -w usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

### Using ffuf

```bash
ffuf -u http://10.10.10.11/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

***

## File Enumeration - Finding Specific files in a Website

### Using Gobuster

```bash
gobuster dir -u http://10.10.10.1/ -w usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .html,.css,.js  
```

### Using ffuf

```bash
ffuf -u http://10.10.10.11/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .html,.css,.js,.conf
```

***

## VHOST Enumeration - Finding Subdomains of a Website

### Using Gobuster

```bash
gobuster vhost -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain
```

### Using ffuf

```bash
ffuf -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -H “HOST:FUZZ.example.com”
```

***

## Digital Certificates

We can find subdomains from certificates issued to the main domain as sometimes they all use the same certificate.

#### Digital Certificates Search Engines

* [https://crt.sh/](https://crt.sh/)
* [https://ui.ctsearch.entrust.com/ui/ctsearchui](https://ui.ctsearch.entrust.com/ui/ctsearchui)
* [https://search.censys.io/](https://search.censys.io/)

***

## DNS Enumeration

### Automated Tools

* `dnsrecon -d zonetransfer.me -t axfr`
* `dnsenum zonetransfer.me`
* `fierce --domain zonetransfer.me`

### Linux

* `dig <ip/domain>` - Normal / DNS lookup
* `dig ns zonetransfer.me` - Name Server
* `dig mx zonetransfer.me` - Mail Server
* `dig cname zonetransfer.me` - cname record
* `host zonetransfer.me` - Normal / DNS lookup
* `host -t ns zonetransfer.me` - Name Server
* `host -t mx zonetransfer.me` - Mail Server
* `host -t cname zontransfer.me` - cname record
* `host <IP>` - Reverse Lookup

### Windows

* `nslookup zonetransfer.me`
* Just type `nslookup` to enter interactive mode in windows.
* Then type `set type=ns`, press enter \[ type = ns, txt, …. ]
* Next type `zonetransfer.me`, press enter

### Zone Transfer

#### Finding Name Servers

First find the name servers using any one of the following commands:

* `host -t ns zonetransfer.me`
* `dig ns zonetransfer.me`

Then Check each name server for zone transfer using the following commands:

#### Using host

```bash
host -l zonetransfer,me 
```

#### Using dig

```bash
dig axfr zonetransfer.me @<nameserver>
```

#### Using nslookup

```bash
# Interactive Mode
nslookup
> set type=ns
> zonetransfer.me
> server <nameserver>
> set type=any
> ls -d zonetransfer.me
```
