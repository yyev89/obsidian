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
```

discard chunks or lines in a file (interactive):
```bash
git restore -p index.html
```