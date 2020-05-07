
* [Basic Scanning](#basic-scanning)
* [Proxy](#proxy)

# Basic Scanning
```sh
nmap -A -v -p- 192.143.147.3
nmap -sC -v -p- 192.143.147.3
```
# Proxy
## Proxy Authentication
If the proxy is configured with authentication, any attempts made via proxy will return an `access denied` error. In case the proxy is configured without authentication,  The connection will be forwarded by the proxy and the error message will be different than `Access denied` error.

By using the command `curl -x 192.201.208.3:3128 127.0.0.1`. We are looking for a different error message than `Access Denied`. Provided the proxy does not use any authentication, in case if a service is running, (for e.g an apache server) an HTTP response will be received, if no service is running then the connection refused error will be received. Either of the error will confirm that the proxy is configured without authentication.

## Proxychains

Proxychains is a tool which forces the TCP connection to go through the configured proxy(s). Proxychians can force the connection through multiple proxy servers. For e.g, if you have the following proxychains configuration (with strict chain option):

- socks5 192.200.34.4
- socks5 192.200.33.5
- socks4 192.200.32.6

The TCP connection will be sent through proxy servers in the following order.

> Host  <---> 192.200.34.4  <--->192.200.33.5 <---> 192.200.32.6 <---> Target

If any of the proxy servers fails to connect, the TCP connection won't be established. To skip a dead proxy server and move on to the next proxy server *Dynamic chain* option is used.

The *random chain* option is used to randomly select the order of the proxy server for forwarding the TCP connection.

Upon starting a program, for e.g Mozilla Firefox with the command: `proxychains firefox`, All the TCP traffic of Mozilla Firefox will be forced through the proxy server.

In this case, to scan the target machine, we are forcing the traffic of nmap to go through the HTTP Proxy server.

Please note: nmap also has an `--proxies` option. But it is not used as it does not work as expected, On the Nmap documentation (https://nmap.org/book/man-bypass-firewalls-ids.html), it is mentioned that the feature is till under development and has limitations.


