System admins team of xFusionCorp Industries has noticed some issues with DNS resolution in some apps intermittently. This time on App Server 2 in Stratos Datacenter is having some DNS resolution issues. So we want to add some additional DNS nameservers on this server.


As a temporary fix we have decided to go with Google public DNS. Please make appropriate changes on this server.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# See current DNS
cat /etc/resolv.conf

# Switch to root
sudo -s

# Add DNS - https://www.cyberciti.biz/faq/change-dns-ip-address-rhel-redhat-linux/
vi /etc/resolv.conf
	nameserver 8.8.8.8
	nameserver 8.8.4.4
	#nameserver 127.0.0.11

# Check it was changed
ping -c 3 google.com
```
