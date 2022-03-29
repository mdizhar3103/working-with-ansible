**Ansible Roles and Collection**
Default Downloads Locations:
- Current direction roles location
- /usr/share/ansible/roles
- /etc/ansible/roles

To override:
- Set the 'ANSIBLE_ROLES_PATH' environment variable
- Define 'role_path' in the 'ansible.cfg' file
- Use the --roles-path option for the ansible-galaxy command

```bash
ansible_galaxy install --roles-path ./roles <roles_name_from_ansible_galaxy>


# or with yaml
- name: deploy ntp
  hosts: all
  roles:
  - izhar.ntp    # to be fetched from ansible-galaxy

ansible-playbook file_name.yml
```

***Collections***
- New Delivery format: 'Collection'
- Possibly includes modules, plugins, roles, playbooks, etc.
- To get Collection:
    - Ansible Galaxy
    - Automation Hub from Red Hat
    - Private Automation Hub

```bash
ansible-galaxy collection list

# to install collection
ansible-galaxy collection install <name_from_ansible_galaxy>

# to install specific verion
ansible-galaxy collection install <name_from_ansible_galaxy>:==<version_number>
```

Collection Search Paths:
- Default Search paths
    - ~/.ansible/collections
    - /usr/share/ansible/collections
- To customize
    - Set the 'ANSIBLE_COLLECTIONS_PATHS' environment variable
    - Define 'collections_paths' in the 'ansible.cfg' file

```bash
[defaults]
collections_path = ~/.ansible/collections:/usr/share/ansible/collections:/etc/ansible/collections
```

***Installing Multiple Collections***
- We need to setup a 'requirements.yml' file to install multiple collections in one command.
- Supported keys: name, source, version, type

```bash
# in yml file
collections:
  - name: community.general
    source: https://galaxy.ansible.com
  - name: f5.nginx
    source: https://cloud.redhat.com/api/automation-hub/


ansible-galaxy install -r requirements.yml
```

**Using Collections in a Playbook**
Method1 - using Fully Qualified Collection Name per task
```yaml
- hosts: all
  tasks:
    - my_namespace.collection1.module_abc:
      option: value

    - my_namespace.collection2.module_xyz:
      option: value
```

Method 2: using Collections keyword
```yaml
- hosts: all
  collections:
    - my_namespace.collection1
    - my_namespace.collection2
  tasks:
    - module_abc:
      option: value

    - module_xyz:
      option: value
```
