**Introduction**

This project contains a very rough playbook to automate the install of a set of KVM-qemu based Linux VM by using some publicly available cloud images via ansible.

I used these playbooks to help bootstrapping VM for my own testing (I know I can probably have some simpler code if I am going to faciliate public cloud instances via API, but I dont want to spend any money on public cloud as it is just my own lab.

**Limitation**

1. Currently it assume the base node (or I call it build node) that doing the orchestration as Fedora, as I am currently using Fedora 29 as the build node. Probably you will need to update the package dependency should you want this to be run under ubuntu
2. Currently the VM defined in the automation are all Ubuntu 18.04 VM. I think this can be modified to support Fedora /CentOS or whatever Linux when needed, probably just need to create another set of interface template for the guest VM.

**Files Structure**

```
The project contains these files
.
├── extra_vars.yml           - the variables definition files for the ansible automation code
├── handlers
│   └── main.yml             - contain the handlers for service restart 
├── main.yml                 - the main playbook
├── build_node_configure.yml - the playbook that being included in main.yml, build node will be configured by this play
├── vm_configure.yml         - the playbook that being included in main.yml, vm will be configured by this play
├── templates
│   ├── build_node_br0.j2    - the nic template file for the bridge interface on build node
│   └── ubuntu_iface.j2      - the nic template file for the interface on ubuntu VM
└── inventory.yml            - the ansible inventory file
```

**Usage**

1. git clone the project
2. update the inventory.yml for the sudo password (if you run the ansible-playbook as non-root user)
3. update the password in extra_vars.yml for the desired password of the VM
4. update the configuration of the VM profile from extra_vars.yml, also the list of VM in inventory.yml
5. run ansible-playbook main.yml -e @extra_vars.yml -i inventory.yml