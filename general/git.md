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