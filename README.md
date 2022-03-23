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
ansible-inventory --graph 
ansible-inventory --graph --vars

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


**Creating a User with authorized keys**
```bash
ansible <host> --private-key pvt_key_name.key  -u user_name -m user -a "name=izhar state=present"

echo "izhar ALL=(ALL) NOPASSWD: ALL" > izhar
visudo -cf izhar

ansible <host> --private-key pvt_key_name.key  -u user_name -m copy -a "src=izhar dest=/etc/sudoers.d/"

ssh-keygen

ansible <host> --private-key pvt_key_name.key -u user_name -m authorized_key \
-a "user=izhar state=present key='{{ lookup('file','~/.ssh/id_rsa.pub') }} 


# add the private key path in ansible.cfg
# [defaults]
# ...
# private_key_file = ~/.ssh/id_rsa
```

> Note: See difference between Shell and Command Module, also see script module

```bash
ansible all -m command -a "echo hello > file1"
ansible all -m shell -a "echo hello > file1"

# redirection operation work with shell module
```

**Managing Variables**
- Using vars in yaml file
- Using folder group_vars

```bash
mkdir group_vars

vi group_vars/host_name_1
    apache_pkg: httpd
    apache_svc: httpd

vi group_vars/host_name_2
    apache_pkg: apache2
    apache_svc: apache2
```

***NOTE***
> All import* statements are pre-processed at the time playbooks are parsed.
> All include* statements are processed as they encountered during the execution of the playbook.
> **So import is static, include is dynamic.**


**Using Jinja2 Templates**
To get rid of newline in for loop use the following syntax
```bash
# the - will stop the for loop in printing new line
{% for loop goes here %}
 {{ data }}
{%- endfor %}
```
> Note: Templating is done on controller

**Working with Roles**
Roles structure:

```bash
|_ role_name
    |_ defaults
    |_ files
    |_ handlers
    |_ meta
    |_ tasks
    |_ templates
    |_ tests
    |_ vars
```
Creating Roles:
- manually create folders
- use ansible-galaxy

```bash
ansible-galaxy init <role_name>
```

***Configuring role in ansible.cfg***
```bash
# in current working directory where ansible.cfg resides
mkdir roles

vim ansible.cfg
    [defaults]
    ....
    roles_path = ./roles

ansible-galaxy init ntp  # creating role for ntp
```

Using Roles:
- At play level with the roles 'option'
- At the task level with __include_role__ (Dynamic reuse)
- At the task level with __import_role__ (Static reuse)
- Execution order
    - Roles used in the roles section run before other tasks in the play
    - Roles used with the 'include_roles' or 'import_roles' run in the order they are defined

**vars in roles have higher precendence than defaults**
