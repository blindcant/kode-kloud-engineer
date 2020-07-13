Several new developers and DevOps engineers just joined xFusionCorp industries. They have been assigned the Nautilus project, and as per the usual initial process we need to create user accounts for new joinees on at least one app server in Stratos DC. We also need to create groups and make new users members of those groups. We need to accomplish this task using Ansible. Below is more information about the task.


There is already an inventory file ~/playbooks/inventory on jump host.

On jump host itself there is a list of users in ~/playbooks/data/users.yml file and there are two groups — admins and developers —that have list of different users. Create a playbook ~/playbooks/add_users.yml on jump host to perform the following tasks on app server 3 in Stratos DC.

a. Add all users given in users.yml file on app server 3.

b. Also add groups developers and admins on same server.

c. As per the list given in users.yml file, make each user member of the respective group they are listed under.

d. Make sure home directory for all users under developers group is /var/www. For admins make sure it is default.

e. Set password dCV3szSGNA for all users under developers group and BruCStnMT5 for users under admins group. Make sure to use the password given in ~/playbooks/secrets/vault.txt file as Ansible vault password to encrypt the original password strings. You can use ~/playbooks/secrets/vault.txt file as vault secret file while running the playbook (make necessary changes in ~/playbooks/ansible.cfg file).

f. All users under admins group must be added as sudo users. To do so, simply make them member of wheel group as well.

Note: Validation will try to run playbook using command ansible-playbook -i inventory add_users.yml so please make sure playbook works this way, without passing any extra arguments.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to application servers, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus

# Change to working directory
cd ~/playbooks

# Inspect inventory
cat inventory

# Test inventory
ansible -i inventoty all -m ping

# Update Ansible config file for Ansible Vault
cat >> ~/playbooks/ansible.cfg
```

```
# ~/playbooks/ansible.cfg contents added/updated
# The newline here is needed as the last command didn't have one.

vault_password_file = /home/thor/playbooks/secrets/vault.txt
```

```bash

# Generate the hashed password with a random salt
# https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module
ansible -i inventory localhost -m debug -a "msg={{ 'dCV3szSGNA' | password_hash('sha512') }}"
ansible -i inventory localhost -m debug -a "msg={{ 'BruCStnMT5' | password_hash('sha512') }}"

# Encrypt the hashed and salted password with our Ansible Vault file.
# https://docs.ansible.com/ansible/latest/user_guide/vault.html#use-encrypt-string-to-create-encrypted-variables-to-embed-in-yaml
ansible-vault encrypt_string '$6$MrpVh9an2d08s4j/$r0jUrIvoqI7IIX54rbg/JF4VgNaY/WjKA1mLHNzQ09yvljd2gOZUainiySms82Jji.zvmwmcKS8puQyHRR..d1'
ansible-vault encrypt_string '$6$WOdTWAt/yUbtUp7k$KURzSUaSAlHzgtIOWLIobuUcMoequAn.gPpEQEZjBxvraSydmE/SsFh4lCSz9FzL32x/rtFKtkCLebAiTPcSG/'

# Create playbook
vi add_users.yml
```

```yaml
---
- name: Add users and groups.
  hosts: stapp03
  become: yes
  gather_facts: yes

  tasks:
    # https://docs.ansible.com/ansible/latest/modules/group_module.html
    - name: Ensure the groups are present.
      group:
        name: "{{ item }}"
        state: present
      loop:
        - admins
        - developers

    # https://docs.ansible.com/ansible/latest/modules/user_module.html
    - name: Add developers.
      user:
        name: "{{ item }}"
        home: "/var/www/{{ item }}"
        groups: developers
        append: yes
        # https://stackoverflow.com/a/29910294
        password: "{{ user_password }}"
      loop:
        - tim
        - ray
        - jim
        - mark
      # https://stackoverflow.com/a/47567783
      vars:
        user_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31316134313531663062323032363765613039633231626162306637336361353437363362333565
          3731663966623161353464303332346631663561396435300a346632363733386339623337393261
          31393336343365363861613130656336663935376663636439653239646533633864353134363461
          3066623864613364380a653663353635643362323162633533613136636266323933373834383964
          35333066376239303632343936623464303863663635303530383537613763633866363235356533
          39343935643338326438656164323862623265633337653838663836653762396230343332303830
          63623033326163663439343239323938623938346131376632336434303536333233303266653939
          66306261316532666436323530306333393930366431623566646163656137353961333530386466
          39343663336264393962363966353632356261623832643939356236663639663065
      
    - name: Add admins.
      user:
        name: "{{ item }}"
        groups: admins,wheel
        append: yes
        password: "{{ user_password }}"
      loop:
        - rob
        - david
        - joy
      vars:
        user_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31663434626163396464633035346366616334373837383234646133633437336263616336643962
          6531363238303766363434373131396264666462316632330a363165653934626133346661306533
          31643061306465326465393039373666323936356238333634653734316334346432666331373766
          3165633362356565350a333834633765363066633165623633306264303838386431396462306337
          30643634396562646438623238663132373932646338316138333638626338386665656362336165
          35393633613230323730653538326637653736366164663633613236363332363862663435303632
          65666636343034353665626139643033323061366264393964643635343862643035616438636538
          62313862373131643236323230306537646336326263636638626639633662643863396262373864
          35326630396536636439633566303063336266636132393334643034616230373262

```

```bash
# Run playbook
ansible-playbook -i ./inventory playbook.yml

# Check results
ssh banner@stapp03

# Check developer password
su - tim # Ok, password dCV3szSGNA worked.

# Check admin password
su - rob # Ok, password BruCStnMT5 worked.

# Check admin can sudo
sudo -s # Ok, changed to root.

# Check home directories
ll /home # Ok, saw the expected admin users
ll /var/www # Ok, saw the expected developer users
```
