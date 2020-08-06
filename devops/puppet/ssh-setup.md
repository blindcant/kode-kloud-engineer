The Puppet master and Puppet agent nodes have been set up by the Nautilus DevOps team so they can perform testing. In Stratos DC all app servers have been configured as Puppet agent nodes. They want to setup a password less SSH connection between puppet master and puppet agent nodes and this task needs to be done using puppet itself. Below are details about the task:

1. Create a puppet programming file official.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server. Define a class ssh_node1 for agent node 1 i.e app server 1, ssh_node2 for agent node 2 i.e app server 2, ssh_node3 for agent node3 i.e app server 3. We already have a default ssh key under location /root/.ssh/ on Jump Server that needs to be added on all app servers.

2. Configure a password less SSH connection from puppet master i.e jump host to all app servers. However make sure key is added to each app's sudo user (i.e tony for app server 1)

Note: Create a single puppet programming code official.pp for above mentioned tasks.

# Master

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Install sshkeys_core - https://forge.puppet.com/puppetlabs/sshkeys_core
puppet module install puppetlabs/sshkeys_core

# Change to working directory
cd /etc/puppetlabs/code/environments/production/manifests

# Create the custom class programming file and inventory file.
# https://forge.puppet.com/puppetlabs/sshkeys_core/reference#ssh_authorized_key
cat > official.pp
```

```
# Contents added/updated to /etc/puppetlabs/code/environments/production/manifests/games.pp

class ssh_node1 {
  ssh_authorized_key { 'tony@stapp01':
      ensure  =>  present,
      user  =>  'tony',
      type  =>  'ssh-rsa',
      key =>  'AAAAB3NzaC1yc2EAAAADAQABAAABAQDPM6WAF/5bG2Ds0m/OZ8IbqQQPBNlfeSLp0ArfbsrYER7CcdRuE/8AzBUBRcsNWLhHY4RINHZTm1j8e+Or4xWL+P1cxVqeSy6hjJBCGfsakBK/LkcWR/0lgO7VRniyPgUgat3AquT7cDHtHxU6MRdFMyDlJXL35cAfKx67/mbiaATsXov1FI25GHbddBQYqnJzwJLgtKaR507WTJuGu/sWBhpqumWOPvaNHoTXfh1/uCPtlQo9VJRK16Zl8+SqJmHnsfu97kjo3PqA3p2DSFhJBeB/fwGGjI1t6ASgzMqO7mvQLkR4W1EnWY/fFb4ouY6hmeFP+tiCsW57e2AevnEF',
  }
}

class ssh_node2 {
  ssh_authorized_key { 'steve@stapp02':
      ensure  =>  present,
      user  =>  'steve',
      type  =>  'ssh-rsa',
      key =>  'AAAAB3NzaC1yc2EAAAADAQABAAABAQDPM6WAF/5bG2Ds0m/OZ8IbqQQPBNlfeSLp0ArfbsrYER7CcdRuE/8AzBUBRcsNWLhHY4RINHZTm1j8e+Or4xWL+P1cxVqeSy6hjJBCGfsakBK/LkcWR/0lgO7VRniyPgUgat3AquT7cDHtHxU6MRdFMyDlJXL35cAfKx67/mbiaATsXov1FI25GHbddBQYqnJzwJLgtKaR507WTJuGu/sWBhpqumWOPvaNHoTXfh1/uCPtlQo9VJRK16Zl8+SqJmHnsfu97kjo3PqA3p2DSFhJBeB/fwGGjI1t6ASgzMqO7mvQLkR4W1EnWY/fFb4ouY6hmeFP+tiCsW57e2AevnEF',
  }
}

class ssh_node3 {
  ssh_authorized_key { 'banner@stapp03':
      ensure  =>  present,
      user  =>  'banner',
      type  =>  'ssh-rsa',
      key =>  'AAAAB3NzaC1yc2EAAAADAQABAAABAQDPM6WAF/5bG2Ds0m/OZ8IbqQQPBNlfeSLp0ArfbsrYER7CcdRuE/8AzBUBRcsNWLhHY4RINHZTm1j8e+Or4xWL+P1cxVqeSy6hjJBCGfsakBK/LkcWR/0lgO7VRniyPgUgat3AquT7cDHtHxU6MRdFMyDlJXL35cAfKx67/mbiaATsXov1FI25GHbddBQYqnJzwJLgtKaR507WTJuGu/sWBhpqumWOPvaNHoTXfh1/uCPtlQo9VJRK16Zl8+SqJmHnsfu97kjo3PqA3p2DSFhJBeB/fwGGjI1t6ASgzMqO7mvQLkR4W1EnWY/fFb4ouY6hmeFP+tiCsW57e2AevnEF',
  }
}

node 'stapp01.stratos.xfusioncorp.com' {
  include ssh_node1
}

node 'stapp02.stratos.xfusioncorp.com' {
  include ssh_node2
}

node 'stapp03.stratos.xfusioncorp.com' {
  include ssh_node3
}
```

# Nodes

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Switch to root
sudo -s

# Install sshkeys_core
puppet module install puppetlabs/sshkeys_core

# Get configuration changes
puppet agent -t

# Check the authorized_keys file
cat ~tony/.ssh/authorized_keys
cat ~steve/.ssh/authorized_keys
cat ~banner/.ssh/authorized_keys

```

# Master

```bash
# Test connections
ssh tony@stapp01 # Ok, no password needed
ssh steve@stapp02 # Ok, no password needed
ssh banner@stapp03 # Ok, no password needed
```
