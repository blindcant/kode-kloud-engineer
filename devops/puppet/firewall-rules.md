The Nautilus DevOps team was auditing some of the applications running on all app servers in Stratos Datacenter. They found some security loop holes like they observed there is no firewall installed on these apps. So team has decided to install firewalld on all app servers and also some rules need to be added. This task needs to be done using Puppet so as per details mentioned below please compete the task:


Create an inventory file code.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server. In this inventory file you need to define nodes specific classes only which are mentioned below.

1. Define a class firewall_node1 for agent node 1 i.e app server 1, firewall_node2 for agent node 2 i.e app server 2, firewall_node3 and for agent node3 i.e app server

Also create a puppet programming file games.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server and write code to perform below mention task.

1. Install puppet firewall module on master node i.e on Jump Server (you can install manually).

2. There are some different applications running on all three apps. One of the application is using port 8085 on App server 1 , 5008 on App server 2 and 8095 on App server. Complete below mentioned tasks:

a. Open all incoming connection for 8085/tcp port on App Server 1 and zone should be public.

b. Open all incoming connection for 5008/tcp port on App Server 2 and zone should be public.

c. Open all incoming connection for 8095/tcp port on App Server 3 and zone should be public.

# Master

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -Ahl --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Install firewalld - https://forge.puppet.com/puppet/firewalld/readme#firewalld-ports
puppet module install puppet/firewalld

# Change to working directory
cd /etc/puppetlabs/code/environments/production/manifests

# Create the expected inventory file
cat > code.pp
```

* https://forge.puppet.com/puppet/firewalld/readme#firewalld-ports

```
# Contents added/updated to /etc/puppetlabs/code/environments/production/manifests/code.pp

node 'stapp01.stratos.xfusioncorp.com' {
  include firewall_node1
}

node 'stapp02.stratos.xfusioncorp.com' {
  include firewall_node2
}

node 'stapp03.stratos.xfusioncorp.com' {
  include firewall_node3
}
```

```bash
# Create the custom class programming file.
cat > games.pp
```

```
# Contents added/updated to /etc/puppetlabs/code/environments/production/manifests/games.pp

class firewall_node1 {
  firewalld_port { 'Open port 8085 in public zone for stapp01':
      ensure   => present,
      zone     => 'public',
      port     => 8085,
      protocol => 'tcp',
  }
}

class firewall_node2 inherits firewalld {
  firewalld_port { 'Open port 5008 in public zone for stapp02':
    ensure   => present,
    zone     => 'public',
    port     => 5008,
    protocol => 'tcp',
  }
}

class firewall_node3 inherits firewalld {
  firewalld_port { 'Open port 8095 in public zone for stapp03':
    ensure   => present,
    zone     => 'public',
    port     => 8095,
    protocol => 'tcp',
  }
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

# Get configuration changes
puppet agent -t
```

# Master

```bash
# Test connections
curl -v @stapp01:8085 # Ok
curl -v @stapp02:5008 # Ok
curl -v @stapp03:8095 # Ok
```