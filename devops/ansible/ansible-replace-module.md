There is data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Create a playbook.yml under /home/thor/ansible on jump host; an inventory is already place under /home/thor/ansible on Jump host itself.

1. We have a file /opt/finance/blog.txt on app server 1. Using Ansible replace module replace string xFusionCorp to Nautilus in that file.

1. We have a file /opt/finance/story.txt on app server 2. Using Ansiblereplace module replace string Nautilus to KodeKloud in that file.

1. We have a file /opt/finance/media.txt on app server 3. Using Ansible replace module replace string KodeKloud to xFusionCorp Industries in that file.

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
ansible -i inventory stapp01 -m shell -a 'fgrep xFusionCorp /opt/finance/blog.txt'
ansible -i inventory stapp02 -m shell -a 'fgrep Nautilus /opt/finance/story.txt'
ansible -i inventory stapp03 -m shell -a 'fgrep KodeKloud /opt/finance/media.txt'

# Create playbook
cat > playbook.yml
```

```yaml
# YAML format
---
- name: Update strings.
  hosts: stapp01
  become: yes
  tasks:
    - name: Replace /opt/finance/blog.txt
      replace:
        path: /opt/finance/blog.txt
        regexp: 'xFusionCorp'
        replace: 'Nautilus'

- name: Update strings.
  hosts: stapp02
  become: yes
  tasks:
    - name: Replace /opt/finance/story.txt
      replace:
        path: /opt/finance/story.txt
        regexp: 'Nautilus'
        replace: 'KodeKloud'

- name: Update strings.
  hosts: stapp03
  become: yes
  tasks:
    - name: Replace /opt/finance/media.txt
      replace:
        path: /opt/finance/media.txt
        regexp: 'KodeKloud'
        replace: 'xFusionCorp Industries'
```

```bash
# Run playbook
ansible-playbook -i ./inventory playbook.yml

# Check results
ansible -i inventory stapp01 -m shell -a 'fgrep Nautilus /opt/finance/blog.txt'
ansible -i inventory stapp02 -m shell -a 'fgrep KodeKloud /opt/finance/story.txt'
ansible -i inventory stapp03 -m shell -a 'fgrep 'xFusionCorp Industries' /opt/finance/media.txt'
```
