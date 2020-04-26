* [Basic Scanning](#bash-scanning)
* [Directory Listing](#directory-listing)
* [Header Manipulations](#header-manipulations)
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
## Using `curl` and bash script
We can use curl to check for all the directories listed in the wordlist. This can be done by a following wrapper bash script
<https://github.com/jai-the-seeker/CTF-OSCP/blob/master/scripts.md#directory-listing>

# Header Manipulations
## Fetch Header
```sh
# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
# msfconsole
use auxiliary/scanner/http/http_header
```
## wfuzz
Refs
* <https://wfuzz.readthedocs.io/en/latest/user/basicusage.html#fuzzing-paths-and-files>
There are two db wordlists associated with `wfuzz` which can be obtained from
* <https://code.google.com/p/fuzzdb/>
* <https://github.com/danielmiessler/SecLists>

We can use `wfuzz` to check for various valid strings of the `User-Agent` by passing it a wordlist containing possible user agents strings.
The word list can of user-agents can be obtained from <https://github.com/fuzzdb-project/fuzzdb/blob/master/discovery/UserAgent/UserAgentListCommon.txt>
```sh
wfuzz -c -z file,user-agent.txt -H "User-Agent: FUZZ" http://192.8.221.3/secret
```
output
```sh

000001:  C=200      1 L        2 W           39 Ch        "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-GB; rv:1.8.1.6) Gecko/20070725 Firefox/2.0.0.6"
000002:  C=403      1 L        2 W           15 Ch        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)"
```
Here response code of `200` indicates that the `user-agent-string` is valid. The same can now be used with `curl` to verify and access the webpage
```sh
curl -H "User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-GB; rv:1.8.1.6) Gecko/20070725 Firefox/2.0.0.6" 192.8.221.3/secret
```
