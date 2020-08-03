The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 2 in Stratos Datacenter. Please perform the task as per details mentioned below:

1. Pull nginx:alpine docker image on Application Server 2.

2. Create a container named cluster using the image you pulled.

3. Map host port 8081 to container port 80. Please keep the container in running state.

# Jump Server

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Pull the image
docker pull nginx:alpine

# Run the container mapping 8081 host to 80 container
docker run -d -t --name cluster -p 8081:80  nginx:alpine

# Check the container is running
docker ps -a 
curl localhost:8081
```
