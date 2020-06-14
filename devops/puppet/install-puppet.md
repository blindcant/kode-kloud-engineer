Nautilus DevOps team started experimenting puppet server to manage some part of their infrastructure in Stratos DC. For testing different scenarios team will be using jump host as puppet master. At this point we just need to install puppet server package and need to make sure its service is up and running. Below you can find more details about the task.


Install puppetserver package on jump host also start its service.

Before starting puppetserver service you might need to change its memory allocation configuration, we recommend to allocate it 512m of memory.

Note: Please make sure to install puppetserver package only not any other alternate package.

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '

# Add the puppetserver repo - https://puppet.com/docs/puppet/latest/puppet_platform.html#enable_the_puppet_platform_yum
sudo rpm -U https://yum.puppet.com/puppet6-release-el-7.noarch.rpm

# Install puppetserver - https://puppet.com/docs/puppetserver/latest/install_from_packages.html#install-puppet-server-from-packages
sudo yum install -y puppetserver

# Change memory allocation - https://puppet.com/docs/puppetserver/latest/install_from_packages.html#memory-allocation
vi /etc/sysconfig/puppetserver
```

```
JAVA_ARGS="-Xms512m -Xmx512m"
```

```bash
# Start the service
sudo systemctl start puppetserver

# Check it is running
systemctl status puppetserver
```