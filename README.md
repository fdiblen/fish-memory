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

## Fetch a file/folder from diffrent branch
```
git checkout <branch_name> -- <paths>
```

To get package.json from dev branch to current branch:
```
git checkout dev -- package.json
```


## Remove folder from entire git history

```
git filter-branch -f --tree-filter "rm -rf FOLDERNAME" --prune-empty HEAD
git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
echo FOLDERNAME/ >> .gitignore
git add .gitignore
git commit -m "Removing FOLDERNAME from git history"
git gc
git push origin master --force
```

# SSH(FS)

## Mounting a remote system:

```
sshfs -p 22 -C -o follow_symlinks,auto_cache,reconnect ~/MOUNT_FOLDER USERNAME@SERVERIP:/home/USERNAME
```

## Save ssh passphrase in (gnome)keyring

```
/usr/lib/seahorse/ssh-askpass ~/.ssh/id_rsa
```


# Docker

## Run GUI apps

```
xhost +si:localuser:root
```

```
docker run --rm -it --net=host \
--privileged \
-e DISPLAY \
--device /dev/dri \
--device /dev/snd \
--device /dev/video0 \
--device /dev/input \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v /etc/localtime:/etc/localtime:ro \
-v $(pwd):/app \
ubuntu /bin/bash
```

## Linux USB disk
```
dd bs=4M if=path/to/distro.iso of=/dev/sdx status=progress oflag=sync
```

## NVIDIA

### NVIDIA devices
```
docker run --rm -ti --runtime=nvidia nvidia/cuda nvidia-smi
```

### PyTorch
```
docker run -it --rm --runtime=nvidia --shm-size=1g -e NVIDIA_VISIBLE_DEVICES=0,1 nvcr.io/nvidia/pytorch
```

### Tensorflow
```
docker run \
    --runtime=nvidia \
    --rm \
    -ti \
    -v "${PWD}:/app" \
    gcr.io/tensorflow/tensorflow:latest-gpu \
    python /app/benchmark.py cpu 10000
```

### OpenGL
xhost +si:localuser:root
docker run --runtime=nvidia -ti --rm -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix nvcr.io/nvidia/pytorch


# Deployment

## Heroku

docker build --network=host -t spot .

### New image

npm install -g heroku
heroku login
heroku container:login
heroku create
heroku container:push spot
heroku container:release spot
heroku open

### Exisiting image

#docker tag <image> registry.heroku.com/<app>/<process-type>

docker tag nlesc/spot registry.heroku.com/nlesc-spot/dev   

#docker push registry.heroku.com/<app>/<process-type>

docker push registry.heroku.com/nlesc-spot/dev




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
. .venv/bin/activate # bash
pip install --upgrade pip
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
## add Desktop bookmark to nautilus (files)
```
gsettings set org.gnome.desktop.background show-desktop-icons true
gsettings set org.gnome.desktop.background draw-background true
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
