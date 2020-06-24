

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Update and check remotes
git remote update
git remote  -v

# Add new remote
git remote add dev_ecommerce /opt/xfusioncorp_ecommerce.git

# Copy and commit file
cp /tmp/index.html .
git add index.html
git commit

# Push the changes to the desired repo and branch
git push dev_ecommerce master
```
