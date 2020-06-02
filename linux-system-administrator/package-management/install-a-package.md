As per new application requirements shared by the Nautilus project development team there are some new packages need to be installed on all app servers in Stratos Datacenter. Most of them are done except samba.


So Install the samba package on all app-servers.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create .ssh folder and keys
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Install cups
yum install -y samba
```
