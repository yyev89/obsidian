merge main before pushing code:
```bash
git checkout main
git pull origin main
git checkout my-branch
git merge main
```

undo local commit, but keep changes in files:
```bash
git reset --soft HEAD~1
# Update latest commit message:
git commit --amend -m "new stuff"
```

view git log in nice short format:
```bash
git log --graph --oneline --decorate
```

switch to previous branch:
```bash
git checkout -
```
