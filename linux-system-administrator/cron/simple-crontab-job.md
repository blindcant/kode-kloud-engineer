Nautilus system admins team has prepared some scripts to automate few day to day tasks. They want them to be deployed on all app servers in Stratos DC and need to set some scheduled run for all of them. Before that they need to test similar functionality with a sample cron job. So perform the steps given below:


a. Install cronie package on all Nautilus app servers and start crond service.

b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.

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

# Install and enable cron
yum install -y cronie
systemctl start crond

# Check crontab entries
crontab -l

# Edit crontab entries
crontab -e
```

```bash
# For details see man 4 crontabs
# Example of job definition:
# .---------------- minute (0 - 59)
# | .------------- hour (0 - 23)
# | | .---------- day of month (1 - 31)
# | | | .------- month (1 - 12) OR jan,feb,mar,apr ...
# | | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# | | | | |
# * * * * * command to be executed
# https://crontab.guru/every-5-minutes
*/5 * * * * echo hello > /tmp/cron_text
```
