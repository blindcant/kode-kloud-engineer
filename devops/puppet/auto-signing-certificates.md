During last weekly meeting Nautilus DevOps team has decided to use puppet autosign config to auto sign certificates for all puppet agent nodes they will keep on adding under puppet master in Stratos DC. Currently puppet master and CA servers are running on jump host and they have configured all three app servers as puppet agent. To setup autosign configuration on puppet master server there are some configuration settings need to be done. Please find below more details about the same:


Puppet server package is already installed on puppet master i.e jump server and puppet agent package is already installed on all App Servers. But you may need to start required services on all these servers.

Configure autosign configuration on puppet master i.e jump server (by creating autosign.conf in puppet configuration directory) and assign certificate for master node as well as for all agent nodes. Use host's FDQN to assign the certificates.

Use an alias puppet (dns_alt_name) for master node and add its entry in /etc/hosts config file on master i.e Jump Server as well as on all agent nodes i.e App Servers.

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -aFlh --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Start and enable the server service
puppet resource service puppetserver ensure=running
puppet resource service puppetserver enable=true

# Check to see if its listening
ss -lntp # Should see Java on 8140

# Update DNS
vi /etc/hosts -> 127.0.0.1       puppet
```

```
# /etc/hosts contents added/updated
127.0.0.1 localhost puppet

```


```bash
# Removing old key - https://puppet.com/docs/puppet/latest/ssl_regenerate_certificates.html
# https://puppet.com/docs/puppet/latest/configuration.html#dns_alt_names
puppetserver ca clean --certname $(hostname -f)

# Stop the service
puppet resource service puppetserver ensure=stopped

# Try to generate new key- https://puppet.com/docs/puppetserver/latest/subcommands.html
puppetserver ca generate --certname $(hostname -f) --subject-alt-names $(hostname -f),puppet --ca-client

# Had to delete these files
rm /etc/puppetlabs/puppet/ssl/private_keys/jump_host.stratos.xfusioncorp.com.pem /etc/puppetlabs/puppet/ssl/public_keys/jump_host.stratos.xfusioncorp.com.pem /etc/puppetlabs/puppet/ssl/certs/jump_host.

# Generate new key - https://puppet.com/docs/puppetserver/latest/subcommands.html
puppetserver ca generate --certname $(hostname -f) --subject-alt-names $(hostname -f),puppet --ca-client
puppet resource service puppetserver ensure=running

# How to autosign - https://puppet.com/docs/puppet/latest/config_file_autosign.html
# Where the configuration folder is - https://puppet.com/docs/puppet/latest/dirs_confdir.html
cat > /etc/puppetlabs/puppet/autosign.conf
jump_host.stratos.xfusioncorp.com
stapp01.stratos.xfusioncorp.com
stapp02.stratos.xfusioncorp.com
stapp03.stratos.xfusioncorp.com
```

```bash
# Connect to application servers
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Update DNS to point to the Puppet master server
vi /etc/hosts
```

```
# /etc/hosts contents added/updated
172.16.238.3    jump_host.stratos.xfusioncorp.com puppet
```

```bash
# Register agents
cat >> /etc/puppetlabs/puppet/puppet.conf
```

```
# /etc/puppetlabs/puppet/puppet.conf contents added
server = puppet
```

```bash
# Regnerate SSL certs - https://puppet.com/docs/puppet/latest/ssl_regenerate_certificates.html
puppet ssl clean
puppet resource service puppet ensure=stopped
rm -rf /etc/puppetlabs/puppet/ssl/*
puppet resource service puppet ensure=running
```