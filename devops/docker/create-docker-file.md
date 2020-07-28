As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 2 in Stratos DC and configure to build an image with the following requirements:


a. Use ubuntu as the base image.

b. Install apache2 and configure it to work on 3004 port. (do not update any other Apache configuration settings like document root etc).


```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Create Dockerfile - https://docs.docker.com/get-started/part2/
cat > /opt/docker/Dockerfile
```

```dockerfile
# Use the official image as a parent image.
# https://github.com/dhanugupta/docker-apache-ubuntu/blob/master/Dockerfile
FROM ubuntu

# Run the command inside your image filesystem.
RUN apt-get -y update
RUN apt-get -y upgrade
# Disable tzdata interactive questions - https://serverfault.com/a/992421
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install apache2

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

# Add metadata to the image to describe which port the container is listening on at runtime.
EXPOSE 3004

# Update Apache listneing port
RUN echo "Listen 3004" >> /etc/apache2/apache2.conf

# Start apache.
CMD /usr/sbin/apache2ctl -D FOREGROUND
```

```bash
# Build the Dockerfile - https://docs.docker.com/get-started/part2/#build-and-test-your-image
docker build --tag my-apache2 .

# Run the container - https://docs.docker.com/get-started/part2/#run-your-image-as-a-container
docker run --publish 3004:3004 --detach --name my-httpd-container my-apache2

# Check container is running
docker ps -a
ss -lntp

# Check container connectivity
curl localhost:3004
