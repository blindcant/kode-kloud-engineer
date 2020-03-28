```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0
ssh banner@stapp03

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
