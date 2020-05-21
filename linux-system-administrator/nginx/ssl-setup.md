System admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. As per requirements shared below prepare the server:


Install and configure nginx on App Server 2.

On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

Create an index.html file with content Welcome! under Nginx document root.

For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh tony@stapp01

# Check current Linux version, it was CentOS 7.6
cat /etc/*release*

# Switch to root
sudo -s

# Install EPEL - https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-centos-7
# https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-centos-7
yum install epel-release

# Install, enable, and start nginx
yum install nginx -y
systemctl enable nginx
systemctl start nginx

# Check the listening ports
ss -lntp # Should see 80

# Make the certificate folders
mkdir -m 755 /etc/pki/nginx
mkdir -m 755 /etc/pki/nginx/private

# Copy certificates
cp /tmp/nautilus.crt /etc/pki/nginx/
cp /tmp/nautilus.key /etc/pki/nginx/private/

# Edit /etc/nginx/nginx.conf - https://nginx.org/en/docs/http/configuring_https_servers.html
vi /etc/nginx/nginx.conf
```

```
...
   server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        ssl_certificate "/etc/pki/nginx/nautilus.crt";
        ssl_certificate_key "/etc/pki/nginx/private/nautilus.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
...   
```

```bash
# Reload nginx config and restart
systemctl reload nginx
systemctl restart nginx # Fix any errors inside of journalctl -xe

# Check the listening ports
ss -lntp # Should see 80 and 443

# Create the index.html path and file
mkdir -p /usr/share/doc/HTML
cat > /usr/share/doc/HTML/index.html
Welcome
^D

# Test locally
curl -kv https://localhost # Should be Welcome

# Test from thor
curl -kv https://stapp02 # Should be Welcome
```