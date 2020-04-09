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

# Check if Apache httpd is running
systemctl status httpd

# It wasn't so I enabled and started
systemctl enable httpd
systemctl start httpd

# Install the necessary packages
# https://cwiki.apache.org/confluence/display/HTTPD/PasswordBasicAuth
# https://www.server-world.info/en/note?os=CentOS_7&p=httpd&f=10
yum --enablerepo=epel -y install mod_authnz_external pwauth

# Update the appropriate configuration file.
vi /etc/httpd/conf.d/authnz_external.conf

<Directory /var/www/html/protected>
    SSLRequireSSL
    AuthType Basic
    AuthName "PAM Authentication"
    AuthBasicProvider external
    AuthExternal pwauth
    require valid-user
</Directory>

# Check if httpd is working
curl localhost:8080 # ok
curl localhost:8080/protected # ok

# Check that my config changes haven't broken anything
systemctl reload httpd

# Restart the service because my config changes are okay
systemctl restart httpd

# Check my config changes are applied.
curl localhost:8080 # ok
curl localhost:8080/protected # 401
```
