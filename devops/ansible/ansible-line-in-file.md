he Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible only. Therefore, prepare the required playbook to complete this task. Find more details about the task below.

We already have an inventory file under /home/thor/ansible on jump host. Create a playbook.yml under /home/thor/ansible on jump host itself.

Using the playbook install httpd web server on all app servers, and make sure its service is up and running.

Create a file /var/www/html/index.html with content:

This is a Nautilus sample file, created using Ansible!

Using lineinfile Ansible module add some more content in /var/www/html/index.html file. Below is the content:
Welcome to xFusionCorp Industries!

Also make sure this new line is added at the top of the file.

The /var/www/html/index.html file's user and group owner should be apache on all app servers.

The /var/www/html/index.html file's permissions should be 0744 on all app servers.

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Change to working directory
cd ~/ansible

# Inspect inventory, was okay
cat inventory

# Check existing files
ansible -i inventory all -m ping

# Create playbook
cat > playbook.yml
```

```yaml
# YAML format
---
- name: Install & configure httpd.
  hosts: all
  become: yes
  tasks:
    - name: Install httpd
      # https://docs.ansible.com/ansible/latest/modules/package_module.html
      package:
        name: httpd
        state: present

    - name: Start service httpd, if not started
      service:
        name: httpd
        state: started

    - name: Create index.html
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: '0744'
        state: touch

    - name : Add content to index.html
      lineinfile:
        path: /var/www/html/index.html
        regexp: '^$'
        firstmatch: yes
        line: This is a Nautilus sample file, created using Ansible!

    - name : Add more content to index.html
      lineinfile:
        path: /var/www/html/index.html
        insertbefore: BOF
        line: Welcome to xFusionCorp Industries!

```

```bash
# Run playbook
ansible-playbook -i ./inventory playbook.yml

# Check results
ansible -i inventory all -m shell -a 'systemctl status httpd' # Ok, all services running
ansible -i inventory all -m shell -a 'ls -Ahl /var/www/html' # Ok, file exists with apache: 0744
ansible -i inventory all -m shell -a 'curl localhost' # Ok, web page displayed correctly.
```
