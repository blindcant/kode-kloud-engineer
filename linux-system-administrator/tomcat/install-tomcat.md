Nautilus application development team recently finished beta version of one of their java based application. They are planning to deploy the same on one of the app server in Stratos DC. After having an internal team meeting they have decided to use tomcat application server. Based on the requirements mentioned below complete the task:


a. Install tomcat server on App Server 2 using yum.

b. Configure it to run on port 8089.

c. There is a ROOT.war file on Jump host at location /tmp, deploy the same on this tomcat server and make sure webpage should directly work on base URL i.e without specifying any sub-directory anything like this http://URL/ROOT .

d. You can access the website on LBR link, to do so click on the + button on top of your terminal and select option Select port to view on Host 1 and after adding port 80 click on Display Port.

```bash
# Connect to stapp02
ssh steve@stapp02

# Change to root
sudo -s

# Check if Java is installed, it wasn't.
java -version

# Install tomcat, which also installs java 1.8 - https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-centos-7-via-yum
yum install tomcat

#  Change port - https://stackoverflow.com/questions/18415578/how-to-change-tomcat-port-number
vi /etc/tomcat/conf/server.xml
  # Added in <Connector port="8089"

# Start and test
systemctl start tomcat
systemctl status tomcat # Running
curl -v localhost:8089 # 404 error since we have nothing to serve

# Drop back to thor jumpbox
^D

# Copy Java app from jumpbox to app server
scp /tmp/ROOT.war steve@stapp02:/home/steve

# Reconnect to stapp02
ssh steve@stapp02

# Change to sudo
sudo -s

# Copy to the correct place - https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-centos-7-via-yum
mv ~steve/ROOT.war /usr/share/tomcat/webapps

# Update WAR ownership
chown tomcat: /usr/share/tomcat/webapps/ROOT.war

# Test webapp
curl localhost@8089 # See the SampleWebApp page.
```
