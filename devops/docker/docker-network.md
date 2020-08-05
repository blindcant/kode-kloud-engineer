The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as ecommerce on App Server 2 in Stratos DC.

b. Configure it to use bridge drivers.

c. Set it to use subnet 192.168.0.0/24 and iprange 192.168.0.2/24.

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

# Create the docker network
docker network create --ip-range 192.168.0.2/24 
--subnet 192.168.0.0/24 ecommerce

# Check the docker network
docker network ls
docker network inspect ecommerce
```
