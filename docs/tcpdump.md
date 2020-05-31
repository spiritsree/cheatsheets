# Tcpdump

Tool to capture and analyze network traffic

## Find everything on an interface

```
$ tcpdump -i eth0
```

## Find traffic by IP

```
$ tcpdump host 8.8.8.8
```

## Filtering by Source, Destination and port and protocol

```
$ tcpdump src 192.168.1.104
$ tcpdump dst 8.8.8.8
$ tcpdump -nnvvS src 192.168.1.104 and dst port 80
```

Destined to a specific IP and not ICMP

```
$ tcpdump dst 8.8.8.8 and src net and not icmp
```

Traffic From a source and not a spcific port

```
$ tcpdump -vv src 192.168.1.104 and not dst port 22
```

## Finding Packets by Network

```
$ tcpdump net 192.168.1.0/24
$ tcpdump -nvX src net 192.168.1.0/24 and dst net 10.0.0.0/8 or 172.16.0.0/16
```

## Get Packet Contents with Hex Output

```
$ tcpdump -c 1 -X icmp
```

## Show Traffic Related to a Specific Port

```
$ tcpdump port 80
$ tcpdump src port 8888
```

## Show Traffic of One Protocol

```
$ tcpdump icmp
```

## Show only IP6 Traffic

```
$ tcpdump ip6
```

## Find Traffic Using Port Ranges

```
$ tcpdump portrange 20-21
```

## Find Traffic Based on Packet Size

```
$ tcpdump less 32
$ tcpdump greater 64
$ tcpdump <= 128
```

## Reading / Writing Captures to a File (pcap)

```
$ tcpdump port 80 -w capture_file
```

```
$ tcpdump -r capture_file
```

## Isolate TCP Flags

Isolate TCP RST flags

```
$ tcpdump 'tcp[13] & 4!=0'
$ tcpdump 'tcp[tcpflags] == tcp-rst'
```

Isolate TCP SYN flags

```
$ tcpdump 'tcp[13] & 2!=0'
$ tcpdump 'tcp[tcpflags] == tcp-syn'
```

Isolate packets that have both SYN and ACK flag set

```
$ tcpdump 'tcp[13]=18'
```

Isolate TCP URG flag

```
$ tcpdump 'tcp[13] & 32!=0'
$ tcpdump 'tcp[tcpflags] == tcp-urg'
```

Isolate TCP ACK flag

```
$ tcpdump 'tcp[13] & 16!=0'
$ tcpdump 'tcp[tcpflags] == tcp-ack'
```

Isolate TCP PSH flag

```
$ tcpdump 'tcp[13] & 8!=0'
$ tcpdump 'tcp[tcpflags] == tcp-psh'
```

Isolate TCP FIN flags

```
$ tcpdump 'tcp[13] & 1!=0'
$ tcpdump 'tcp[tcpflags] == tcp-fin'
```

Both SYN and RST set

```
$ tcpdump 'tcp[13] = 6'
```

## HTTP Traffic filtering

Find user agent

```
$ tcpdump -vvAls0 | grep 'User-Agent:'
```

Cleartext GET requests

```
$ tcpdump -vvAls0 | grep 'GET'
```

Get Host headers

```
$ tcpdump -vvAls0 | grep 'Host:'
```

Find Cookies

```
$ tcpdump -vvAls0 | grep 'Set-Cookie|Host:|Cookie:'
```

Monitor HTTP traffic including request and response headers and message body

```
$ tcpdump -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

Monitor HTTP traffic including request and response headers and message body from a particular source

```
$ tcpdump -A -s 0 'src example.com and tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

Monitor HTTP traffic including request and response headers and message body from local host to local host

```
$ tcpdump -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' -i lo
```

How to capture All incoming  HTTP GET traffic (or) requests

```
$ tcpdump -s 0 -A 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420'
```

`0x47455420` depicts the ASCII value of  characters  `'G' 'E' 'T' ' '`

How to capture All incoming HTTP POST requests

```
$ tcpdump -s 0 -A 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504F5354'
```

`0x504F5354` represents  the ASCII value of  `'P' 'O' 'S' 'T'`

## SSH Connections

```
$ tcpdump 'tcp[(tcp[12]>>2):4] = 0x5353482D'
```

## DNS Traffic

```
$ tcpdump -vvAs0 port 53
```

## FTP Traffic

```
$ tcpdump -vvAs0 port ftp or ftp-data
```

## NTP Traffic

```
$ tcpdump -vvAs0 port 123
```

## Find traffic with evil bit set

```
$ tcpdump 'ip[6] & 128 != 0'
```
