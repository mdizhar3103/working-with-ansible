## Integerating Ansible with Jira
- Create account on atlassian jira for free
- Create a project __my-first-project__ and keep the key generated safely.
```bash
# https://mdizhar3103.atlassian.net/
# Installing ansible jira plugin
ansible-galaxy collection install community.general
ls .ansible
cd .ansible
ls -l
cd collections/ansible_collections/
ls
cd community/general
ls -lc
cd plugins
ls -l
cd modules
ls -l
```

**Encrypting Jira Credentials using Ansible Vault**
```bash
vim secrets.yml
---
username: 
password: # dont use password instead use API token generated from JIRA account and paste it here.

ansible-vault encrypt secrets.yml
    # enter vault password

vi encrypt.txt              # save the vault password in file to be used in decrypting

#===================================
# In my case vault-id is configured so I am using the following method to setup
ansible-vault create --encrypt-vault-id prod jira-integeration/secrets.yml
```

**Creating Jira Issue with Ansible**
```bash
vim 00-first-jira-issue.yml

# Executing playbook
ansible-playbook 00-first-jira-issue.yml --vault-password-file encrypt.txt
ansible-playbook -vvv 00-first-jira-issue.yml --vault-password-file encrypt.txt
```

> Note: The reporter name in Jira will be used as administrator who created token
