The Nautilus DevOps team was working on developing some custom RPMs to meet some application needs. Most of those RPMs will be installed on all application servers in Stratos Datacenter. So to accomplish this task we need to configure a local yum repository on all app servers so that we can install those custom RPMs from that local yum repository. But this task needs to be done using Puppet, below you can find more details.

Create a puppet programming file demo.pp under /etc/puppetlabs/code/environments/production/manifests directory on master node i.e on Jump Server. Define a class local_yum_repo and perform below mentioned tasks:

There are some RPMs already present at location /packages/downloaded_rpms on all puppet agent nodes i.e on all App Servers.

Create a local yum repository named localyum (make sure to set Repository ID to localyum). Configure it to use packages location /packages/downloaded_rpms on all puppet agent nodes i.e on all App Servers.

Install package vsftpd from this newly created repository through on all puppet agent nodes i.e on all App Servers.

Make sure to create a single puppet programming file demo.pp to configure local yum repository and install the package.

Note: Please perform this task using demo.pp only, do not create any separate inventory file.

# Jump Box

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Make sure root
sudo -s

# Create necessary file - https://puppet.com/docs/puppet/latest/quick_start_ntp.html
# Create custom class - https://fullstack-puppet-docs.readthedocs.io/en/latest/puppet_modules.html
# Create yum repo
# https://forge.puppet.com/puppetlabs/yumrepo_core
# https://www.puppetcookbook.com/posts/add-a-yum-repo-config.html
# Install a package
# https://www.puppetcookbook.com/posts/install-package.html
cat > /etc/puppetlabs/code/environments/production/manifests/demo.pp
```

```
# Contents added/updated to /etc/puppetlabs/code/environments/production/manifests/demo.pp
class local_yum_repo {

  # configure the repo we want to use
  yumrepo { 'localyum':
    ensure   => 'present',
    name     => 'localyum',
    enabled  => 1,
    descr    => 'Local repo holding company application packages',
    baseurl  => 'file:///packages/downloaded_rpms',
    gpgcheck => 0,
  }

  package { 'vsftpd':
    ensure=> 'present',
  }
}

include local_yum_repo
```

# App Servers

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Get root
sudo -s

# Create local repo
# https://wiki.centos.org/HowTos/CreateLocalRepos
createrepo /packages/downloaded_rpms

# Get configuration changes
puppet agent -t

# Check repo file and installed package
cat /etc/yum.repos.d/localyum.repo # Exists
systemctl status vsftpd # Disable, but installed.
```
