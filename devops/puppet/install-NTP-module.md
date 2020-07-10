While troubleshooting one of the issue on app servers in Stratos Datacenter DevOps team identified the root cause that the time isn't synchronized properly among all app servers which cause issues sometimes. So team has decided to use a specific time server for all app servers so that they all remain in sync. This task needs to be done using Puppet so as per details mentioned below please compete the task:

1. Create a puppet programming file cluster.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server. Within the programming file define a custom class ntpconfig to install and configure ntp server on all app servers.

1. Also add NTP Server server 2.north-america.pool.ntp.org in default configuration file on all app servers.

1. Please note that do not try to start/restart/stop ntp service as we already have a scheduled restart for this service tonight and we don't want these changes to be applied right now.

Note: Please perform this task using cluster.pp only, do not try to create any separate inventory file.

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Make sure root
sudo -s

# Install the NTP module - https://puppet.com/docs/puppet/latest/quick_start_ntp.html
puppet module install puppetlabs-ntp

# Create necessary file - https://puppet.com/docs/puppet/latest/quick_start_ntp.html
# Create custom class - https://fullstack-puppet-docs.readthedocs.io/en/latest/puppet_modules.html
cat > /etc/puppetlabs/code/environments/production/manifests/cluster.pp
```

```
# Contents added/updated to /etc/puppetlabs/code/environments/production/manifests/cluster.pp
class ntpconfig {
        package { 'ntp':
                ensure=> 'present'
        }

        file { '/etc/ntp.conf':
                ensure=> 'present',
                content=> "server 2.north-america.pool.ntp.org\n"
  }
}
include ntpconfig
```

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Get root
sudo -s

# Get configuration changes
puppet agent -t

# Check file and service
cat /etc/ntp.conf
puppet resource service ntpd
```
