The Nautilus application development team shared static website content that needs to be hosted on the httpd web server using a containerized platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:

a. On App Server 1 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).

b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.

c. Map 80 number port of container with port 8089 of docker host.

d. Map container's /usr/local/apache2/htdocs volume with /opt/security volume of docker host which is already there. (please do not modify any data within these locations).

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh steve@stapp02

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Create the Docker Compose file - https://docs.docker.com/compose/
cat /opt/docker
cat > docker-compose.yml
```

```yaml
version: '3.3'
services:
  httpd-service:
    # https://docs.docker.com/config/containers/container-networking/
    ports:
      - "8089:80" # host:container
    volumes:
      - /opt/security:/usr/local/apache2/htdocs # host:container
    image: httpd:latest
    # https://docs.docker.com/compose/compose-file/#container_name
    container_name: httpd

```

```bash
# Deploy the container
docker-compose up

# Test the container
curl localhost:8089 # Didn't see index1.html but that is okay.
```
