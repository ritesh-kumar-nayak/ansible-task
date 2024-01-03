# ansible-task
for project task
Setup
=========

Ansible Setup On Azure VMs
================
First go to "Identity" section of Controller VM in azure portal
Trun on the system assigned and click on save
Then click on "Azure role assignments" under permission right there
Then click on Add role assignment (Preview) and give the "Contributor" role to that resource group

The Contributor role grants full access to resources within a scope, allowing the assigned identity to create, modify, and 
delete resources. While it provides flexibility for managing resources, it also introduces potential security risks if not carefully controlled.

Then Install Azure Collection on controller machine:
ansible-galaxy collection install azure.azcollection


Create an Ansible configuration(ansible.cfg) file if you don't already have one. 
You can place this file in the same directory as your playbooks or in /etc/ansible/ansible.cfg. Here's a simple example:

[defaults]
inventory = ./azure_rm.py
ansible_managed_identity_identity = <Managed Identity Client ID>
Replace <Managed Identity Client ID> with the Client ID of the Managed Identity associated with your Ubuntu machine.

Download and Configure Azure Dynamic Inventory Script: (not necessary always)

wget https://raw.githubusercontent.com/ansible-collections/azure/dev/plugins/inventory/azure_rm.py
chmod +x azure_rm.py

Test if the dynamic inventory script works by running:( Not necessary always)

./azure_rm.py --list

Configure SSH Key on Controller
================================
1)Generate the SSH key using: ssh-keygen

2) Distribute SSH Public Key to Managed Nodes: ssh-copy-id user@public/private_ip

Copy the SSH public key (~/.ssh/id_rsa.pub by default) from the Ansible controller to each managed node. 
You can use tools like ssh-copy-id or manually append the public key to the ~/.ssh/authorized_keys file on each managed node.

3) Create an Ansible inventory file with the below content and change the fields accordingly

[linux]
linux_vm ansible_host=10.0.0.5(Private IP of Linux managed vm)

[windows]
windows_vm ansible_host=10.0.0.6(Private IP of Windows Managed VM)

[windows:vars] (#Variables required for windows host)
ansible_user=ansible				(User can be different as well)
ansible_password=Azure$$%^#dyudg37		(Give the correct password that u created during launching the VM)
ansible_connection=psrp				( here the connection for windows is PSRP)
ansible_psrp_cert_validation=ignore
ansible_psrp_protocol=http

======================
Test the connection 
======================
ansible -m ping -i inventory web_servers
ansible -m win_ping -i inventory windows
