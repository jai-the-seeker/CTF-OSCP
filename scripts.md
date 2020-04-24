# Scripts
# Basic Commands
Refs :
* <https://www.youtube.com/watch?v=LTuuMtQR1uQ&list=PLBf0hzazHTGMJzHon4YXGscxUvsFpxrZT
>
## Read input from user prompt
```bash
#!/bin/bash

read -p "Enter your Name : " NAME
echo "Your name is $NAME"
```
output
```sh
$ ./testscript.sh 
Enter your Name : tester
Your name is tester
```

## If-else
```bash
#!/bin/bash

echo "Enter your username : "
read NAME

if [ "$NAME" = "Eliot" ];
then
  echo "Welcome back Eliot"
else
  echo "Please register"
fi
```
output
```sh
$ ./testscript.sh 
Enter your username : 
Eliot
Welcome back Eliot
```


# Useful Scripts
## Authorization Token
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

