```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create ~/.ssh folder
mkdir -p ~/.ssh
cd ~/.ssh

# Generate ssh keys - https://www.ssh.com/ssh/keygen#creating-an-ssh-key-pair-for-user-authentication
ssh-keygen -f ~/.ssh/id_ed25519 -t ed25519
	No password

# Copy ssh private key to required servers - https://www.ssh.com/ssh/keygen#copying-the-public-key-to-the-server
# Application server logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh-copy-id -i ~/.ssh/id_ed25519 tony@stapp01
ssh-copy-id -i ~/.ssh/id_ed25519 steve@stapp02
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03

# Test passwordless ssh with user
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Configure ssh config - https://www.ssh.com/ssh/config & https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/
cat > ~/.ssh/config

Host stapp01
	User tony
	IndetityFile ~/.ssh/id_ed25519
	PreferredAuthentications publickey

Host stapp02
	User steve
	IndetityFile ~/.ssh/id_ed25519
	PreferredAuthentications publickey

Host stapp03
	User banner
	IndetityFile ~/.ssh/id_ed25519
	PreferredAuthentications publickey
^D

# Update ~/.ssh/config file permissions so it works.
chmod 644 ~/.ssh/config

# Test passwordless ssh without user
ssh stapp01
ssh stapp02
ssh stapp03
```
