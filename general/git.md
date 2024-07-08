### fixing mistakes
merge main before pushing code:
```bash
git checkout main
git pull origin main
git checkout my-branch
git merge main
```

undo local commit, but keep changes in files and staged status:
```bash
git reset --soft HEAD~1
```

`--mixed` keep changes in files and unstage status
`--hard` discard all changes, also in files

update latest commit message:
```bash
git commit --amend -m "correct"
```

view git log in nice short format:
```bash
git log --graph --oneline --decorate
```

switch to previous branch:
```bash
git checkout -
```

discard all local changes in a file, restore deleted file:
```bash
git restore index.html
# Discard all local changes:
git restore .
# Reset a speicfic file to an old version:
git restore --source <hash> index.html
```

discard all local changes, that are not commited yet (can't be undone):
```bash
git checkout .
```

discard chunks or lines in a file (interactive):
```bash
git restore -p index.html
```

revert commit in the middle with opposit changes (creates new commit):
```bash
git revert <hash>
```

recover deleted branch:
```bash
git reflog
git branch develop <hash>
```

moving commit to a new branch (which didn't exist):
```bash
git branch feature/login
# Clean up main:
git reset --hard HEAD~1
```

moving commit to a different branch (which existed before):
```bash
git checkout feature/newsletter
git cherry-pick <hash>
# Clean up main:
git checkout main
git reset --hard HEAD~1
```

### branches
create new branch:
```bash
git branch my-new-branch
# Based on specific revision:
git branch other-branch <hash>
```

change branch:
```bash
git checkout my-new-branch
# Modern way:
git switch other-branch
```

rename current HEAD branch:
```bash
git branch -m better-branch
# Rename any other local branch:
git branch -m old-branch new-branch
```

upload a local branch to remote:
```bash
git push -u origin local-branch
```

connect branches with each other (local and remote):
```bash
git branch --track feature origin/feature
git checkout --track origin/feature
```

sync local and remote branches:
```bash
git pull
# Always try git pull --rebase first, if you get merge conflict, you can undo everything with git rebase --abort and try with regular pull instead to solve that conflict
git push
# Check status:
git branch -v
```

delete branch:
```bash
# Local:
git branch -d feature
# Remote:
git push origin --delete feature
```

integrate changes from another branch into your current local HEAD branch:
```bash
git switch main
git merge feature/uploader
# Using rebase:
git switch feature/uploader
git rebase main
```

check which commits are in branch-2, but not in branch-1:
```bash
# Local:
git log main..feature/uploader
# Remote:
git log origin/main..main
```