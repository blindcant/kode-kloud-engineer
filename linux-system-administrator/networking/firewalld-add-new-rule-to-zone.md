```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to backup server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh clint@stbkp01

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Check currently listening applications
ss -lntp
	should see httpd:5001

# Check firewalld status - https://fedoramagazine.org/control-the-firewall-at-the-command-line/
# https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7
systemctl status firewalld
	should see active
firewall-cmd --state
	should see running

# Check firewall zones
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --get-zones

# Add a new port/protocol permanently
firewall-cmd --add-port=5001/tcp --permanent
firewall-cmd --list-ports
	should see 5001/tcp

# Get network interface name
ip -c -h a

# Set default zone to public for the network interface
firewall-cmd --change-interface=eth0 --zone=public
firewall-cmd --get-default-zone
	should see public
firewall-cmd --get-active-zones
	should see public and eth0

# Check connection internally
curl stbkp01:5001
	See httpd default page

# Check connection externally - exit out to thor
curl stbkp01:5001
	See httpd default page
```
