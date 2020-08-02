Nautilus Application development team wants to test an Apache and PHP setup on one of the app servers in Stratos Datacenter. They want the DevOps team to prepare an Ansible playbook to accomplish this task. Below you can find more details about the task.


There is an inventory file ~/playbooks/inventory on jump host.

Create a playbook ~/playbooks/httpd.yml on jump host and perform the following tasks on App Server 2.

a. Install httpd and php packages (whatever default version is available in yum repo).

b. Change default document root of Apache to /var/www/html/myroot in default Apache config /etc/httpd/conf/httpd.conf. Make sure /var/www/html/myroot path exists (if not please create the same).

c. There is a template ~/playbooks/templates/phpinfo.php.j2 on jump host. Copy this template to the Apache document root you created as phpinfo.php file and make sure user owner and the group owner for this file is apache user.

d. Start and enable httpd service.

Note: Validation will try to run playbook using command ansible-playbook -i inventory httpd.yml, so please make sure playbook works this way without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create ssh key and deploy to stapp03
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519 steve@stapp02

# Change to working directory
cd ~/ansible

# Inspect inventory, was Ok
cat inventory

# Test inventory, was Ok
ansible -i ./inventory all -m ping

# Create playbook
vi playbook.yml
```

```yaml
---
- hosts: stapp02
  become: yes

  tasks:
    # https://docs.ansible.com/ansible/latest/modules/package_module.html
    - name: Install packages
      package:
        name:
          - httpd
          - php
        state: latest

    # https://docs.ansible.com/ansible/latest/modules/replace_module.html
    - name: Replace httpd DocumentRoot
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: 'DocumentRoot "/var/www/html"'
        replace: 'DocumentRoot "/var/www/html/myroot"'

    # https://docs.ansible.com/ansible/latest/modules/file_module.html
    - name: Make sure new httpd DocumentRoot Exists
      file:
        path: /var/www/html/myroot
        owner: root
        group: root
        mode: '0755'
        state: directory

    # https://docs.ansible.com/ansible/latest/modules/template_module.html
    - name: Template a file to /etc/files.conf
      template:
        src:  ~/playbooks/templates/phpinfo.php.j2
        dest: /var/www/html/myroot/phpinfo.php
        owner: apache
        group: apache
        mode: '0644'

    # https://docs.ansible.com/ansible/latest/modules/service_module.html
    - name: Start and enable service httpd.
      service:
        name: httpd
        state: started
        enabled: yes

```

```bash
# Run playbook
ansible-playbook -i ./inventory httpd.yml

# Check results, Ok
curl @stapp02/phpinfo.php
```
