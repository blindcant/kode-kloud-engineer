```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create .ssh folder and keys
mkdir -p ~/.ssh
ssh-keygen -i ed25519 -f ~/.ssh/id_ed2559

# Copy keys
ssh-copy-id -i ~/.ssh/id_ed25519 tony@stapp01
ssh-copy-id -i ~/.ssh/id_ed25519 steve@stapp02
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03
ssh-copy-id -i ~/.ssh/id_ed25519 peter@stdb01

# Copy motd to app servers
cp ~/motd_template tony@stapp01:~
cp ~/motd_template steve@stapp02:~
cp ~/motd_template banner@stapp03:~

# Overrite /etc/motd
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

sudo -s

mv motd_template /etc/motd

# Check banner
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# DB server didn't have scp - https://askubuntu.com/a/872537
cat motd_template | ssh peter@stdb01 "sudo tee -a /etc/motd"
```
