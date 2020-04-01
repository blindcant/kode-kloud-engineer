```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Check if group exists
cat /etc/group | grep naut

# Create the new group - https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/
groupadd nautilus_developers

# Check the group creation
cat /etc/group | grep naut

# Check if user exits
cat /etc/passwd | grep stark

# Create the new user
useradd stark

# Check the user creation
cat /etc/passwd | grep stark

# Add new user to group - https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/
usermod -aG nautilus_developers stark

# Check user is in group
cat /etc/passwd | grep -E 'stark|nautilus'
```
