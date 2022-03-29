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
