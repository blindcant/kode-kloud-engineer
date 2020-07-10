There are directory structures in the Stratos Datacenter that need to be changed, including directories that need to be linked to the default Apache document root. We need to accomplish this task using only Puppet as per the instructions given below:

Create a puppet programming file ecommerce.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server. Within that define a class symlink and perform below mentioned tasks:

1. Create a symbolic link through puppet programming code. The source path should be /opt/dba and destination path should be /var/www/html on all Puppet agents i.e on all App Servers.

1. Create a blank file story.txt under /opt/dba directory on all puppet agent nodes i.e on all App Servers.

Note: Please perform this task using ecommerce.pp only, do not create any separate inventory file.

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Create the expected file
cat > /etc/puppetlabs/code/environments/production/manifests/ecommerce.pp
```

* https://coderwall.com/p/yfnesa/create-a-symlink-in-puppet
* https://puppet.com/docs/puppet/latest/types/file.html

```
# /etc/puppetlabs/code/environments/production/manifests/ecommerce.pp contents added/updated
class symlink {
  file { '/opt/dba':
    ensure=> 'link',
    target=> "/var/www/html"
  }
}
include symlink
```

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Switch to root
sudo -s

# Get configuration changes
puppet agent -t

# Create a test file
touch /opt/dba/story.txt

# Check the file exists through the symlink
ll /var/www/html
```
