```bash
# ssh from thor jumpbox as the correct user
ssh banner@stapp03

# Check current Linux version, it was RHEL 7
cat /etc/*release*

# Switch to root
sudo -s

# Check syntax for no interactive shell, it was /sbin/nologin
cat /etc/passwd

# Create the user without an interactive shell
useradd -s /sbin/nologin anita
```
