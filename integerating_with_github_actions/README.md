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
