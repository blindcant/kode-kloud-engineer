One of the DevOps team members was working on to create a new custom docker image on App Server 1 in Stratos DC. He is done with his changes and image is saved on same server with name news:nautilus. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on App Server 3. So we need to provide them that image on App Server 3 in Stratos DC.


a. On App Server 1 save the image news:nautilus in an archive.

b. Transfer the image archive to App Server 3.

c. Load that image archive on App Server 3 with same name and tag which was used on App Server 1.

Note: Docker is already installed on both servers; however, if its service is down please make sure to start it.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Check if Docker daemon is running
systemctl status docker # Ok

# Check Docker images
docker image ls

# Save Docker image to tar file
docker save -o image.tar news:nautilus

# Zip up Docker image to save space
gzip image.tar

# Copy to the other server
scp image.tar.gz banner@stapp03:/home/banner/

# Connect to the other server, from thor
ssh banner@stapp03

# Check O/S, it was CentOS 7
cat /etc/*release*

# Switch to root
sudo -s

# Check if Docker daemon is running
systemctl status docker # Not running
systemctl start docker
systemctl status docker # Ok

# Unzip the file
gunzip image.tar.gz

# Recreate the Docker image from the tar file
docker load -i ./image.tar

# Check it was created
docker image ls # Ok
```
