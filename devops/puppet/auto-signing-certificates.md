During last weekly meeting Nautilus DevOps team has decided to use puppet autosign config to auto sign certificates for all puppet agent nodes they will keep on adding under puppet master in Stratos DC. Currently puppet master and CA servers are running on jump host and they have configured all three app servers as puppet agent. To setup autosign configuration on puppet master server there are some configuration settings need to be done. Please find below more details about the same:

Puppet server package is already installed on puppet master i.e jump server and puppet agent package is already installed on all App Servers. But you may need to start required services on all these servers.

Configure autosign configuration on puppet master i.e jump server (by creating autosign.conf in puppet configuration directory) and assign certificate for master node as well as for all agent nodes. Use host's FDQN to assign the certificates.

Use an alias puppet (dns_alt_name) for master node and add its entry in /etc/hosts config file on master i.e Jump Server as well as on all agent nodes i.e App Servers.

# Master

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -aFlh --color=auto'

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Create a default intermediate certificate authority (CA) on the puppet master (jump host)
# https://puppet.com/docs/puppet/latest/ssl_certificates.html
# https://puppet.com/docs/puppet/latest/puppet_server_ca_cli.html
puppetserver ca setup # Error, certs already exist

# Update DNS
cat >> /etc/hosts
```

```
# /etc/hosts contents added/updated
127.0.0.1 localhost puppet
```

```bash
# Start and enable the server service
puppet resource service puppetserver ensure=running
puppet resource service puppetserver enable=true

# Check to see if its listening
ss -lntp # Should see Java on 8140


# Autosign is on by default but the whitelist is empty, populate this file.
# https://puppet.com/docs/puppet/latest/config_file_autosign.html 
# https://puppet.com/docs/puppet/latest/ssl_autosign.html#ssl_basic_autosigning
# Where the configuration folder is - https://puppet.com/docs/puppet/latest/dirs_confdir.html
cat > /etc/puppetlabs/puppet/autosign.conf
```

```
# /etc/puppetlabs/puppet/autosign.conf contents added/updated
jump_host.stratos.xfusioncorp.com
stapp01.stratos.xfusioncorp.com
stapp02.stratos.xfusioncorp.com
stapp03.stratos.xfusioncorp.com
```

```bash
# Add some stuff to the master config file
vi /etc/puppetlabs/puppet/puppet.conf
```

```
# /etc/puppetlabs/puppet/puppet.conf contents added/updated
certname = puppet
dns_alt_names = puppet,jump_host.stratos.xfusioncorp.com
server = jump_host.stratos.xfusioncorp.com
autosign = true
```

# Agents

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Update DNS to point to the Puppet master server
cat >> /etc/hosts
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
# Get the local machine's configuration from the master
# https://puppet.com/docs/puppet/latest/man/agent.html
puppet agent -t
```

# Master

```bash
# Check the certificates
puppetserver ca list --all # Can see all hosts, done
```