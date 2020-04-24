
## HTTP Login
```sh
# HTTP Basic Authentication
curl -u bob:qwerty http://192.165.34.3/dir/

# HTTP Digest Authentication
curl --digest -u alice:password1 http://192.165.34.3/poc/

# HTTP Token Authentication
# -H, --header <header/@file> Pass custom header(s) to server
curl -H 'Authorization: Token 123123123' 192.186.248.3
```
## Fetch Headers
```sh
# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
```
