```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to backup server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh clint@stbkp01

# iptables overview https://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/
# https://www.digitalocean.com/community/tutorials/how-the-iptables-firewall-works

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Check if firewalld is running, and it wasn't.
systemctl status firewalld

# Check if iptables is running, it was but inactive
systemctl status iptables

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
# Accept incoming/outgoing TCP connections from/to 0.0.0.0:8098
# Insert at postion 1 means the first rule AFTER the policy rules.
iptables --insert INPUT 1 --protocol tcp --dport 8098 --jump ACCEPT
# Reject incoming TCP connections from/to 0.0.0.0:8089
# DROP does it silently, REJECT lets them know the connection was refused.
iptables -I INPUT 2 -p tcp --dport 8089 -j REJECT

# Save the updated rules - https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands
service iptables save

# Reload the service
systemctl reload iptables
service iptables reload

# Check connection internally
curl stbkp01:8089
	Connection refused.
curl stbkp01:8098
	See nginx default page

# Check connection externally - exit out to thor
curl stbkp01:8089
	Connection refused.
curl stbkp01:8098
	See nginx default page
```
