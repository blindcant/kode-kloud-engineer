There is a static website of Nautilus project running in Stratos Datacenter. Based on the infrastructure, they have already configured app servers and code is already deployed there. To make it work properly, they need to configure LBR server. There are number of options for that, but team has decided to go with HAproxy.


a. So install and configure HAproxy on LBR server using yum only and make sure all app servers are added to HAproxy load balancer. HAproxy must serve on default http port (Note: Please do not remove stats socket /var/lib/haproxy/stats entry from haproxy default config.).

b. You can access the website on LBR link—to do so click on the + button on top of your terminal, select option Select port to view on Host 1, and after adding port 80 click on Display Port.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to load balancing server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh loki@stlb01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/load_balancer_administration/install_haproxy_example1
# Install HA Proxy
sudo yum install -y haproxy

# Configure HA Proxy
vi /etc/haproxy/haproxy.cfg

# Get httpd port, ssh from thor
ssh tony@stapp01
fgrep Listen /etc/httpd/httpd.conf # Was 6400
```

```
# Contents added/updated to /etc/haproxy/haproxy.cfg
#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
frontend  main *:80
    mode http

#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
backend app
    balance     roundrobin
    server  app1 172.16.238.10:6400 check
    server  app2 172.16.238.11:6400 check
    server  app3 172.16.238.12:6400 check
```

```bash
# Start and enable HA Proxy
systemctl start haproxy
systemctl enable haproxy

# Check HA Proxy is working - internal
curl localhost:80 # Ok

# Check HA Proxy is working - externally from thor
curl stlb01:80 # Ok
```
