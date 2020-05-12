There are some specific access levels for users defined by system admins team of xFusionCorp Industries. Rather than providing some access levels to every user individually team has decided to create groups with required access levels and add users to that groups as needed. So there is a requirement as below:

a. Create a group named nautilus_sftp_users in all App servers in Stratos Datacenter.

b. Add the user rajesh to nautilus_sftp_users in all App servers. (create the user if not present already)

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
cat /etc/passwd | grep rajesh

# Create the new user
useradd rajesh

# Check the user creation
cat /etc/passwd | grep rajesh

# Add new user to group - https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/
usermod -aG nautilus_developers rajesh

# Check user is in group
cat /etc/passwd | grep rajesh
```
