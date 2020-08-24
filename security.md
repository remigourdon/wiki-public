# Security

## Minimal

```sh
# Change root password
passwd

# Add non root user
adduser <login>           
usermod -a -G sudo <login>

# Prevent root login on the server
sed -i /etc/ssh/sshd_config -e "s/^PermitRootLogin .*$/PermitRootLogin no/"
service ssh restart

# Add local SSH key to the remote server
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote
```
