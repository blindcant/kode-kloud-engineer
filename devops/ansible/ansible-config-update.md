To manage all servers within the stack using Ansible, the Nautilus DevOps team is planning to use a common sudo user among all servers. Ansible will be able to use this to perform different tasks on each server. This is not finalized yet, but the team has decided to first perform testing. The DevOps team has already installed Ansible on jump host using yum, and they now have the following requirement:


On jump host make appropriate changes so that Ansible can use yousuf as the default ssh user for all hosts. Make changes in Ansible's default configuration only —please do not try to create a new config.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Make sure root
sudo -s

# Set the default ssh user
# https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings
# https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-remote-user
vi /etc/ansible/ansible.cfg

```

```
## /etc/ansible/ansible.cfg contents added/updated
remote_user = yousuf
```
