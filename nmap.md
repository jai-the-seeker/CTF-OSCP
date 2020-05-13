* [scripts](#scripts)
  * [brute](#brute)
    * [http-proxy-brute](#http-proxy-brute)
# scripts
## brute
### http-proxy-brute
The http-brute script uses, by default, the database files `usernames.lst` and `passwords.lst` located at `/nselib/data/` to try each password, for every user
```sh
nmap --script http-proxy-brute -p3128 192.144.18.3
```
To use different username and password lists, set the arguments `userdb` and `passdb` : 
```sh
nmap --script http-proxy-brute -p3128 192.144.18.3 --script-args userdb=usernames.lst,passdb=passwords.lst
```
