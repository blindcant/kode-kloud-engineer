Nautilus Application development team has finished development of one of the app recently which they want to deploy on a containerized platform. Nautilus Application development and DevOps team had a meeting to discuss some of the basic pre-requisites and requirements to complete the deployment. Team want to test the deployment on one of the app server before going live and they want to setup complete containerized stack using a docker compose fie. Please find below mentioned details about the task:


On app server 1 in Stratos Datacenter create a docker compose file /opt/finance/docker-compose.yml (should be named exactly).

The compose should deploy two services web and DB and each service should deploy a container as per details mentioned below:

For web service:

a. Container name must be php_web.

b. Use image php with any apache tag. Check here for more details https://hub.docker.com/_/php?tab=tags.

c. Map php_web container's port 80 with host port 3003

d. Map php_web container's /var/www/html volume with host volume /var/www/html.

For DB service:

a. Container name must be mysql_web.

b. Use image mariadb with any tag (preferably latest). Check here for more details https://hub.docker.com/_/mariadb?tab=tags.

c. Map mysql_web container's port 3306 with host port 3306

d. Map mysql_web container's /var/lib/mysql volume with host volume /var/lib/mysql.

e. Set MYSQL_DATABASE=database_web and use user root with some complex password for DB connections.

After running docker-compose up finally you can access the app with curl command curl <server-ip or hostname>:3003/
For more details check here: https://hub.docker.com/_/mariadb?tab=description

Note: Once you click on FINISH button all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01
# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Create the Docker Compose file - https://docs.docker.com/compose/
cat > /opt/finance/docker-compose.yml
```

```yaml
version: '3.3'
services:
  php_web:
    # https://docs.docker.com/config/containers/container-networking/
    ports:
      - "3003:80" # host:container
    volumes:
      - /var/www/html:/var/www/html # host:container
    image: php:apache
  mysql_web:
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    image: mariadb:latest
    # https://hub.docker.com/_/mariadb
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_ROOT_PASSWORD: P@$$word123.
```

```bash
# Deploy the container
docker-compose up

# Test the container
curl localhost:3003 # Saw the contents of the host's /var/www/html/index.php
```
