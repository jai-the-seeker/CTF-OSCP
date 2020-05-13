
* [Basic Scanning](#basic-scanning)
* [Proxy](#proxy)
  * [Proxy Authentication](#proxy-authentication)
    * [Open Authentication](#open-authentication)
    * [username and password](#username-and-password)
    * [bruteforce](#bruteforce)
  * [Proxychains](#proxychains)
    * [Configure proxychain](#configure-proxychain)
    * [Proxychain with `nmap`](#proxychain-with-nmap)
    * [Proxychain with `curl`](#proxychain-with-curl)
  

# Basic Scanning
```sh
nmap -A -v -p- 192.143.147.3
nmap -sC -v -p- 192.143.147.3
```
# Proxy
## Proxy Authentication
### Open Authentication
If the proxy is configured with authentication, any attempts made via proxy will return an `access denied` error. In case the proxy is configured without authentication, the connection will be forwarded by the proxy and the error message will be different than `Access denied` error.

By using the command `curl -x 192.201.208.3:3128 127.0.0.1`. We are looking for a different error message than `Access Denied`. Provided the proxy does not use any authentication, in case if a service is running, (for e.g an apache server) an HTTP response will be received, if no service is running then the connection refused error will be received. Either of the error will confirm that the proxy is configured without authentication. The following example shows that squid proxy is configured without authentication.
```sh
$ curl -i -x 192.234.192.3:3128 127.0.0.1

HTTP/1.1 503 Service Unavailable
Server: squid/3.5.12
...
X-Squid-Error: ERR_CONNECT_FAIL 111
Vary: Accept-Language
Content-Language: en
X-Cache: MISS from victim-1
X-Cache-Lookup: MISS from victim-1:3128
Via: 1.1 victim-1 (squid/3.5.12)
Connection: keep-alive

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html><head>
<meta type="copyright" content="Copyright (C) 1996-2015 The Squid Software Foundation and contributors">
<meta http-equiv="Content-Type" CONTENT="text/html; charset=utf-8">
<title>ERROR: The requested URL could not be retrieved</title>
<style type="text/css"><!-- 
...
.
<blockquote id="error">
<p><b>Connection to 127.0.0.1 failed.</b></p>
</blockquote>
...
```
### username and password
#### HTTP server authentication
The curl `-u` option is used to specify credentials for the *target* server authentication and not for the *proxy* server authentication.

For e.g, when you are using the command `curl -u admin:laurie -x 192.173.117.3:3128 127.0.0.1:1996`. The credentials `admin:laurie` are *not* being passed as credentials to the proxy server, instead, they are passed as a part of the request which is supposed to be forwarded to the target server.  The option value will be considered as credentials for the target server authentication and not for the proxy authentication.

The above command will work in the scenario where the proxy server is configured without authentication, but the target server is protected with authentication (basic/digest authentication). This way, the connection will be forwarded by the proxy server and the credentials will be used to authenticate with the target server.
#### proxy authnetication
In order to pass the credentials to the proxy server, you will have to use the curl option `-U`. Therefore, the following command will work:
```sh
curl -U admin:laurie -x 192.173.117.3:3128 127.0.0.1:1996
```
Alternatively,
```sh
# curl -x <[protocol://][user:password@]proxyhost[:port]> url
# Example 1
curl -x http://user:password@proxy-IP-here:port url-of-website:port
# Example 2
curl -x admin:laurie@192.142.49.3:3128 127.0.0.1:1996
```
### bruteforce
#### nmap
```sh
nmap --script http-proxy-brute -p3128 192.144.18.3 --script-args userdb=usernames.lst,passdb=passwords.lst
```
#### bash script
<https://github.com/jai-the-seeker/CTF-OSCP/blob/master/scripts.md#proxy-authentication-dictionary-attk>
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

### Configure proxychain
```sh
cat /etc/proxychains.conf

# Add details of http proxy at the end of the file
http 192.234.192.3 3128
```
### Proxychain with `nmap`
We can use proxychain, to scan the target machine, we are forcing the traffic of `nmap` to go through the HTTP Proxy server.
Since, we have configured `http` proxy, we need to use `-sT` flag with `nmap`.
```sh
proxychains nmap -sV -sT -p- 127.0.0.1
```
Please note: nmap also has an `--proxies` option. But it is not used as it does not work as expected, On the Nmap documentation (https://nmap.org/book/man-bypass-firewalls-ids.html), it is mentioned that the feature is till under development and has limitations.

### Proxychain with `curl`
```sh
curl -x 192.201.208.3:3128 127.0.0.1:1337

# The above command is same as
proxychains curl 127.0.0.1:1337
```
