* [Hashcat](#hashcat)
* [john the ripper](#john-the-ripper)

# Hashcat
## Mask Based Attack
Refs
* <https://hashcat.net/wiki/doku.php?id=mask_attack>
* <https://www.4armed.com/blog/perform-mask-attack-hashcat/>
```sh
command: -a 3 -1 ?l?d ?1?1?1?1?1
keyspace: aaaaa - 99999
```
```sh
# password length is 5 characters
# password can only contain characters from this character set:  a-z, 0-9
hashcat -m 0 -a 3 digest.txt -1 ?l?d ?1?1?1?1?1
```
### Increment Mode
```sh
# If you take the mask “?l?l?l?l?l?l?l” it will only try passwords of that length. To cycle through all the possible combinations from 
# one to seven, the “--increment argument” can be used.
?l
?l?l
?l?l?l
?l?l?l?l
?l?l?l?l?l
?l?l?l?l?l?l?l
```
### Hashcat Mask Files
Let us consider the following scenario for creating a password masking attack:

* Length between five and eight characters
* Always starts with a capital letter
* Always ends with a number
* The characters in the middle are either lower or upper case

For a single entry in a mask file, the following structure is used:
```
charset1,charset2,charset3,charset4,mask
```
It's important to highlight that the charset parameters are optional. So it's possible to create entries in the following format:
```
charset1,charset2,charset3,charset4,mask
charset1,charset2,mask
mask
```
To meet the previous scenario, we can create a mask file containing the following:
```
?u,?u?l,?d,?1?2?2?2?3
?u,?u?l,?d,?1?2?2?2?2?3
?u,?u?l,?d,?1?2?2?2?2?2?3
```
Mask files have the file extension of `.hcmask` and can be used from the command line like below:
```sh 
hashcat -m 0 -a 3 hash masks.hcmask
```

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

