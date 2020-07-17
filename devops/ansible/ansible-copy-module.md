There is data on jump host that needs to be copied on all application servers in Stratos DC. Nautilus DevOps team want to perform this task using Ansible only. Perform this task using Ansible as per details mentioned below:

a. On jump host create an inventory file /home/thor/ansible/inventory and add all application servers as managed nodes.

b. On jump host create a playbook /home/thor/ansible/playbook.yml to copy /usr/src/sysops/index.html file to all application servers at location /opt/sysops.

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Change to working directory
cd ~/ansible

# Create inventory
cat > inventory
```

```yaml
# YAML format
all:
  hosts:
    stapp01:
      ansible_user: tony
      ansible_password: Ir0nM@n
    stapp02:
      ansible_user: steve
      ansible_password: Am3ric@
    stapp03:
      ansible_user: banner
      ansible_password: BigGr33n
  vars:
    ansible_connection: ssh
```

```bash
# Test inventory
ansible -i inventory all -m ping # Ok

# Create playbook
cat > playbook.yml
```

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Copy file.
      # https://docs.ansible.com/ansible/latest/modules/copy_module.html
      copy:
        src: /usr/src/sysops/index.html
        dest: /opt/sysops
```

```bash
# Run playbook
ansible-playbook -i inventory playbook.yml # Ok

# Check if file exists
ansible -i inventory all -m shell -a 'ls /opt/sysops' # Ok, done.
```
