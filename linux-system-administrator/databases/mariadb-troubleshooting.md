There is a critical issue going on with Nautilus application in Stratos DC. Production support team has identified that application is not able to connect to database. After digging into the issue team found that mariadb service is down on database server.


Look into the issue and fix the same.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to database server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh peter@stdb01

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Change to root
sudo -s

# Check the service
systemctl status mariadb # inactive

# Try to start the service
systemctl start mariadb
systemctl status mariadb # Error

# Check logs - found [ERROR] /usr/libexec/mysqld: Can't create/write to file '/var/run/mysqld/mysqld.pid' (Errcode: 13)
journalctl -xe
less /var/log/mariadb/mariadb.log # Error found in here

# https://stackoverflow.com/questions/15408643/cant-connect-to-mysql-server-cant-create-write-the-pid-file
chown mysql: /var/run/mariadb
systemctl start mariadb
systemctl status mariadb # OK
```