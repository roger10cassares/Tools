Install Gnome-Tweaks
From: https://linuxconfig.org/how-to-install-tweak-tool-on-ubuntu-22-04-lts-jammy-jellyfish-linux

```bash
sudo add-apt-repository universe
sudo apt install gnome-tweaks
gnome-tweaks
apt search gnome-shell-extension
sudo apt install $(apt search gnome-shell-extension | grep ^gnome | cut -d / -f1)
```

Install Mojave SUpport and Mac themes
From: https://linuxconfig.org/how-to-install-macos-theme-on-ubuntu-22-04-jammy-jellyfish-linux

```bash
sudo apt update
sudo apt install gtk2-engines-murrine gtk2-engines-pixbuf

mkdir -p ~/.icons
cp -r ~/Tools/Mac/Mac-Icons/Mojave-CT-Light ~/.icons
cp -r ~/Tools/Mac/Mac-Cursor/Mac-Cursor-Set ~/.icons

mkdir -p ~/.themes
cp -r ~/Tools/Mac/Mac-Icons/Mojave-light ~/.themes

sudo apt install plank
```

Then, insert Plank in gnome-tweaks startup application
