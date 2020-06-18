Nautilus DevOps team is testing some applications containerization which are supposed to be migrated on docker containers based environments soon. In today's stand-up meeting one of the team member has been assigned a task to create and test a docker container with some particular requirements. Below are more details:


a. On App Server 2 in Stratos DC pull ubuntu image (preferably latest tag but others should work too).

b. Create a new container with name cluster from the image you just pulled.

c. Map the host volume /opt/security with container volume /tmp. There is an sample.txt file present on same server under /tmp, copy that file to /opt/security. Also please keep the container in running state.


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# https://docs.docker.com/storage/bind-mounts/
docker run -d -t \
  --name cluster \
  --mount type=bind,source=/opt/security,target=/tmp \
  ubuntu:latest

# Same as above
docker run -d -t --name cluster -v /opt/security:/tmp ubuntu:latest

# Check the mount worked, get container id
docker ps -a 
docker exec -it $CONTAINER_ID bash
ll /tmp # Should be sample.txt
```
