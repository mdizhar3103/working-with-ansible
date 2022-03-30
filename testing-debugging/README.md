### Testing and Debugging Using Ansible
Using Command line options:
```bash
ansible-playbook --syntax-check playbook_name.yml

# Dry Run
ansible-playbook --check playbook_name.yml
```
**Note:** Not all modules fully support check mode:
- Task using modules that support check mode will report what changes would have made
- Task using modules that do not support check mode will also take no action, but will not report what changes they might have made

__Check Mode: Checking State__
- We can configure a task to always run in check mode: set check_mode: yes on the task
- In this case, we run the play normally and that task still runs in check mode
- If the task runs and it would normally change the remote system, the task will report CHANGED but not change anything
- We can register a varaible and react to the result without changing the managed host

> Maximum 5 verbosity level we can have -vvvvv

### Using Test Modules
- __Debug Module:__ Prints a message or a variable to the Ansible output. The debug module is useful for debugging varaibles or expressions
    - msg: This paramter is used to print custom messgag
    - var: A variable to print. No need for {{ }}. Mutually exclusive with msg
    - verbosity: Max level of verbosity is 3, -vvv.

- __Assert Module:__ Assert module checks that the given expressions are true with an optional custom message. The assert module is useful for validating your variables and conditions are what you expect.
    - that: A list of string expressions, same format as ***when*** statements
    - quiet: A boolean to avoid verbose output
    - success_msg: A custom message used when the asserted expression pass
    - fail_msg: A custom message used when the asserted expression fail

- __Fail Module:__ Fail module forces the Ansible playbook to fail with a custom message. The fail module is useful when testing certain conditions with when
    - msg: A custom message to be printed for failing the playbook

- __Meta Tasks:__ Meta tasks are special tasks that can influence Ansible internal execution or state. Meta tasks can be used anywhere within the playbook. Meta is not actually a module or an action plugin, so it can't be overwritten or used in loops.
    - clear_facts: Remove the gathered facts for the hosts specified in the play's list of hosts, including removing the facts from the fact cache.
    - clear_host_errors: Clears the failed state from hosts specified in the play's list of hosts

-----
### Playbook Debugger
- Ansible includes a debugger as part of the strategy plugins.
- We can check or set the value of variables, update module arguments, and re-run a task with new variables and arguments to help resolve the cause of a failure
- The debugger can be **enabled in ansible.cfg** and also can be set using environment variable: **ANSIBLE_ENABLE_TASK_DEBUGGER=True**
- Playbook Debugger Keywords:
    - always: Always invoke the debugger, regardless of the outcome
    - never: Never invoke the debugger, regardless of the outcome
    - on_failed: Only invoke the debugger ig the task fails
    - on_unreachable: Only invoke the debugger if the host was unreachable.
    - on_skipped: Only invoke the debugger if the task is skipped.
- Debugger can be set at:
    - task level
    - play level
    - both mixed level and more specific wins
- __Debugger commands__
    - p: Print values used to execute a module
    - task.args[key] = value: Update module's arguments
    - task_vars[key] = value: Update the vars used in the tasks
    - u(pdate task)
    - r(edo): run the task again
    - c(ontinue): Continues the playbook execution
    - q(uit): Quit from the debugger. The playbook execution is aborted.

```bash
[defaults]
enable_task_debugger = True
```

> See ansible-lint documentation for working with this tool
