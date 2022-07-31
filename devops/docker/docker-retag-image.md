Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

a. Pull `busybox:musl` image on App Server 1 in Stratos DC and re-tag (create new tag) this image as `busybox:blog`.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -i

# Pull image
docker pull busybox:musl

# Get image ID
docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
busybox      musl      886b6935b1b9   37 hours ago   1.4MB

# Tag image
docker tag 886b6935b1b9 busybox:blog

# Check new tag
docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
busybox      blog      886b6935b1b9   37 hours ago   1.4MB
busybox      musl      886b6935b1b9   37 hours ago   1.4MB

```