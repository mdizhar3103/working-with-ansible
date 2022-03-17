## Working with Ansible
**Ansible Config file Precedence**
1. $ANSIBLE_CONFIG : This environment variable will override all other configuration files
2. If this variable is not set ansible will look for ansible.cfg in the directory where the command is run
3. If ansible.cfg is not found, it will look for .ansible.cfg
4. And if .ansible.cfg is not found it will check for global config file in /etc/ansible/ansible.cfg


```bash
grep "^\[" /etc/ansible/ansible.cfg

ansible all -m setup
ansible all -m setup -a "filter=ansible_os_family"
ansible all -m setup -a "filter=ansible_distribution"

# ansible-config command
ansible-config view
ansible-config list
ansible-config dump
ansible-config dump --only-changed

# Disabling the ANSIBLE_CONFIG variable
# In .bashrc file add the below line
declare -xr ANSIBLE_CONFIG=/etc/ansible/ansible.cfg

```

**Ansible Inventories**
```bash
ansible --list all
ansible --list ungrouped
ansible-inventory --list
ansible-inventory --list -y

# inventory variables
mkdir {host,group}_vars
echo "ansible_connection: local" > host_vars/webserver
ansible-inventory --host webserver
```

**Playbook Syntax Variations**
```bash
ansible-playbook <file_name> --syntax-check
```

Multi-Line String Syntax
- '|':  This vertical bar denotes that the newline characters within the string are to be preserved
- '>': Greater than character is used to indicate that newline character are to be converted to spaces and that leading white spaces are to be removed.

**Managing Variables**
