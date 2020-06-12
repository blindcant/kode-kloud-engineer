As per details shared by development team the new application release has some dependencies on back end. There are some packages/services need to be installed on all app servers under Stratos Datacenter. As per requirements please perform below given steps:


a. Install cups package on all the application servers.

b. Once installed, make sure it is enabled to start during boot.


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
yum install -y cups

# Enable the service for automatic start at boot time
systemctl enable cups

# Start the service now
systemctl start cups

# View the service status
systemctl status cups # Enabled and running
```

As per details shared by development team the new application release has some dependencies on back end. There are some packages/services need to be installed on all app servers under Stratos Datacenter. As per requirements please perform below given steps:


a. Install squid package on all the application servers.

b. Once installed, make sure it is enabled to start during boot.

```bash
    1  sudo yum -y install squid
    2  systemctl enable squid
    3  systemctl start squid
```