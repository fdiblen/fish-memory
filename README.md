Tips &amp; Tricks to help my fish memory and not to google the same stuff over and over again

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
sudo ufw allow 443/tcp
sudo ufw allow 1725/udp
sudo ufw allow 80/tcp
sudo ufw allow 22
sudo ufw logging on
sudo ufw status
```
