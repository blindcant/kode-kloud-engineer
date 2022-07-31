

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

   1  cd /usr/src/
    2  ll
    3  cd kodekloudrepos/
    4  ll
    5  git status
    6  git clone /opt/games.git
    7  alias l='ls -Ahl --color=auto'
    8  l /opt/beta.git/
    9  pwd
   10  cd /opt/beta.git/
   11  cat config
   12  l
   13  cat description
   14  git init
   15  git add .
   16  git commit
   17  git config --global user.email "root@ststor01"
   18  git config --global user.email "root@ststor01"
   19  git config --global user.name "root"
   20  git commit
   21  git remote -v
   22  git clone . /usr/src/kodekloudrepos/
   23  ll /usr/src/kodekloudrepos/
   24  l
   25  ll /usr/src/kodekloudrepos/beta.git
   26  rm /usr/src/kodekloudrepos/*
   27  rm -rf /usr/src/kodekloudrepos/*
   28  ll /usr/src/kodekloudrepos/
   29  git clone . /usr/src/kodekloudrepos/beta.git
   30  ll /usr/src/kodekloudrepos/beta.git
   31  l /usr/src/kodekloudrepos/beta.git
   32  history



   - '/opt/beta.git' git repository is not cloned under '/usr/src/kodekloudrepos/' on Storage Server

[root@ststor01 beta.git]# cd /usr/src/kodekloudrepos
[root@ststor01 kodekloudrepos]# l
total 8.0K
drwxr-xr-x 5 root root 4.0K Aug 10 01:16 beta.git
drwxr-xr-x 8 root root 4.0K Aug 10 01:14 .git
[root@ststor01 kodekloudrepos]# rm -rf .git
[root@ststor01 kodekloudrepos]# l
total 4.0K
drwxr-xr-x 5 root root 4.0K Aug 10 01:16 beta.git
[root@ststor01 kodekloudrepos]# l beta.git/
total 24K
-rw-r--r-- 1 root root   66 Aug 10 01:16 config
-rw-r--r-- 1 root root   73 Aug 10 01:16 description
drwxr-xr-x 8 root root 4.0K Aug 10 01:16 .git
-rw-r--r-- 1 root root   23 Aug 10 01:16 HEAD
drwxr-xr-x 2 root root 4.0K Aug 10 01:16 hooks
drwxr-xr-x 2 root root 4.0K Aug 10 01:16 info
[root@ststor01 kodekloudrepos]# thor@jump_host /$


kke_lab_devops-git-clone