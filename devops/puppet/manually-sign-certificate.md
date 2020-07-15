The Nautilus DevOps team has set up a puppet master and an agent node in Stratos Datacenter. Puppet master is running on jump host itself (also note that Puppet master node is also running as Puppet CA server) and Puppet agent is running on App Server 1. Since it is a fresh set up, the team wants to sign certificates for puppet master as well as puppet agent nodes so that they can proceed with the next steps of set up. You can find more details about the task below:

Puppet server and agent nodes are already have required packages, but you may need to start puppetserver (on master) and puppet service on both nodes.

Assign and sign certificates for both master and agent node.

# Master

```bash
# Update prompt as it was crappy sh
PS1='[\u@\h \W] \$ '
alias l='ls -aFlh --color=auto'
reset

# Add the puppet installation into our PATH
export PATH=/opt/puppetlabs/bin:$PATH

# Check current CA certificates
# https://puppet.com/docs/puppet/latest/ssl_certificates.html
# https://puppet.com/docs/puppet/latest/puppet_server_ca_cli.html
puppetserver ca list --all # Service not running

# Check service status
systemctl status puppet # Was dead

# Start and enable the service
systemctl start puppet
puppet resource service puppetserver ensure=running
puppet resource service puppetserver enable=true

# Check the service
systemctl status puppet # Running, but errors
ss -lntp # Listening on 8140

# Update DNS
vi /etc/hosts
```

```
# /etc/hosts contents added/updated
127.0.0.1 localhost puppet
```

```bash
# Restart the service
systemctl restart puppet
systemctl status puppet # Running

# Check current certificates
puppetserver ca list --all # thor only
```

# Agents

```bash
# Connect to application servers which are the puppet agents
ssh tony@stapp01

# Switch to root
sudo -s

# Check service
systemctl status puppet # Running, but errors

# Update DNS to point to the Puppet master server
vi /etc/hosts
```

```
# /etc/hosts contents added/updated
172.16.238.3    jump_host.stratos.xfusioncorp.com puppet
```

```bash
# Register agents
vi /etc/puppetlabs/puppet/puppet.conf
```

```
# /etc/puppetlabs/puppet/puppet.conf contents added
server = puppet
```

```bash
# Restart service
systemctl restart puppet
systemctl status puppet # Running

# Get the local machine's configuration from the master
# https://puppet.com/docs/puppet/latest/man/agent.html
puppet agent -t # Could'nt get certificate, needs signing
```

# Master

```bash
# Check the certificates
puppetserver ca list --all # Can thor done, not stapp01

# Sign certificate
puppetserver ca sign --certname stapp01.stratos.xfusioncorp.com
puppetserver ca list --all # thor and stapp01 done.
```

# Agents

```bash
# Get the local machine's configuration from the master
puppet agent -t # Done
```
