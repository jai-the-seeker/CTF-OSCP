* [Password Attack](#Password-Attack)
* [john the ripper](#john-the-ripper)

# Password Attack
## Wordlists
Generate a custom wordlist
cewl -w createWordlist.txt -m <min password length> https://www.example.com

## Offline
https://hashes.org/search.php

# john the ripper
```sh
john --wordlist=/user/share/wordlists/rockyou.txt hash.txt
```

Hashcat << check type online - hashcat sample hash
hashcat -m\<type> -a 0 /usr/share/wordlists/rockyou.txt hash.txt

# WiFi
## WEP
```sh
# -b <bssid> : target selection: access point's MAC
aircrack-ng -b 00:21:91:d2:8e:25 WEP-Cracking.cap
```

