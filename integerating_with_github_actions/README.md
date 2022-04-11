### Integrating Ansible with Github Actions
```bash
# Create a directory in project folder
mkdir -p .github/workflows

# creating a file
vim .github/workflows/web-deployment.yml

git add .github
git status
git commit -m "added initial github workflow"
git push origin main

git add -A
git commit -m "adding playbook, inventory, index.php files"
git push origin main

# Now goto this github repo and run the workflow
```

**Running the Workflow on every push**
```bash
# edit web-deployment.yml and add the following changes
on: 
  workflow_dispatch:
  push:
    branches:
      - main

git add -A
git commit -m "run workflow on pushed to main branch"
git push origin main
```

equest Protection Rule**
```bash
1. go to repo settings
2. Select Branches and click on add rule
3. specify branch or branches (example: main)
4. Select specific rules (example: check the require pull request reviews before merging, include administrators)


# checking the setting
# modify a file
git add -A
git commit -m "PR setting check"
git push origin main
    # if it fails then setting is successfull

# Opening Pull request for merging, by creating new branch
git checkout -b izhar/repo-pr-setting-check
git push
    # if upstream is not set this command will give a git command to setup upstream and then push again after running that command

# Now go to github and open pull request with necessary comments
# The repo owner will review the changes and approve the request

# Incase of workflow failure on owner side need to add some more rule
# 1. repo settings and edit rule
# 2. Enable rule: Require status check to pass before merging and in search bar, search for our workflow (jobs: in workflow, example: Lint Ansible Files)
# 3. Save the changes

# Now again modify the workflow file to run on every pull request
on: 
  workflow_dispatch:
  push:
    branches:
      - main
  pull_request:

# to be commited on newly created branch
git add -A
git commit -m "run workflow on pull request"
git push origin main
# Go to github and open pull request
```

