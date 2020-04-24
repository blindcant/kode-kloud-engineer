```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp2

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Create the user with an expiry date - https://unix.stackexchange.com/questions/80968/how-can-i-create-automatically-expiring-user-accounts
useradd -e 2021-02-17 ravi

# Check expiry time - https://www.cyberciti.biz/faq/linux-howto-check-user-password-expiration-date-and-time/
chage -l ravi
```
