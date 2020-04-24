# Login
## HTTP
```sh
# HTTP Basic Authentication
curl -u bob:qwerty http://192.165.34.3/dir/

# HTTP Digest Authentication
curl --digest -u alice:password1 http://192.165.34.3/poc/

# HTTP Token Authentication
curl -H 'Authorization: Token 123123123' 192.186.248.3
```
