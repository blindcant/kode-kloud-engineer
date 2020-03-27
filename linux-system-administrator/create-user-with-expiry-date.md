```bash
# ssh from thor jumpbox as the correct user
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
