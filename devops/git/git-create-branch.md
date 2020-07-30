Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/cluster. They recently decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

1. On Storage server in Stratos DC create a new branch xfusioncorp_cluster from master branch in /usr/src/kodekloudrepos/cluster git repo.

Please do not try to make any changes in code.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to storage server logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh natasha@ststor01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Go to the correct path
cd /usr/src/kodekloudrepos/cluster/

# Check current branch
git status # Wasn't master

# SWitch to master and create new branch
git checkout master
git checkout -b  xfusioncorp_cluster
```
