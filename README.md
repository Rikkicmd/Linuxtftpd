Setup the expected IP address:

    linux$ sudo ifconfig eth0:0 192.0.0.128
    osx$   sudo ifconfig en0 alias 192.0.0.128 255.255.255.0

Download the firmware to use or if already downloaded copy to the same directory as hikserv.py

    $ curl -o digicap.dav <url of firmware>

Run the script:

    $ sudo ./hikvision_tftpd.py

Hit ctrl-C when done.

----------------------

Expected IP addresses and file name appear to differ by model.

| client IP    | server IP    | filename      |
| ------------ | ------------ | ------------- |
| 192.0.0.64   | 192.0.0.128  | `digicap.dav` |
| 172.9.18.100 | 172.9.18.80  | `digicap.mav` |

This program defaults to the former. The latter requires commandline overrides:

    $ sudo ./hikvision_tftp.py --server-ip=172.9.18.80 --filename=digicap.mav

If nothing happens when the D/NVR restarts, your device may be expecting
another IP address. tcpdump may be helpful in diagnosing this:

$ sudo tcpdump -i eth0 -vv -e -nn ether proto 0x0806
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
16:21:58.804425 28:57:be:8a:aa:53 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Ethernet (len 6), IPv4 (len 4), Request   who-has 172.9.18.80 tell 172.9.18.100, length 46
16:22:00.805251 28:57:be:8a:aa:53 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Ethernet (len 6), IPv4 (len 4), Request who-has 172.9.18.80 tell 172.9.18.100, length 46


This shows that the recorder is looking for 172.9.18.80 to get its .dav/.mav file from.
