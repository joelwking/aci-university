# aci-university
Training environment for WWT ACI University students. It creates a virtual machine with Ansible installed along with a sample playbook, Jinja template and XML file to create a tenant in an ACI fabric and add subnets to a bridge domain.

## Instructions
Create a directory called aci-university, enter it, and use an editor to place the contents of the Vagrantfile from https://github.com/joelwking/aci-university to a work directory on your laptop.  If you have git installed on your laptop, you can create a work directory, enter it and clone the repository.
```
git clone https://github.com/joelwking/aci-university.git
```

Issue the command `vagrant up` to bring up the virtual machine, then `vagrant ssh` to logon.

Enter the working directory:
```
cd aci-university/ansible/playbooks/
```
Modify the inventory file and to reference your training ACI fabric name or IP address
```
sudo vi hosts
```
Modify the passwords.yml file to use your training fabric username and password
```
sudo vi passwords.yml
```
Encrypt the passwords.yml file
```
sudo andible-vault encrypt passwords.yml
sudo cat passwords.yml
```
Review the playbook 
```
sudo vi jinja2_loop.yml
```
Review the Jinja template file in ./templates
```
cat ./templates/*
```
Review the XML file used to create the tenant
```
cat ./files/*
```
Run the playbook to create a tenant and update the subnets in the bridge domain, you 
will be prompted for the vault password used to encrypt the file.
```
sudo ansible-playbook jinja2_loop.yml --ask-vault
```
The output should look like the following:
```
Vault password:

PLAY [Jinja2 Lab looping in a template] ****************************************

TASK [setup] *******************************************************************
ok: [aci-demo.sandbox.wwtatc.local]

TASK [Decrypt the password file] ***********************************************
ok: [aci-demo.sandbox.wwtatc.local]

TASK [Display the Tenant name] *************************************************
ok: [aci-demo.sandbox.wwtatc.local] => {
    "msg": "Creating tenant student_1486236944"
}

TASK [Create XML file  from a template to add subnets under a bridge domain] ***
changed: [aci-demo.sandbox.wwtatc.local]

TASK [Create the Tenant] *******************************************************
changed: [aci-demo.sandbox.wwtatc.local]

TASK [Add the subnets to the bridge domain] ************************************
changed: [aci-demo.sandbox.wwtatc.local]

TASK [Clone the Tenant "student_1486236944"] ***********************************
changed: [aci-demo.sandbox.wwtatc.local]

PLAY RECAP *********************************************************************
aci-demo.sandbox.wwtatc.local : ok=7    changed=4    unreachable=0    failed=0
```
Examine the file created from the Jinja template
```
cat ./files/*__student*
```
Logon the APIC and examine the Tenant, and the clone of the Tenant, you created.

Issue `exit` to terminate the ssh session and issue `vagrant destroy` to remove the virtual machine
from your environment.
