Production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One of them is to create a bash script for taking websites backup. They have a static website running on App Server 2 in Stratos Datacenter. They need to create a bash script named official_backup.sh which should accomplish below given tasks. (Also remember to place the script under /scripts directory on App Server 2)

a. Create a zip archive named xfusioncorp_official.zip of /var/www/html/official directory.

b. Save the archive in /backup/ on App Server 2. This is a temporary storage as backups from this location will be clean on weekly basis so we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while coping the archive file also respective server user (for example tony in case of App Server 1) must be able to run it.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

cat > /scripts/official_backup.sh

#!/usr/bin/env bash

echo "# Back Up Script"
rm -rfv /backup/xfusioncorp_official.zip
zip -r /backup/xfusioncorp_official.zip /var/www/html/official
ls -Ahl --color /backup
scp /backup/xfusioncorp_official.zip stpbkp01:/backup/

^D

# Make executable
chmod 755 /scripts/official_backup.sh
/scripts/official_backup.sh # ssh will fail

# Make it runnable by steve
chown steve: /scripts/official_backup.sh

# Set up passwordless ssh between stapp02 and stbkp01
# Create ~/.ssh folder
mkdir -p ~/.ssh
cd ~/.ssh

# Generate ssh keys - https://www.ssh.com/ssh/keygen#creating-an-ssh-key-pair-for-user-authentication
ssh-keygen -f ~/.ssh/id_ed25519 -t ed25519
	No password

# Copy ssh private key to required servers - https://www.ssh.com/ssh/keygen#copying-the-public-key-to-the-server
ssh-copy-id -i ~/.ssh/id_ed25519 clint@stbkp01

# Set up ssh config to we don't need to specify the clint user with stbkp01
cat > ~/.ssh/config

Host stbkp01
	User clint
	IndetityFile ~/.ssh/id_ed25519
	PreferredAuthentications publickey

# Restrict default permissions
chmod 600 ~/.ssh/config

# Run the script as steve
/scripts/official_backup.sh

# Check results
ssh stbkp01
ll /backup # See the zip file here
```