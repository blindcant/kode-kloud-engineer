The document root /var/www/html of all web apps is on NFS share /data on storage server in Stratos Datacenter. We have a requirement where we want to password protect a directory in Apache web server document root. We want to password protect http://<website-url>:<apache_port>/protected URL as per below given requirements (you can use any website-url for it like localhost since there are no such specific requirements as of now):

a. We want to use basic authentication.

b. We do not want to use htpasswd file base authentication instead we want to use PAM authentication i.e Basic Auth + PAM. So that we can authenticate with a Linux user itself.

c. We already have a user ammar with password 8FmzjvFU6S which you need to provide access.

d. You can access the website on LBR link, to do so click on the + button on top of your terminal and select option Select port to view on Host 1 and after adding port 80 click on Display Port.

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

## Check what port httpd is running on
ss -lntp # Was 8080

# Install the necessary packages
# https://cwiki.apache.org/confluence/display/HTTPD/PasswordBasicAuth
# https://www.server-world.info/en/note?os=CentOS_7&p=httpd&f=10
yum --enablerepo=epel -y install mod_authnz_external pwauth

# Update the appropriate configuration file.
vi /etc/httpd/conf.d/authnz_external.conf

<Directory /var/www/html/protected>
    #SSLRequireSSL # Commented out otherwise we need to install mod_ssl
    AuthType Basic
    AuthName "PAM Authentication"
    AuthBasicProvider external
    AuthExternal pwauth
    require valid-user
</Directory>

# Check if httpd is working
curl localhost:8080 # Ok
curl localhost:8080/protected # Ok with 301
curl localhost:8080/protected/index.html # Ok

# Check that my config changes haven't broken anything
systemctl reload httpd # Had an error because of mod_ssl missing, commented out #SSLRequireSSL

# Restart the service because my config changes are okay
systemctl restart httpd

# Check my config changes are applied.
curl localhost:8080 # Ok
curl localhost:8080/protected # 401
curl localhost:8080/protected/index.html # 401
curl --basic --user anita:8FmzjvFU6S localhost:8080/protected # Ok
curl --basic --user anita:8FmzjvFU6S localhost:8080/protected/index.html # Ok
```
