# Networking in Linux

## ip command

list interfaces: ip a
loopback ipv4 = 127.0.0.1/8
loopback ipv6 = ::1/128

## Interface naming
Since systemd took over, a more predictable naming scheme is used.

en = ethernet
wl = wireless
p = bus number of the ethernet card
s = slot number of the ethernet card
