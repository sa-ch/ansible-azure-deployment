This ansible project demonstrates how to setup an environment on the Microsoft Azure cloud
and deploy virtual machines. The deployed VM's will be added to the /etc/hosts of the 
ansible servers and configured with the SSH keys of the ansible user to provide password 
less access for ansible. 

Requirements: 
- azzure account with valid subscription
- new generated application key for access
- ansible core or ansible tower installed

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

