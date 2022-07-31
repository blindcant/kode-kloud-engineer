Some new developers have joined xFusionCorp Industries and have been assigned Nautilus project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on storage server in Stratos DC.

a. Install git, set up any values for user.email and user.name globally and create a bare repository /opt/apps.git.

b. There is an update hook (to block direct pushes to master branch) under /tmp on storage server itself; use the same to block direct pushes to master branch in /opt/apps.git repo.

c. Clone /opt/apps.git repo in /usr/src/kodekloudrepos/apps directory.

d. Create a new branch xfusioncorp_apps in repo that you cloned in /usr/src/kodekloudrepos.

e. There is a readme.md file in /tmp on storage server itself; copy that to repo, add/commit in the new branch you created, and finally push your branch to origin.

f. Also create master branch from your branch and remember you should not be able to push to master as per hook you have set up.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to storage server logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh natasha@ststor01

# Install git
yum install -y git

# Create repo
mkdir -p /opt/apps
cd /opt/apps
git init

# Set up default user information.
git config --global user.name "root"
git config --global user.email "root@localhost"

# Create hook to block pushes to master
# https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks
cp /tmp/update /opt/apps/.git/hooks/update
chmod 755 /opt/apps/.git/hooks/update

# Clone the repo to a new location
git clone . /usr/src/kodekloudrepos/apps/

# Create a new branch
cd /usr/src/kodekloudrepos/apps
git checkout -b xfusioncorp_apps

# Create the initial commit
cp /tmp/readme.md .
git push origin xfusioncorp_apps

# Create master branch
git checkout -b master

# Test git hook
git push origin master # Ok, was blocked.
```
