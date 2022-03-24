### Custom Facts in Ansible
- User can create custom facts which are stored locally on each managed hosts
- Custom facts can defined in a static file, formatted as an INI file or using JSON. They can be executable scripts which generate JSON output, just like a dynamic inventory script
- ***By default setup loads custom facts file from each managed hosts /etc/ansible/facts.d directory. The name of each file must end in .fact in order to be used.***
- *Custom facts are stored by setup in the ansible_local variable.*

**Example of static custom facts in INI format**
```bash
# vim /etc/ansible/facts.d/custom.fact
[packages]
web_package = httpd
db_package = mariadb-server

[users]
user1 = joe
user2 = jane

# to get value of custom fact
ansible_local['custom']['users']['user1']
```

