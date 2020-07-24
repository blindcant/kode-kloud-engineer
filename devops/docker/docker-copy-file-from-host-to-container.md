Copy /tmp/nautilus.txt.gpg to ubuntu_latest container folder /tmp


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh banner@stapp03

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Copy file
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp

# Check that the file copeid
docker ubuntu_latest ls /tmp # Ok
```