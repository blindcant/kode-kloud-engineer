```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to storage server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh natasha@ststor01

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Change to /data and make the backup
cd /data
tar zcvf javed.tar.gz javed/

# Move the backup to /home
mv javed.tar.gz /home

# Check it was moved
ls -ahl --color /home
