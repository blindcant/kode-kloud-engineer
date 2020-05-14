On our Storage server in Stratos Datacenter we are having some issues where nfsuser user is holding hundred of processes which is degrading the performance of server. So we have a requirement to limit its maximum processes. Please set its maximum process limits as below:


a. soft limit = 72

b. hard_limit = 99

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create .ssh folder and keys
ssh natasha@ststor01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Check currents limits - https://serverfault.com/questions/637212/increasing-ulimit-on-centos
# https://www.tecmint.com/set-limits-on-user-processes-using-ulimit-in-linux/
cat /etc/sysctl.conf
cat /etc/security/limits.conf
ll /etc/security/limits.d
cat /etc/security/limits.d/20-nproc.conf

# Update the limits
vi /etc/security/limits.d/20-nproc.conf
# nfsuser soft  nproc 72
# nfsuser hard  nproc 99

# Reload config file
sysctl -p
```
