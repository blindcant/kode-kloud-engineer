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

# Check if Apache httpd is running, was stopped on stapp01 but OK on stapp02 and stapp03
systemctl status httpd

# It wasn't so I enabled and started
systemctl enable httpd
systemctl start httpd

# Failed, so I checked the logs
journalctl -xe

# The problem was the port was already being used, so find the culprit
ss -lntp

# The problem was sendmail listening on 8085
systemctl stop sendmail

# Retart httpd and check its ok
systemctl start httpd
curl localhost:8085 # default httpd page, OK!

# Update sendmail config, port 8085 to default port 25
vi /etc/mail/sendmail.mc

# Check its okay
systemctl start sendmail
systemctl status sendmail
```
