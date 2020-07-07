One of the Nautilus DevOps team member was working on to test an Ansible playbook on jump host. However he was only able to create the inventory so far and due to some other priorities that came in he has to work on some other tasks. Please pick this task from where he left and complete the same. Below are more details about the task:


The inventory file /home/thor/ansible/inventory seems to be having some issues please fix the same. The playbook needs to be run on App Server 3 in Stratos DC so inventory file needs to be updated accordingly.

Create a playbook /home/thor/ansible/playbook.yml and add a task to create an empty file /tmp/file.txt on App Server 3.

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create ssh key and deploy to stapp03
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03

# Change to working directory
cd ~/ansible

# Inspect inventory, it was for stapp02 and was wrong
cat inventory

# Get stapp03 ip
cat /etc/hosts

# Update inventory
vi inventory
```

```yaml
# YAML format
all:
  hosts:
    stapp03:
  vars:
    ansible_user: banner
    ansible_connection: ssh
```

```
# INI format
stapp03 ansible_host=172.16.238.12 ansible_connection=ssh ansible_user=banner
```

```bash
# Test inventory
ansible -i ./inventory all -m ping

# Create playbook
vi playbook.yml
```

```yaml
---
- hosts: all

  tasks:
    - name: Create empty file.
      file:
        path: /tmp/file.txt
        state: touch
```

```bash
# Run playbook
ansible-playbook -i ./inventory playbook.yml

# Check results
ssh banner@stapp03
ll /tmp # Saw file.txt, OK
```
