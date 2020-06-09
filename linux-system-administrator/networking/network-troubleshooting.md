Our monitoring tool has reported an issue in Stratos Datacenter where one of our app server has some issue as its Apache service is not reachable on port 8084 (which is our Apache port). Either service it self is down or some firewall or something is causing the issues.


Try to test yourself using tools like telnet and netstat etc and fix the issue once identified. Also make sure Apache is reachable from jump host without compromising any security settings.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox
curl @stapp01:8084 # broken
curl @stapp02:8084 # Ok
curl @stapp03:8084 # Ok

# Connect to stapp01 server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Get root access
sudo -s

# Check listening services
ss -lntp # No httpd but sendmail is listening on 8084

# Check service
systemctl status httpd # Not running

# Try to start the service
systemctl start httpd # Fails

# Check error
journalctl -xe # Port already in use

# Fix port problem
systemctl stop sendmail
systemctl start httpd

# Check service
systemctl status httpd # Running
ss -lntp # httpd listening on port 8084

# Test connection locally
curl localhost:8084 # Ok

# Test from thor
curl stapp01:8084 # Fails, unreachable

# Check firewall
systemctl status firewalld # Doesn't exist
systemctl status iptables # Running

# Check firewall rules
iptables -L # Can't see anything why its being blocked

# Turn off firewall and test from thor
systemctl stop iptables
curl stapp01:8084 # Works, firewall problem

# Add a specific rule for httpd
iptables --insert INPUT 1 --protocol tcp --dport 8084 --jump ACCEPT
systemctl start iptables

# Test changes from thor
curl stapp01:8084 # Ok, we are done.
```
