```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh peter@stdb01

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Change to root
sudo -s

# CentOS 7 has 9.2.24 - https://www.postgresql.org/docs/9.2/
yum install postgres-server

# https://www.postgresql.org/docs/9.2/creating-cluster.html
sudo -iu postgres
pg_ctl initdb

# https://www.postgresql.org/docs/9.2/server-start.html
pg_ctl -D /var/lib/pgsql/data -l postgres_log start

# Connect to the database
psql

# https://www.postgresql.org/docs/9.2/sql-createrole.html
CREATE USER kodekloud_roy WITH PASSWORD 'LQfKeWWxWD';

# https://www.postgresql.org/docs/9.2/sql-createdatabase.html
CREATE DATABASE kodekloud_roy OWNER kodekloud_roy;

# Update the authentication settings - https://www.postgresql.org/docs/9.2/auth-pg-hba-conf.html
cd /var/lib/pgsql/data
vi pg_hba.conf

# TYPE	DATABASE	USER	ADDRESS	METHOD	
local 	kodekloud_db6	kodekloud_roy		md5

# Check the connection
psql # fail
psql -d kodekloud_db6 # fail
psql -U kodekloud_roy # fail
psql -d kodekloud_db6 # fail
psql -d kodekloud_db6 -U kodekloud_roy # success with correct password
```
