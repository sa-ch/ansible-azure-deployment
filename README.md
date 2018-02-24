This ansible project demonstrates how to setup an environment on the Microsoft Azure cloud
and deploy virtual machines. The deployed VM's will be added to the /etc/hosts of the 
ansible servers and configured with the SSH keys of the ansible user to provide password 
less access for ansible. 

Requirements: 
- azzure account with valid subscription
- new generated application key for access
- ansible core or ansible tower installed

Installation On Ansible Tower
1.) Allow awx user to become root
    echo 'awx ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers.d/awx
2.) Add Azure Credentials to the AWX user
    cat ~awx/.azure/credentials
    [default]
    subscription_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    tenant: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    secret: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    client_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX

    # Alternatively, you can sett "Extra Values" in the Job
3.) Login to ansible and create a new project

    - Extra Values (replace content with azur credentials)
      vmname: srv001
      debug: 1
      subscription_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
      tenant: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
      secret: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
      client_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX


# AZURE ACCOUNT CREDENTIOALS
cat ~/.azure/credentials
[default]
subscription_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
tenant: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
secret: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
client_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX


# SETUP AZUR LOCATON WITH VIRT NETWORK AND STORAGE
ansible-playbook -e vmname=srv001 create_vm_instance.yml
ansible-playbook -e vmname=srv002 create_vm_instance.yml

ansible-playbook -e vmname=srv001 delete_vm_instance.yml
ansible-playbook -e vmname=srv002 delete_vm_instance.yml

ansible-playbook -e location_name=azure_westeurope build_azure_environment.yml
ansible-playbook -e location_name=azure_westeurope delete_web01.yml 
ansible-playbook -e location_name=azure_westeurope delete_web02.yml 
