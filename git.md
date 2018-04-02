# remote
## add
```bash
git remote set-url origin --push --add <a remote>
```
## change
`git remote set-url origin repo_url`
## list
`git remote -v`
# fetch all branches
```bash
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
```
__
# stash
## not pushed commit
```bash
git reset --soft HEAD^
git stash
```
## pushed commit
### stash
```bash
git reset --soft HEAD^
git stash
git pull
git stash pop # Will cause a conflict
git commit    # Re-commit 7826b2
```
### cherrypick
```bash
git reset --hard HEAD^
git pull
git cherry-pick 7826b2 # Will cause a conflict
```
