As per details shared by Nautilus application development team. they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. Application uses PostgreSQL database. So as a pre-requisite we need to setup PostgreSQL database server as per requirements shared below:


a. Install and configure PostgreSQL database on Nautilus database server.

b. Create a database user kodekloud_sam and set its password to BruCStnMT5.

c. Create a database kodekloud_db5 and grant full permissions to user kodekloud_sam on this database.

d. Make appropriate settings to allow all local clients (local socket connections) to connect to the kodekloud_db5 database through kodekloud_sam user using md5 encrypted password for authentication.

e. At the end its good to test the db connection using these new credentials from root user or server's sudo user.

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
yum install postgresql-server -y

# https://www.postgresql.org/docs/9.2/creating-cluster.html
sudo -iu postgres
pg_ctl initdb

# https://www.postgresql.org/docs/9.2/server-start.html
pg_ctl -D /var/lib/pgsql/data -l postgres_log start

# Connect to the database
psql

# https://www.postgresql.org/docs/9.2/sql-createrole.html
CREATE USER kodekloud_sam WITH PASSWORD 'BruCStnMT5';

# https://www.postgresql.org/docs/9.2/sql-createdatabase.html
CREATE DATABASE kodekloud_db5 OWNER kodekloud_sam;

# Update the authentication settings - https://www.postgresql.org/docs/9.2/auth-pg-hba-conf.html
cd /var/lib/pgsql/data
vi pg_hba.conf
```
```
# TYPE	DATABASE	USER	ADDRESS	METHOD	
local 	kodekloud_db5	kodekloud_sam		md5
#local   all           all             trust
```

```bash
# Reload the service - https://www.postgresql.org/docs/9.2/app-pg-ctl.html
pg_ctl reload -D /var/lib/pgsql/data -l postgres_log

# Check the connection as root user
psql # fail
psql -d kodekloud_db5 # fail
psql -U kodekloud_sam # fail
psql -d kodekloud_db5 -U kodekloud_sam # success with correct password
```
