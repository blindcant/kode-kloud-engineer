The Nautilus DevOps team has started testing their Ansible playbooks on different servers within the stack. They have placed some playbooks under /home/thor/playbook/ on jump host which they want to test. Some of these playbooks have already been tested on different servers, but now they want to test them on app server 3 in Stratos DC. However, they first need to create an inventory file so that Ansible can connect to the respective app. Below are some requirements:


a. Create an ini type Ansible inventory file /home/thor/playbook/inventory on jump host.

b. Add App Server 3 in this inventory along with required variables that are needed to make it work.

c. The inventory hostname of host should be the server name as per wiki, for example stapp01 for app server 1 in Stratos DC.

d. At the end /home/thor/playbook/playbook.yml, playbook should be able to run.

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Create ssh key and deploy to stapp03
ssh-keygen -t ed25519
ssh-copy-id -i ~/.ssh/id_ed25519 banner@stapp03

# Update inventory
cd /home/thor/playbook/ 
cat > inventory
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

# Run playbook
ansible-playbook -i ./inventory playbook.yml
```
