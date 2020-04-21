# Basic Commands
```sh
# -D : print list of interfaces and exit
tshark -D

# -i <interface> name or idx of interface
tshark -i eth0

# Input file:
#  -r <infile> set the filename to read from (- to read from stdin)
tshark -r HTTP_traffic.pcap
```
