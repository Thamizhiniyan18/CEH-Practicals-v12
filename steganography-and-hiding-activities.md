# Steganography and Hiding Activities

## Covert Communication

### Using Covert TCP

For hiding data in TCP/IP packet headers.

{% @github-files/github-code-block %}

#### Configuring Sender

Setup

{% code lineNumbers="true" %}
```bash
echo "Secret Message!" > message.txt
wget https://raw.githubusercontent.com/cudeso/security-tools/master/networktools/covert/covert_tcp.c`
cc -o covert_tcp covert_tcp.c
```
{% endcode %}

Sending Message

{% code overflow="wrap" %}
```bash
./covert_tcp -dest 10.0.2.46 -source 10.0.2.42 -source_port 8888 -dest_port 9999 -file /root/Desktop/send/message.txt
```
{% endcode %}

#### Configuring Receiver

Setup

```bash
wget https://raw.githubusercontent.com/cudeso/security-tools/master/networktools/covert/covert_tcp.c
cc -o covert_tcp covert_tcp.c
```

Starting the Listener

{% code overflow="wrap" %}
```bash
./covert_tcp -dest 10.0.2.46 -source 10.0.2.42 -source_port 9999 -dest_port 8888 -server -file /home/s4msepi0l/Desktop/receive/receive.txt
```
{% endcode %}

***

## White Space Steganography

### Using Snow

{% embed url="https://darkside.com.au/snow/manual.html" %}

For hiding and extracting hidden data from a text file.

#### Hiding Text

```bash
snow.exe -C -m "Someone this something" -p "test" original.txt modifiedOriginalFile.txt
```

* `-m` : Message
* `-p` : Password

#### Revealing Text

```bash
snow.exe -C -p "test" modifiedOriginalFile.txt
```

### Using Stegsnow

{% embed url="https://manpages.ubuntu.com/manpages/jammy/man1/stegsnow.1.html" %}

***

## Image Steganography

### Openstego

{% embed url="https://www.openstego.com/" %}

For hiding and extracting hidden data from an image file.

### Stegonline

{% embed url="https://stegonline.georgeom.net/upload" %}

***

## Alternate Data Streams

Hiding files using alternate data streams.

* Copy calc from system32 folder to your test folder, Now create a text file and append the cal.exe to the file:

```powershell
type calc.exe > readme.txt:calc.exe
```

* Now create a link to the ADS file to create backdoor:&#x20;

```powershell
mklink backdoor.exe readme.txt:calc.exe
```

* Opening the backdoor link will open the hidden file.
