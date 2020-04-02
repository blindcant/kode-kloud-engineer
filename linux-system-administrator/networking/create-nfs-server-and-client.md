```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to storage server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh natasha@ststor01

# Get root access
sudo -s

# Install required packages - https://www.thegeekdiary.com/centos-rhel-7-configuring-an-nfs-server-and-nfs-client/
yum install -y nfs-utils rpcbind

# Enable services at boot
systemctl enable nfs-server
systemctl enable rpcbind
systemctl enable nfs-lock
systemctl enable nfs-idmap

# Start the services
systemctl start nfs-server
systemctl start rpcbind
systemctl start nfs-lock
systemctl start nfs-idmap

# Edit the configuration file - https://www.unixmen.com/setting-nfs-server-client-centos-7/
vi /etc/exports
	/code stapp01(rw,sync,no_root_squash,no_all_squash)
	/code stapp02(rw,sync,no_root_squash,no_all_squash)
	/code stapp03(rw,sync,no_root_squash,no_all_squash)	

# Export the network file share
exportfs -r

# Restart service to pick up new configuration
systemctl restart nfs-server

# Connect to application servers
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Install required packages
yum install -y nfs-utils

# Create the NFS share folder
mkdir -p /var/www/data

# Mount the NFS share
mount -t nfs -o rw ststor01:/code /var/www/data

# Update /etc/fstab for automatic mount
vi /etc/fstab
	ststor01:/code 	/var/www/data	 nfs 	rw,sync,no_root_squash,no_all_squash 	0 	0
```