* [Hashcat](#hashcat)
  * [delete found password](#delete-found-password)
  * [Mask Based Attack](#mask-based-attack)
    * [Increment Mode](#increment-mode)
    * [Hashcat Mask Files](#hashcat-mask-files)
  * [Dictionary Attack](#dictionary-attack)
  * [MD5](#md5)
  * [HMAC](#hmac)
  
* [john the ripper](#john-the-ripper)

# Hashcat
## delete found password
You can disable potfile support completely by using `--potfile-disable`
```sh
# find hashcat.potfile
find / -type f -name 'hashcat.potfile'

# -n which tells echo to not output the trailing newline
echo -n "" > /hashcat/hashcat-4.2.1/hashcat.potfile
```
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
## Dictionary Attack
Refs:
* <https://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-passwords-part-3-using-hashcat-0156543/>
```sh
hashcat -m 1800 -a 0 -o cracked.txt hash.lst /usr/share/sqlmap/txt/wordlist.txt
```
`-m` 1800 designates the type of hash we are cracking (SHA-512)

`-a 0` designates a dictionary attack

`-o` cracked.txt is the output file for the cracked passwords

`hash.lst` is our input file of hashes

`/usr/share/sqlmap/txt/wordlist.txt` is the absolute path to our wordlist for this dictionary attack
## MD5
```sh
# -m 0 : MD5 without salt

# MD5 hash:salt 
# cf0b18ddb1a31d05fc73f50fcd29e0a8:salt123
hashcat -m 10 -a 0 digest.txt password-seclists.txt
```
## HMAC
HMAC is a keyed hash (authenticated hash) scheme which ensures that a specific hash value can only be generated if the entity possess a secret key. This scheme can be used to turn any existing hash function into an authenticated hash function which can be then used to check the authenticity of the message in addition to its integrity. HMAC-SHA1 was widely used in online banking security, HTTPS, VPN connections in addition to verify the integrity of the files/binaries. In essence, it is mostly used to protect the data in transit over insecure mediums. 

A plain-text string and corresponding HMAC-SHA1 digest is provided in digest.txt file. The key used to generate the HMAC-SHA1 is either taken from a key dictionary or by using the key policy. The `digest.txt` file and the dictionary file `password-seclists.txt` is present in the user's home directory.

Objective: Recover the secret key.
```sh
# hash:plaintext
# 69f7e54d484620ed6e9d731ca51780a000463fc2:tinkerbell97
hashcat -m 150 -a 0 digest.txt password-seclists.txt
```

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

