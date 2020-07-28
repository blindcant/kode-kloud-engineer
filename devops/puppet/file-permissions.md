The Nautilus DevOps team has put data on all app servers in Stratos DC. jump host is configured as Puppet master server, and all app servers are already been configured as Puppet agent nodes. The team needs to update content of some of the exiting files as well as update its permissions, etc. Please find below more details about the task:


Create a Puppet programming file official.pp under /etc/puppetlabs/code/environments/production/manifests directory on master node i.e Jump Server. Using puppet file resource, perform the below mentioned tasks.

File official.txt already exists under /opt/dba directory on App Server 2.

1. Add content Welcome to xFusionCorp Industries! in file official.txt on App Server 2.Set permissions 0655 for file official.txt on App Server 2.

Note: Please perform this task using official.pp only, do not create any separate inventory file.

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Create the expected file
cat > /etc/puppetlabs/code/environments/production/manifests/official.pp
```

* https://coderwall.com/p/yfnesa/create-a-symlink-in-puppet
* https://puppet.com/docs/puppet/latest/types/file.html

```
# /etc/puppetlabs/code/environments/production/manifests/official.pp contents added/updated
file { '/opt/dba/official.txt':
  content=> 'Welcome to xFusionCorp Industries!',
  mode=> '0655'
}
```

```bash
# Connect to application servers which are the puppet agents
ssh steve@stapp02

# Switch to root
sudo -s

# Get configuration changes
puppet agent -t


# Check the file exists through the symlink
ll /opt/dba/official.txt # Ok, 0655
cat /opt/dba/official.txt # Ok, contents added
```
