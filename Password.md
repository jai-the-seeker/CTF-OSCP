* [Hashcat](#hashcat)
* [john the ripper](#john-the-ripper)

# Hashcat
Refs:
* <https://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-passwords-part-3-using-hashcat-0156543/>
## SHA-512 Dictionary Attack
```sh
hashcat -m 1800 -a 0 -o cracked.txt --remove hash.lst /usr/share/sqlmap/txt/wordlist.txt
```
`-m` 1800 designates the type of hash we are cracking (SHA-512)

`-a 0` designates a dictionary attack

`-o` cracked.txt is the output file for the cracked passwords

`--remove` tells hashcat to remove the hash after it has been cracked

`hash.lst` is our input file of hashes

`/usr/share/sqlmap/txt/wordlist.txt` is the absolute path to our wordlist for this dictionary attack




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

