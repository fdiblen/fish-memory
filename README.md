# Fish memory

Tips &amp; Tricks to help my fish memory and not to google the same stuff over and over again.

## Networking

### List active tcp ports

```
watch ss -antlp
```

## Shell

### Find and replace in many files

```
grep -iR 'search' | xargs sed -i 's/search/replace/g'
```

## Git

### Undo last commit

[Source](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git)

```bash
$ git commit -m "Something terribly misguided" # (0: Your Accident)
$ git reset HEAD~                              # (1)
<< edit files as necessary >>                  # (2)
$ git add .                                    # (3)
$ git commit -c ORIG_HEAD                      # (4)
```

### Merge branches

```bash
git checkout master
git merge dev
git push origin master
```

### Push to different remote branch

```bash
git push origin master:newBranch
```

### Fetch a file/folder from diffrent branch

```bash
git checkout <branch_name> -- <paths>
```

To get package.json from dev branch to current branch:
```bash
git checkout dev -- package.json
```

### Commit current changes to different (existing )branch

```bash
git stash
git checkout other-branch
git stash pop
```

The first stash hides away your changes (basically making a temporary commit), and the subsequent stash pop re-applies them. 

### Commit current changes to new branch

```bash
git stash
git checkout -b new-branch
git stash pop
```

### Remove folder from entire git history

```bash
git filter-branch -f --tree-filter "rm -rf FOLDERNAME" --prune-empty HEAD
git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
echo FOLDERNAME/ >> .gitignore
git add .gitignore
git commit -m "Removing FOLDERNAME from git history"
git gc
git push origin master --force
```

### Sign commits by default

```bash
git config --global user.signingkey <YOUR_SIGNING_KEY>
git config --global commit.gpgsign true
git config --global gpg.program gpg
```

## GPG

### List keys

```bash
gpg --list-secret-keys --keyid-format LONG
```

### Export key

```bash
gpg --armor --export 189ABH19KLDU83
```

## SSH(FS)

### Mounting a remote system:

```bash
sshfs -p 22 -C -o follow_symlinks,auto_cache,reconnect ~/MOUNT_FOLDER USERNAME@SERVERIP:/home/USERNAME
```

### Save ssh passphrase in (gnome)keyring

Install seahorse

```bash
sudo pacman -S seahorse
```

```bash
/usr/lib/seahorse/ssh-askpass ~/.ssh/id_rsa
```

### SSH password auth

```bash
user=test
useradd -m -d /home/$user -s /bin/bash $user
ssh-keygen -b 2048 -t rsa -f ./$user'_sshkey' -q -N ""
mv $user'_sshkey' $user'_sshkey'.private
mkdir -p /home/$user/.ssh
cat $user'_sshkey'.pub > /home/$user/.ssh/authorized_keys
chown -R $user:$user /home/$user/.ssh
chmod 600 /home/$user/.ssh/authorized_keys
chmod 700 /home/$user/.ssh
cat $user'_sshkey'.private
```

## Docker

### Run GUI apps

```bash
xhost +si:localuser:root
```

```bash
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

## Linux iso --> bootable USB drive

```bash
dd bs=4M if=path/to/distro.iso of=/dev/sdX status=progress oflag=sync
```

## GPUs

### NVIDIA devices

```bash
docker run --rm -ti --runtime=nvidia nvidia/cuda nvidia-smi
```

### PyTorch

```bash
docker run -it --rm --runtime=nvidia --shm-size=1g -e NVIDIA_VISIBLE_DEVICES=0,1 nvcr.io/nvidia/pytorch
```

### Tensorflow

```bash
docker run \
    --runtime=nvidia \
    --rm \
    -ti \
    -v "${PWD}:/app" \
    gcr.io/tensorflow/tensorflow:latest-gpu \
    python /app/benchmark.py cpu 10000
```

### OpenGL

```bash
xhost +si:localuser:root
docker run --runtime=nvidia -ti --rm -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix nvcr.io/nvidia/pytorch
```

## Deployment

### Heroku

```bash
docker build --network=host -t spot .
```

#### New image

```bash
npm install -g heroku
heroku login
heroku container:login
heroku create
heroku container:push spot
heroku container:release spot
heroku open
```

#### Exisiting image

#docker tag <image> registry.heroku.com/<app>/<process-type>

docker tag nlesc/spot registry.heroku.com/nlesc-spot/dev   

#docker push registry.heroku.com/<app>/<process-type>

docker push registry.heroku.com/nlesc-spot/dev


## Python

### Http server

```bash
python3 -m http.server --directory ./ 8080
```

### Pyenv installation (Archlinux)

```bash
yay -S pyenv
pyenv install --list
pyenv install 3.7.2
pyenv global 3.7.2
pip install --upgrade pip
```

### Virtual environment

#### venv

```bash
python3 -m venv .venv
. .venv/bin/activate.fish # fish shell
. .venv/bin/activate # bash
pip install --upgrade pip
```

#### pipenv

```bash
pip install -U pipenv
cd my_project
pipenv install
```
[Quick tutorial](https://hackernoon.com/reaching-python-development-nirvana-bb5692adf30c?gi=1588ba978f78)

#### Use virtualenvironment in Visual studio code

[https://code.visualstudio.com/docs/python/environments](https://code.visualstudio.com/docs/python/environments)

#### Clear variable on IPython/Jupyter

```python
%reset
%reset_selective variable_name
```

#### Upgrade outdated Python packages

```bash
pip install $(pip list --outdated | awk 'NR>2 { print $1 }') --upgrade
```

#### Anaconda

##### Create an environment

```bash
conda create -n nso python=3.7 pandas
```

##### Export environment 

```bash
conda env export -c conda-forge -n nso --override-channels | grep -v "^prefix: "  > environment.yml
```

##### List environments

```bash
conda env list
```

##### Create an environment from a file

```bash
conda env create -f environment.yml
```

##### Deactivate an environment

```bash
conda deactivate
```

##### Remove the environment

```bash
conda env remove -n nso
```

## Nodejs

### Update outdated packages

```bash
npm i -g npm-check-updates && ncu -u && npm i
```

## GNOME

### Set Nautilus as default file manager

```bash
xdg-mime default org.gnome.Nautilus.desktop inode/directory
```

### add Desktop bookmark to nautilus (files)

```bash
gsettings set org.gnome.desktop.background show-desktop-icons true
gsettings set org.gnome.desktop.background draw-background true
```

## Xterm

Start xterm with a proper font.

```bash
xterm -fa 'Monospace' -fs 15
```

## UFW

```bash
sudo ufw enable
sudo ufw default deny
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22
sudo ufw logging on
sudo ufw status
```

## Systemd

### list targets

```bash
systemctl list-units -t target --all
```

### get default target

```bash
systemctl get-default
```

### see what target needs

```bash
systemctl show -p "Wants" graphical.target
```

### show enabled services

```bash
systemctl list-unit-files --state enabled
```

### show failed services

```bash
systemctl list-units --failed
```

### show logs of a service

```bash
journalctl -u optimus-manager.service
```
