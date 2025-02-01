install a package:
```bash
pacman -S neovim
```

refresh package repository:
```bash
pacman -Sy
```

upgrade packages:
```bash
pacman -Syu
# Force update:
pacman -Syyu
```

remove a package (with config files and dependencies):
```bash
pacman -Rns
```

search for packages:
```bash
pacman -Ss zsh
```

generate list of installed packages:
```bash
pacman -Q | grep firefox
# List with descriptions:
pacman -Qs | less
```

remove dependencies that are no longer needed:
```bash
pacman -Qdtq | pacman -Rs -
```

get back to previous version of the package:
```bash
sudo downgrade firefox
```

package which automatically cleanups the pacman package cache:
```bash
yay -S paccache-hook
```

update AUR packages only:
```bash
yay -Sua
```

remove a package:
```bash
yay -Rnsc tldr
```