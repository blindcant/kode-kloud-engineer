For some security reasons xFusionCorp Industries security team has decided to use custom Apache users for each web application hosted there rather than its default user. Since this is going to be the Apache user so it shouldn't use the default home directory. Create the user as per requirements given below:

a. Create a user named anita on the App server 1 in Stratos Datacenter.

b. Set UID to 1675 and its home directory to /var/www/anita

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp1

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Create /var/www as it didn't exist
mkdir -p /var/www
chmod 755 /var/www

# Create the user
useradd --home-dir /var/www/anita --uid 1675 anita

# Check
cat /etc/passwd
```