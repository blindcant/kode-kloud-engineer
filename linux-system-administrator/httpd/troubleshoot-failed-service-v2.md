xFusionCorp Industries having some monitoring tools to check the status of every service, applications etc running on the systems. So, the monitoring system acknowledged that Apache service is not running on some of the Nautilus Application Servers in Stratos Datacenter.


Identify the faulty Nautilus Application Servers and fix the issue. And also make sure Apache service is up and running on all Nautilus Application Servers, do not try to stop any kind of firewall if already running.

Apache is running on 8084 port on all Nautilus Application Servers and it document root must be /var/www/html on all app servers.

Finally you can test from jump host using curl command to access Apache on all app servers and it should work fine. e.g curl http://172.16.238.10:8084/

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

# Check if Apache httpd is running, it was off on all servers
ss -lntp
systemctl status httpd

# It wasn't so I enabled and started
systemctl enable httpd
systemctl start httpd

# stapp02 was fine after the service was started
# stapp01 and stapp03 had more problems so I checked the logs
journalctl -xe

# The problem on stapp01 /etc/httpd/conf/httpd.conf had ServerRoot /etc/httpd; instead of /etc/httpd
# The problem on stapp02 /etc/httpd/conf/httpd.conf had Listen "8084" instead of Listen 8084 and DocumentRoot /var/www instead of DocumentRoot "/var/www/html"

# Retart httpd and check its ok
systemctl start httpd
curl localhost:8084 # default httpd page, OK!

# Check from jump box
curl stapp01:8084 # default httpd page, OK!
curl stapp02:8084 # default httpd page, OK!
curl stapp03:8084 # default httpd page, OK!
```