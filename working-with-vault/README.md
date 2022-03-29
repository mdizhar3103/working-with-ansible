### Working with Ansible Vault
- Provides native encryption capabilites
- Encrypts snsitive data at rest
- Encrypted content can be source controlled
- Encrypts files and variables

**File Encryption with Vault**
- The full file is encrytped
- The file can contain Ansible variables or any other content:
    - Inventory
    - Inventory host/group variables
    - vars/defaults in roles
    - Tasks files
    - Handlers files

```bash
ansible-vault create vaultfile1.yml
    >>> prompt to enter vault-password and confirm password
    # add your sensitive information
    db_user: izhar

# editing the created file
ansible-vault edit vaultfile1.yml
    >>> enter the password and add new content 
    db_pwd: mysecret

# encrypting the existing file
ansible-vault encrypt existing_file_name
    >>> enter vault password

# viewing encrypted file
ansible-vault view encrytped_file_name
    >>> enter vault password

# decrypting file
ansible-vault decrypt encrytped_file_name
    >>> enter vault password

# creating password for vault-file
ansible-vault rekey vaultfile1.yml
    >>> enter vault password
    >>> enter new-vault password and confirm password

```

**Adding vault_password_file in ansible.cfg**
```bash
[defaults]
....
vault_password_file = ./working-with-vault/.vaultpass
```

**Managing Multiple Passwords with Vault IDs**
- Using Vault-IDs helps differentiate between different passwords
- Helps avoid password sharing among team members
- To pass vault ID as an option:
    - '--vault-id label@source'
    - '--encrypt-vault-id label@source'
- Label is arbitarily chosen
- Source can be a prompt, a file, or a script

**Adding vault_id in ansible.cfg**
```bash
[defaults]
....
vault_identity_list = dev@./working-with-vault/.dev_pass , prod@./working-with-vault/.prod_pass


>>> echo 'devpass' > ./working-with-vault/.dev_pass
>>> echo 'prodpass' > ./working-with-vault/.prod_pass

# creating vault id for dev, ansible will automatically fetch the password from ansible.cfg specified path of label
ansible-vault create --encrypt-vault-id dev working-with-vault/vault_dev.yml
    # adding the varaible data or data in this file
    dev_variable: "Dev Rocks!"

ansible-vault create --encrypt-vault-id prod working-with-vault/vault_prod.yml
    prod_variable: "Prod Rocks!"
```
