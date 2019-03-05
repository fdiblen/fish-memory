Tips &amp; Tricks to help my fish memory and not to google the same stuff over and over again
# Git
## Merge branches
```
git checkout master
git merge dev
git push origin master
```

## Push to different remote branch
```
git push origin master:newBranch
```

# Python

## Pyenv (Archlinux)
```
yay -S pyenv
pyenv install --list
pyenv install 3.7.2
pyenv global 3.7.2
pip install --upgrade pip
```

## Virtual environment
### venv
```
python3 -m venv .venv
. .venv/bin/activate.fish # fish shell
```

### pipenv
```
pip install -U pipenv
cd my_project
pipenv install
```
[Quick tutorial](https://hackernoon.com/reaching-python-development-nirvana-bb5692adf30c?gi=1588ba978f78)



# GNOME
## Set Nautilus as default file manager
```
xdg-mime default org.gnome.Nautilus.desktop inode/directory
```

# Xterm
Start xterm with a proper font.
```
xterm -fa 'Monospace' -fs 15
```

# UFW
```
sudo ufw enable
sudo ufw default deny
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22
sudo ufw logging on
sudo ufw status
```
