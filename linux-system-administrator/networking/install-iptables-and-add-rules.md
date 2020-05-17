We have one of our website up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 3000 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with below given requirements:


Install iptables and all its dependencies on each app host.

Block incoming port 3000 on all apps for everyone except for LBR host.

Make sure the rules should persist even after system reboot.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to app servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# iptables overview https://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/
# https://www.digitalocean.com/community/tutorials/how-the-iptables-firewall-works

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Check if firewalld is running, and it wasn't.
systemctl status firewalld

# Check if iptables is running, it wasn't installed
systemctl status iptables

# Install iptables
yum install iptables-services -y

# Automatically start iptables at boot time and start it right now
systemctl enable iptables
systemctl start iptables

# Check out network interfaces, eth0 is the one being used.
ip -c -h a

# Check if the httpd and nginx services are running and listening. They were, on 8089 and 8098 respectively
ss -lntp

# Check the default policy chain behaviour, standard is to ACCEPT  - https://www.digitalocean.com/community/tutorials/how-to-list-and-delete-iptables-firewall-rules
iptables -S # specificaiton list
iptables -L # table list

# Add new rules
# Accept incoming TCP connections from 172.16.238.14:3000
# Insert at postion 1 means the first rule AFTER the policy rules.
# https://serverfault.com/questions/163111/allow-traffic-to-from-specific-ip-with-iptables
iptables --insert INPUT 1 --protocol tcp --source 172.16.238.14 --dport 3000 --jump ACCEPT

# Reject incoming TCP connections from/to 0.0.0.0:8089
# DROP does it silently, REJECT lets them know the connection was refused.
iptables -I INPUT 2 -p tcp --dport 3000 -j REJECT

# Save the updated rules - https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands
service iptables save

# Reload the service
systemctl reload iptables
service iptables reload

# Check connection externally from thor
curl stapp01:3000 # Connection refused, good.
curl stapp02:3000 # Connection refused, good.
curl stapp03:3000 # Connection refused, good.

# Check connection externally from stlb01
ssh loki@stlb01
curl stapp01:3000 # Ok, default httpd page.
curl stapp02:3000 # Ok, default httpd page.
curl stapp03:3000 # Ok, default httpd page.
```
