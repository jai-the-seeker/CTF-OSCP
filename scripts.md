# Scripts
# Basic Commands
Refs :
* <https://www.youtube.com/watch?v=LTuuMtQR1uQ&list=PLBf0hzazHTGMJzHon4YXGscxUvsFpxrZT
>
## If-else
```bash
#!/bin/bash

echo "Enter your username : "; read NAME

# format 1
# if test-result-is-true; then COMMAND; fi
if [ "$NAME" = "Eliot" ]; then echo "Welcome back Eliot"; else echo "Please register"; fi

function test_func() {
  return 0
}

# format 2
# if function-return-zero; then COMMAND; fi
if test_func
then
  echo "Function returned zero"
else
  echo "Non zero return!!!"
fi
```
output
```sh
$ ./testscript.sh 
Enter your username : 
Eliot
Welcome back Eliot
Function returned zero
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
  
if [ -e /etc/shadow ]
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
#!/bin/bash

echo "Please enter the subnet : "
read SUBNET

for IP in $(seq 1 254); do
  ping -c 1 $SUBNET.$IP
done
```
output
```sh
$ ./testscript.sh
Please enter the subnet : 
192.234.246
PING 192.234.246.1 (192.234.246.1) 56(84) bytes of data.
64 bytes from 192.234.246.1: icmp_seq=1 ttl=64 time=0.079 ms

--- 192.234.246.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.079/0.079/0.079/0.000 ms
PING 192.234.246.2 (192.234.246.2) 56(84) bytes of data.
64 bytes from 192.234.246.2: icmp_seq=1 ttl=64 time=0.022 ms
```
## Read file line by line
Refs:
* <https://www.cyberciti.biz/faq/unix-howto-read-line-by-line-from-file/>

`while read -r line; do COMMAND; done < input.file`

The `-r` option passed to read command prevents backslash escapes from being interpreted.
Add `IFS=` option before read command to prevent leading/trailing whitespace from being trimmed.

`while IFS= read -r line; do COMMAND $line; done < input.file`
```bash
#!/bin/bash
input="/path/to/txt/file"
while IFS= read -r line
do
  echo "$line"
done < "$input"
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

