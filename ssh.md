Ref
* <https://www.youtube.com/watch?v=hX4eWqAKQpE>

# Enable `SSH` on Kali Linux

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
Now, `start` the service
```
service ssh start
service ssh status
```
