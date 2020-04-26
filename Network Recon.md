# Basic Scanning
```sh
# version of the running services
# -sV: Probe open ports to determine service/version info
# -sC: equivalent to --script=default
nmap -sV -sC 192.165.34.3
```
# Directory Listing
```sh
# bruteforce on web server directories and list the names of directories found using msfconsole
use auxiliary/scanner/http/brute_dirs

# DIRB  IS  a  Web Content Scanner. It looks for existing (and/or hidden) Web Objects. It basically works by
# launching a dictionary basesd attack against a web server and analizing the response.
# Note: Please remember to remove the preceding ‘/’ from each of directory name entry (in the
# directory.txt). Without this change, dirb won’t work.
dirb http://192.166.161.3 directory.txt
```
### Using `curl` and bash script
<https://github.com/jai-the-seeker/CTF-OSCP/blob/master/scripts.md#directory-listing>
# Fetch Headers
```sh
# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
# msfconsole
use auxiliary/scanner/http/http_header
```
