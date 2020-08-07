The Nautilus DevOps team to would like to set up a Puppet agent mode to manage their infrastructure in Stratos DC. For testing they are trying to install and set up Puppet agent package on App Server 2. Please find below more details about the task to proceed further.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Install puppet repo - https://puppet.com/docs/puppet/6.17/install_puppet.html#enable_the_puppet_platform_repository
rpm -U https://yum.puppet.com/puppet6-release-el-7.noarch.rpm

# Install puppet agent - https://puppet.com/docs/puppet/6.17/install_agents.html
yum install puppet-agent

# Enable service
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true
systecmctl status puppet # Ok
```
