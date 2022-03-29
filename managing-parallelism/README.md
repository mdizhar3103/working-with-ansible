### Managing Parallism in Ansible
- Performance becomes important for large fleets
- How Ansible works:
    - Parse and load inventory
    - Parse and load playbook
    - Start iterating
- Customizing default behaviour helps us:
    - Tune performance
    - Control execution order

**Setting the number of fork**
- In ansible.cfg file
- By passing it on the command line with '--forks <FORKS>'
```bash
[defaults]
...
forks = 1
```

**Serial**
- Limits the number of hosts affected by a play at a given time
- Allows to control the seize of a rolling update window
- Set in the play header
- We can set a number, a percentage, or a list of numbers of hosts
