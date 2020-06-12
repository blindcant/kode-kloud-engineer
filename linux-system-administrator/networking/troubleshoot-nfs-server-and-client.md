Nautilus production support team was trying to fix some issues with their storage server. Storage server has a shared directory /data which is mounted on all app servers at location /var/www/html so that whatever data they store on storage server under /data can be shared among all app servers. Now somehow NFS server is broken and having some issues.


Identify the root cause of issue and fix the same to make sure sharing works fine among all app servers and storage server.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to storage server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh natasha@ststor01

# Get root access
sudo -s

# Check if services are running
systemctl status nfs-server # Down
systemctl status rpcbind # Ok
systemctl status nfs-lock # Ok
systemctl status nfs-idmap # Ok

# Start the services
systemctl start nfs-server # Ok

# Check config file - https://www.unixmen.com/setting-nfs-server-client-centos-7/
vi /etc/exports # IP addresses were wrong, using hostnames instead.
```

```
# /etc/exports additions
/data stapp01(rw,sync,no_root_squash,no_all_squash)
/data stapp02(rw,sync,no_root_squash,no_all_squash)
/data stapp03(rw,sync,no_root_squash,no_all_squash)	
```

```bash
# Export the network file share
exportfs -r # Ok

# Restart service to pick up new configuration
systemctl restart nfs-server # Ok

# Connect to application servers
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Install required packages
yum install -y nfs-utils

# Mount the NFS share
mount -t nfs -o rw ststor01:/data /var/www/html

# Update /etc/fstab for automatic mount
vi /etc/fstab
```

```
# /etc/fstab additions
ststor01:/data 	/var/www/html	 nfs 	rw,sync,no_root_squash,no_all_squash 	0 	0
```
