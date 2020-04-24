# Scripts
# Basic Commands
Refs :
* <https://www.youtube.com/watch?v=LTuuMtQR1uQ&list=PLBf0hzazHTGMJzHon4YXGscxUvsFpxrZT
>
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
## Conditional Expressions
We can make use of conditional expressions in the bash scripting. In order to know the available expressions use
```sh
$ help test
test: test [expr]
    Evaluate conditional expression.
    
    Exits with a status of 0 (true) or 1 (false) depending on
    the evaluation of EXPR.
    
    -d FILE        True if file is a directory.
    -e FILE        True if file exists.
```
### Use of conditional expression in if-else
```bash
#!/bin/bash
  
if [ -e /etc/shadow ];
then
  echo "File exists"
else
  echo "Missing..."
fi
```
## for loops
```bash
#!/bin/bash

for NAME in $(cat names.txt); do
  echo "Name is : $NAME"
done
```
output
```sh
$ ./testscript.sh 
Name is : ram
Name is : shayam
Name is : sita
```

```bash
```
output
```sh
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

