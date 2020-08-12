During their weekly meeting, the Nautilus DevOps team developed automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing with Ansible, so they have decided to use jump host as an Ansible controller to test different kinds of tasks on the rest of the servers.


Install ansible version 2.5.7 on Jump host.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Download old Ansible
sudo -s
yum install wget -y
wget https://releases.ansible.com/ansible/rpm/release/epel-7-x86_64/ansible-2.5.7-1.el7.ans.noarch.rpm

# Install old Ansible
rpm -i ansible-2.5.7-1.el7.ans.noarch.rpm
ansible --version # Ok
```
