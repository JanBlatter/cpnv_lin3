# Chrony

## Configure as server

```
#!/bin/bash

systemctl disable --now systemd-timesyncd.service

apt install -y chrony

systemctl enable --now chrony.service

cat << EOF > /etc/chrony/chrony.conf
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usable directives.

# Include configuration files found in /etc/chrony/conf.d.
confdir /etc/chrony/conf.d

# Use Debian vendor zone.
server 0.ie.pool.ntp.org iburst
server 1.ie.pool.ntp.org iburst
server 2.ie.pool.ntp.org iburst
server 3.ie.pool.ntp.org iburst

allow 127.0.0.0/8
allow 10.0.5.0/24

bindaddress 0.0.0.0

# Use time sources from DHCP.
sourcedir /run/chrony-dhcp

# Use NTP sources found in /etc/chrony/sources.d.
sourcedir /etc/chrony/sources.d

# This directive specify the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specify the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Save NTS keys and cookies.
ntsdumpdir /var/lib/chrony

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can't be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3

# Get TAI-UTC offset and leap seconds from the system tz database.
# This directive must be commented out when using time sources serving
# leap-smeared time.
leapsectz right/UTC
EOF

systemctl restart chrony.service
```
# Capture

## tcpdump

```
root@ip-10-0-5-10:~# tcpdump port 123
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ens5, link-type EN10MB (Ethernet), snapshot length 262144 bytes
08:06:22.322531 IP ip-10-0-5-10.45167 > time.cloudflare.com.ntp: NTPv4, Client, length 48
08:06:22.327842 IP time.cloudflare.com.ntp > ip-10-0-5-10.45167: NTPv4, Server, length 48
08:06:22.459877 IP ip-10-0-5-10.50422 > ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp: NTPv4, Client, length 48
08:06:22.460496 IP ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp > ip-10-0-5-10.50422: NTPv4, Server, length 48
08:06:22.887757 IP ip-10-0-5-10.35197 > t2.time.ir2.yahoo.com.ntp: NTPv4, Client, length 48
08:06:22.889269 IP t2.time.ir2.yahoo.com.ntp > ip-10-0-5-10.35197: NTPv4, Server, length 48
08:06:23.498294 IP ip-10-0-5-10.35592 > ntp.waltoninstitute.ie.ntp: NTPv4, Client, length 48
08:06:23.503853 IP ntp.waltoninstitute.ie.ntp > ip-10-0-5-10.35592: NTPv4, Server, length 48
08:07:26.682992 IP ip-10-0-5-10.54976 > ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp: NTPv4, Client, length 48
08:07:26.683590 IP ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp > ip-10-0-5-10.54976: NTPv4, Server, length 48
08:07:26.826018 IP ip-10-0-5-10.38732 > time.cloudflare.com.ntp: NTPv4, Client, length 48
08:07:26.827666 IP time.cloudflare.com.ntp > ip-10-0-5-10.38732: NTPv4, Server, length 48
08:07:27.026199 IP ip-10-0-5-10.45618 > t2.time.ir2.yahoo.com.ntp: NTPv4, Client, length 48
08:07:27.027787 IP t2.time.ir2.yahoo.com.ntp > ip-10-0-5-10.45618: NTPv4, Server, length 48
08:07:28.588290 IP ip-10-0-5-10.53598 > ntp.waltoninstitute.ie.ntp: NTPv4, Client, length 48
08:07:28.593476 IP ntp.waltoninstitute.ie.ntp > ip-10-0-5-10.53598: NTPv4, Server, length 48
08:07:46.760379 IP 10.0.5.11.ntp > ip-10-0-5-10.ntp: NTPv4, symmetric active, length 48
08:07:46.760467 IP ip-10-0-5-10.ntp > 10.0.5.11.ntp: NTPv4, symmetric passive, length 48
08:08:31.028765 IP ip-10-0-5-10.36100 > ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp: NTPv4, Client, length 48
08:08:31.029486 IP ec2-52-17-231-73.eu-west-1.compute.amazonaws.com.ntp > ip-10-0-5-10.36100: NTPv4, Server, length 48
08:08:31.185458 IP ip-10-0-5-10.48189 > time.cloudflare.com.ntp: NTPv4, Client, length 48
08:08:31.186976 IP time.cloudflare.com.ntp > ip-10-0-5-10.48189: NTPv4, Server, length 48
08:08:31.386409 IP ip-10-0-5-10.52739 > t2.time.ir2.yahoo.com.ntp: NTPv4, Client, length 48
08:08:31.387611 IP t2.time.ir2.yahoo.com.ntp > ip-10-0-5-10.52739: NTPv4, Server, length 48
08:08:32.935407 IP ip-10-0-5-10.51836 > ntp.waltoninstitute.ie.ntp: NTPv4, Client, length 48
08:08:32.941942 IP ntp.waltoninstitute.ie.ntp > ip-10-0-5-10.51836: NTPv4, Server, length 48
08:08:51.003276 IP 10.0.5.11.ntp > ip-10-0-5-10.ntp: NTPv4, symmetric active, length 48
08:08:51.003365 IP ip-10-0-5-10.ntp > 10.0.5.11.ntp: NTPv4, symmetric passive, length 48
```

## nmap

```
root@ip-10-0-5-10:~# nmap -Pn 10.0.5.12 -sU -p 123
Starting Nmap 7.93 ( https://nmap.org ) at 2023-12-14 07:55 UTC
Nmap scan report for 10.0.5.12
Host is up (0.00016s latency).

PORT    STATE SERVICE
123/udp open  ntp
MAC Address: 06:12:2F:2D:29:9F (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.17 seconds
```

# Sources

https://www.linuxtricks.fr/wiki/ntp-gestion-de-l-heure-par-le-reseau-avec-chronyd

https://www.ntppool.org/zone/ie

https://hackertarget.com/tcpdump-examples/
