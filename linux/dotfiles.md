## packets for fresh system
### flatpak:
```bash
sudo apt install flatpak gnome-software-plugin-flatpak
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
### black ports repo:
```bash
sudo vim /etc/apt/sources.list.d/backports.list
```
```
deb http://deb.debian.org/debian bookworm-backports main
```
### fonts:
```bash
sudo apt install fonts-jetbrains-mono
fc-cache -fv
```
### starship prompt:
```bash
curl -sS https://starship.rs/install.sh | sh
echo 'eval "$(starship init bash)"' >> ~/.bashrc
```
### useful programms:
```bash
sudo apt install -y git exa htop
```