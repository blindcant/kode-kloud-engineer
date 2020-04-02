```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
ssh peter@stdb01

# Check O/S, it was CentOS 7
cat /etc/*release*

# See current
systemctl get-default
	multi-user.target # CLI

# Show all
systemctl list-units --type=target

# Change to GUI
sudo systemctl set-default graphical.target

# Check it was changed
systemctl get-default
	graphical.target # GUI
```