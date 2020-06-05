System admins team of xFusionCorp Industries has setup a new tool on all app servers, they have a requirement to create a service user account that will be used by that tool. They are done with all apps except App 2 in Stratos Datacente.


Create a user named john in App Server 2 without a home directory.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Create the user without an interactive shell
useradd --no-create-home john

# Check our work
fgrep 'john' /etc/passwd # Should exist
ll /home # No siva folder here
```