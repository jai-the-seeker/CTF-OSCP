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
# Statistics

`-z io,phs[,filter]`

Create Protocol Hierarchy Statistics listing both number of packets and bytes. If no filter is specified the statistics will be calculated for all packets. If a filter is specified statistics will only be calculated for those packets that match the filter.

```
tshark -r HTTP_traffic.pcap -z io,phs -q
```

Refs:
* <https://www.wireshark.org/docs/man-pages/tshark.html>
