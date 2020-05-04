To secure our Nautilus infrastructure in Stratos Datacenter we have decided to install and configure firewalld on all app servers. We have Apache and Nginx services running on these apps. Nginx is running as a reverse proxy server for Apache. We might have more robust firewall settings in future but for now we have decided to go with below given requirements:

a. Allow all incoming connections on Nginx port.

b. Allow incoming connections from LB host only on Apache port and block for rest.

c. All rules must be permanent.

d. Zone should be public.

e. If Apache or Nginx services aren't running already please make sure to start them.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to app servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh-keygen -t ed25519
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
ssh-copy-id loki@stlb01

# Run these command on all servers
tony@stapp01
steve@stapp02
banner@stapp03

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Check services
systemctl status httpd # OK
systemctl status nginx # OK
systemctl status firewalld # Not installed

# Install firewalld
yum install firewalld -y
systemctl enable firewalld
systemctl start firewalld 

# On stapp03 this failed with error Exception DBusException: org.freedesktop.DBus.Error.AccessDenied: Connection ":1.4" is not allowed to own the service "org.fedoraproject.FirewallD1" due to security policies in the configuration file
systemctl restart dbus
systemctl restart firewalld 

# Check currently listening applications
ss -lntp # httpd = 8085 & nginx = 8096

# https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7
systemctl status firewalld # Should see active
firewall-cmd --state # Should see running

# Check firewall zones
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --get-zones

# Get network interface name
ip -c -h a

# Set default zone to public for the network interface
firewall-cmd --permanent --change-interface=eth0 --zone=public
systemctl restart firewalld
firewall-cmd --get-default-zone # Should see public
firewall-cmd --get-active-zones # Should see public and eth0

# Add a new port/protocol permanently
firewall-cmd --permanent --zone=public --add-port=8098/tcp 
systemctl restart firewalld
firewall-cmd --list-ports # should see 8098/tcp

# Add a rich rule - https://serverfault.com/a/684603
firewall-cmd --permanent --zone=public --add-rich-rule='
  rule family="ipv4"
  source address="172.16.238.14/16"
  port protocol="tcp"
	port="8085"
	accept'

firewall-cmd --permanent --zone=public --add-rich-rule='
  rule family="ipv4"
  source address="0.0.0.0"
  port protocol="tcp"
	port="8085"
	reject'

firewall-cmd --list-all # should see both rich rules for 8085

# Check connection internally
curl localhost:8085 # OK, default httpd
curl localhost:8096 # OK, nginx 403 but this is right

# Check connection externally - exit out to thor
curl stapp01:8085 # Blocked, no route to host
curl stapp02:8085 # Blocked, no route to host
curl stapp03:8085 # Blocked, no route to host
	
curl stapp01:8096 # OK, nginx 403 but this is right
curl stapp02:8096 # OK, nginx 403 but this is right
curl stapp03:8096 # OK, nginx 403 but this is right

# Check connection externally
ssh loki@stlb01

curl stapp01:3002 # OK, default httpd page
curl stapp02:3002 # OK, default httpd page
curl stapp03:3002 # OK, default httpd page
	
curl stapp01:8098 # OK, nginx 403
curl stapp02:8098 # OK, nginx 403
curl stapp03:8098 # OK, nginx 403
```