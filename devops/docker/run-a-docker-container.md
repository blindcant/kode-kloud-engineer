Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 2. Please complete the task as per details given below:


On Application Server 2 create a container named nginx_2 using image nginx with alpine tag and make sure container is in running state.

 

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s
```