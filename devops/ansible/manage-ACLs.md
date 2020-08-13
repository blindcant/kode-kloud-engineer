There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only; however, they also want that app-specific user to have a set of permissions to these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

1. Create a playbook.yml under /home/thor/ansible on jump host, an inventory file is already present under /home/thor/ansible on Jump Server itself.

2. Create an empty file blog.txt under /opt/finance/ directory on app server 1. Set some acl properties for this file. Using acl provide read '(r)' permissions to group tony (i.e entity is tony and etype is group).

3. Create an empty file story.txt under /opt/finance/ directory on app server 2. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to user steve (i.e entity is steve and etype is user).

4. Create an empty file media.txt under /opt/finance/ on app server 3. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to group banner (i.e entity is banner and etype is group).

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.

# Master

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Test inventory
cd ~/ansible
ansible -i inventory all -m ping # Ok

# Create playbook
cat > playbook.yml
```

```yaml
---
- tasks:
  - name: stapp01 block
    block:
      - name: Touch a file.
        # https://docs.ansible.com/ansible/latest/modules/file_module.html
        file:
          path: /opt/finance/blog.txt
          state: touch
          owner: root
          group: root

      - name: Set file ACL
        # https://docs.ansible.com/ansible/latest/modules/acl_module.html
        acl:
          path: /opt/finance/blog.txt
          entity: tony
          etype: group
          permissions: r
          state: present
  hosts: stapp01
  become: yes

- tasks:
  - name: stapp02 block
    block:
      - name: Touch a file.
        # https://docs.ansible.com/ansible/latest/modules/file_module.html
        file:
          path: /opt/finance/story.txt
          state: touch
          owner: root
          group: root

      - name: Set file ACL
        # https://docs.ansible.com/ansible/latest/modules/acl_module.html
        acl:
          path: /opt/finance/story.txt
          entity: steve
          etype: user
          permissions: rw
          state: present
  hosts: stapp02
  become: yes

- tasks:
  - name: stapp03 block
    block:
      - name: Touch a file.
        file:
          path: /opt/finance/media.txt
          state: touch
          owner: root
          group: root

      - name: Set file ACL
        acl:
          path: /opt/finance/media.txt
          entity: banner
          etype: group
          permissions: rw
          state: present
  hosts: stapp03
  become: yes
```

```bash
# Run the play
ansible-playbook -i inventory playbook.yml

# Check the results
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

# Nodes
```bash
# stapp01
ll /opt/finance/blog.txt # Ok, owned by root:root
getfacl /opt/finance/blog.txt  # Ok, facl with group:tony r

# stapp02
ll /opt/finance/media.txt # Ok, owned by root:root
getfacl /opt/finance/media.txt  # Ok, facl with user:steve rw

# stapp03
ll /opt/finance/story.txt # Ok, owned by root:root
getfacl /opt/finance/story.txt  # Ok, facl with group:banner rw
```
