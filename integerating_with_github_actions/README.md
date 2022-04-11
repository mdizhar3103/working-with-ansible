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


