The Nautilus DevOps team is working to deploy one of the applications on App Server 3 in Stratos DC. Due to a misconfiguration in the docker compose file, the deployment is failing. We would like you to take a look into it to identify and fix the issues. More details can be found below:

a. docker-compose.yml file is present on App Server 3 under /opt/docker directory.

b. Try to run the same and make sure it works fine.

c. Please do not change the container names being used. Also, do not update or alter any other valid config settings in the compose file or any other relevant data that can cause app failure.

Note: Please note that once you click on FINISH button all existing running/stopped containers will be destroyed, and your compose will be run.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh banner@stapp03

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Create the Docker Compose file - https://docs.docker.com/compose/
cd /opt/docker
cat > docker-compose.yml

# Deploy the container
docker-compose up

# Fixed the following errors
```

```
ERROR: Version in "./docker-compose.yml" is invalid. You might be seeing this error because you're using the wrong Compose file version. Either specify a supported version (e.g "2.2" or "3.3") and place your service definitions under the `services` key, or omit the `version` key and place your service definitions at the root of the file to use version 1.
For more on the Compose file format versions, see https://docs.docker.com/compose/compose-file/

Changed to version 3.3

ERROR: The Compose file './docker-compose.yml' is invalid because:
Unsupported config option for services.web: 'depends'

Changed depends to depends_on

ERROR: The Compose file './docker-compose.yml' is invalid because:
Unsupported config option for services.web: 'volume' (did you mean 'volumes'?)

Changed volume to volumes

ERROR: build path /opt/docker/redis either does not exist, is not accessible, or is not a valid URL.

Changed build to image
```
```bash
# Deploy the container with the final YAML
docker-compose up # Okay.
```

```yaml
version: '3.3'
services:
    web:
        build: .
        container_name: python
        ports:
            - "5000:5000"
        volumes:
            - .:/code
        depends_on:
            - redis
    redis:
        image: redis
        container_name: redis
```
