xFusionCorp Industries planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress but we want to get the servers ready. The storage server has a shared directory /data that is mounted on each app host under /var/www/html directory. Please perform below given steps to accomplish the task:


a. Install httpd package and dependencies on all app hosts.

b. Apache should serve on port 8080 within the apps.

c. There are two website's backups /home/thor/official and /home/thor/news on jump_host. Setup them on Apache in a way that official should work on link http://<<lb-url>>/official/ and news should work on link http://<<lb-url>>/news. (do not worry about load balancer configuration, its already configured)

d. You can access the website on LBR link, to do so click on the + button on top of your terminal and select option Select port to view on Host 1 and after adding port 80 click on Display Port.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Generate ssh keys - https://www.ssh.com/ssh/keygen#creating-an-ssh-key-pair-for-user-authentication
# This will create ~/.ssh and ~/.ssh/id_ed25519
ssh-keygen -t ed25519

# Copy ssh private key to required servers - https://www.ssh.com/ssh/keygen#copying-the-public-key-to-the-server
# Application server logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh-copy-id -i ~/.ssh/id_ed25519 tony@stapp01
ssh-copy-id -i ~/.ssh/id_ed25519 steve@stapp02
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03
ssh-copy-id -i ~/.ssh/id_ed25519 

# scp fails on ststor01 because scp isn't installed, so install openssh-clients for scp
ssh natasha@ststor01
yum provides scp
yum install openssh-clients -y

# Copy over files from thor to storage
tar zcvf official.tgz official
tar zcvf news.tgz news
scp /home/thor/official.tgz natasha@ststor01:/home/natasha
scp /home/thor/news.tgz natasha@ststor01:/home/natasha

# Unpack the files to the correct place
ssh natasha@ststor01
sudo tar zxvf official.tgz /data
sudo tar zxvf news.tgz /data

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
curl @localhost:8080/news # 301 redirect because of no /
curl @localhost:8080/official # 301 redirect because of no /
curl @localhost:8080/news/ # 200, OK
curl @localhost:8080/official/ # 200, OK

# Test websites from thor
curl @stapp01:8080/news/ # 200, OK
curl @stapp01:8080/official/ # 200, OK
curl @stapp02:8080/news/ # 200, OK
curl @stapp02:8080/official/ # 200, OK
curl @stapp03:8080/news/ # 200, OK
curl @stapp03:8080/official/ # 200, OK
```