```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
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
