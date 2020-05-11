xFusionCorp Industries planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress but we want to get the servers ready. The storage server has a shared directory /data that is mounted on each app host under /var/www/html directory. Please perform below given steps to accomplish the task:


a. Install httpd package and dependencies on all app hosts.

b. Apache should serve on port 8080 within the apps.

c. There are two website's backups /home/thor/media and /home/thor/news on jump_host. Setup them on Apache in a way that media should work on link http://<<lb-url>>/media/ and news should work on link http://<<lb-url>>/news. (do not worry about load balancer configuration, its already configured)

d. You can access the website on LBR link, to do so click on the + button on top of your terminal and select option Select port to view on Host 1 and after adding port 80 click on Display Port.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Create ~/.ssh folder
mkdir -p ~/.ssh
cd ~/.ssh

# Generate ssh keys - https://www.ssh.com/ssh/keygen#creating-an-ssh-key-pair-for-user-authentication
ssh-keygen -t ed25519

# Copy ssh private key to required servers - https://www.ssh.com/ssh/keygen#copying-the-public-key-to-the-server
# Application server logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh-copy-id -i ~/.ssh/id_ed25519 tony@stapp01
ssh-copy-id -i ~/.ssh/id_ed25519 steve@stapp02
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03
ssh-copy-id -i ~/.ssh/id_ed25519 natasha@ststor01

# Copy over files from thor to storage
scp /home/thor/media natasha@ststor01:/home/natasha
scp /home/thor/news natasha@ststor01:/home/natasha
ssh natasha@ststor01

# Switch to root
sudo -s
cp ~nastaha/media /data
cp ~nastaha/news /data
chown -R $USER: /data

# Configure app servers from thor
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03


# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Check if Apache httpd is running, it was off on all servers
ss -lntp
systemctl status httpd

# Install httpd
yum install -y httpd

# Confiugre apache
vi etc/httpd/conf/httpd.conf
# Listen 8080

# Start and check httpd is listening on 8080
systemctl start httpd
ss -lntp

# Test websites from app servers
curl @localhost:8080/news
curl @localhost:8080/media

# Test websites from thor
curl @stapp01:8080/news
curl @stapp01:8080/media
curl @stapp02:8080/news
curl @stapp02:8080/media
curl @stapp03:8080/news
curl @stapp03:8080/media
```