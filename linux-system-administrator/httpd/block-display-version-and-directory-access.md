During recent security audit, application security team of xFusionCorp Industries found some security issues with Apache web server on Nautilus App Server 1 server in Stratos DC. They have listed out some security issues that need to be fixed on this server. Please apply the below mentioned security settings:

a. On Nautilus App Server 1 it was identified that Apache web server is exposing the version number. Make appropriate settings on this server to hide the version number of Apache web server.

b. There is a website hosted under /var/www/html/ecommerce on App Server 1. It was detected that the directory /ecommerce is listing all of its contents while browsing the URL. Disable directory browser listing in Apache config.

c. Also make sure to restart Apache service after making the changes.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Check if Apache httpd is running, was stopped on stapp01 but OK on stapp02 and stapp03
systemctl status httpd

# It wasn't so I enabled and started
systemctl enable httpd

# Turn off version numbers - https://www.tecmint.com/hide-apache-web-server-version-information/
vi /etc/httpd/conf/httpd/conf
ServerTokens Prod
ServerSignature Off

# Reload, restart, and test
systemctl reload httpd
systemctl restart httpd
curl -v localhost:8080

# Access folder we shouldn't be able to
curl -v localhost:8080/ecommerce/ # Can see

# Block access - https://www.tecmint.com/disable-apache-directory-listing-htaccess/
vi /etc/httpd/conf/httpd/conf

<Directory /var/www/html/>
  AllowOverride All # Changed this from None to All

vi /var/www/html/ecommerce/.htaccess
Options -Indexes

# Reload, restart, and test
systemctl reload httpd
systemctl restart httpd
curl -v localhost:8080/ecommerce/ # Can't see
```
