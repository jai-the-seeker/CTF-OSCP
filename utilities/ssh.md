* [`SSH` Server on Kali Linux](#ssh-server-on-kali-linux)
  * [Install `SSH` server](#install-ssh-server)
  * [Enable root account access from `ssh`](#enable-root-account-access-from-ssh)

# `SSH` Server on Kali Linux
Ref
* <https://www.youtube.com/watch?v=hX4eWqAKQpE>

## Install `SSH` server
This step is usually not required in Kali Linux as it comes preinstalled with `ssh` server. However, if its not installed, you can use
```
sudo apt-get install openssh-server
```
## Enable root account access from `ssh`
By default Kali does not allow root account access from `ssh`. You need to make following changes in the config file.
```
nano /etc/ssh/sshd_config
```
Change following in the config file
```
PermitRootLogin yes
```
Now, `start` the service and check its status
```
service ssh start
service ssh status
```
