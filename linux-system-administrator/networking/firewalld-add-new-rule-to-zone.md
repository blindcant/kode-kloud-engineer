Nautilus system admins team just deployed a web UI application for their backup utility running on Nautilus backup server in Stratos Datacenter. The application is running on port 8082 . They have firewalld installed on that server. Some requirements have came up as mentioned below:


Open all incoming connection on 8082/tcp port. Zone should be public.

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
ss -lntp # should see httpd:8082

# Check firewalld status - https://fedoramagazine.org/control-the-firewall-at-the-command-line/
# https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7
systemctl status firewalld # should see active
firewall-cmd --state # should see running

# Check firewall zones
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --get-zones

# Add a new port/protocol permanently
firewall-cmd --add-port=8082/tcp --permanent

# Get network interface name
ip -c -h a

# Set default zone to public for the network interface
firewall-cmd --change-interface=eth0 --zone=public
firewall-cmd --get-default-zone # should see public
firewall-cmd --get-active-zones #	should see public and eth0

# Reload service
systemctl reload firewalld
firewall-cmd --list-ports # should see 8082/tcp

# Check connection internally
curl localhost:8082 # See httpd default page

# Check connection externally - exit out to thor
curl stbkp01:8082 #	See httpd default page
```
