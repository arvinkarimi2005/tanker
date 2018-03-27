# remote
## change
`git remote set-url origin repo_url`
## list
`git remote -v`

## fetch all branches
```bash 
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
```