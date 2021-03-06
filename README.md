# LinuxTricks
Record some ueful Linux Tricks that I have been using

## find all files modified within 24 hours 

```
find . -maxdepth 1 -mtime -1
```
```
find . -mtime -1
```


## fish config.fish 

```fish
set -gx PATH /home/myt007/miniconda3/bin $PATH

alias ta='tmux attach'
alias yg='you-get '
alias ytdl="youtube-dl --rm-cache-dir -i --no-check-certificate --merge-output-format mp4 --format 'bestvideo[ext=mp4]+bestaudio[ext=m4a]' --add-metadata --embed-thumbnail "
alias ytdlmax="youtube-dl --rm-cache-dir -i --no-check-certificate --merge-output-format mp4 --format 'bestvideo+bestaudio[ext=m4a]' --add-metadata --embed-thumbnail "

alias subl='sublime'
alias nvishow='watch -n1 nvidia-smi'
alias tree='tree -N'
alias blogserver='JEKYLL_ENV=production bundle exec jekyll serve -w'
alias atomblog='atom ~/blog'
alias filecount='find . | wc -l'
alias fcount='find . | wc -l'
alias ta='tmux attach'
alias brewup='brew update; brew upgrade; brew cleanup; brew doctor'

alias gits='git status'
alias aria2c='aria2c -x 16 -s 16 '
alias ca='source activate dl'
alias cadl='conda activate dl'
alias ipyserver='jupyter notebook --no-browser --ip=127.0.0.1 --port=8888'
alias ipyconvert='ipython nbconvert --to script '
alias kalicontainer='sudo docker run -it --net="host" --privileged kali:1 /bin/bash'

alias untar='sudo tar -xzvf'
alias update='sudo apt update; sudo apt upgrade -y; wget -O - https://raw.githubusercontent.com/laurent22/joplin/master/Joplin_install_and_update.sh | bash'
alias install='sudo apt-get install'

alias h265izehere='h265ize -v -m fast  -q 25 -x --no-sao --aq-mode 3 --stats --delete'
alias rmv='rsync --remove-source-files -avhP'
alias rcp='rsync -avhP'
alias ncduroot='sudo ncdu -x /'
alias minuimushere='find . -type f -print0 | xargs  -0 -P 8 -n 8 minuimus.pl'
alias dc='docker-compose '
```

## enhance image resolution 

### Setup the alias. Put this in your .bashrc or .zshrc file so it's available at startup.
```bash
alias enhance='function ne() { docker run --rm -v "$(pwd)/`dirname ${@:$#}`":/ne/input -it alexjc/neural-enhance ${@:1:$#-1} "input/`basename ${@:$#}`"; }; ne'
```

### Now run any of the examples above using this alias, without the `.py` extension.
```bash
enhance --zoom=1 --model=repair images/broken.jpg
```

## Create a python kernel that jupyter can perceive 

```bash
conda create -n gpu python pip ipykernel tensorflow-gpu
conda activate gpu
python -m ipykernel install --user --name gpu --display-name gpu
```

## Change multiple files with find and sed

```bash
find . -type f -name 'download.sh' -exec sed -i 's/youtube-dl /youtube-dl  --download-archive ytdl-archive-record.txt /g' {} \;
```

## Rsync directory with progress bar and without overwrite

```bash
rsync -r --ignore-existing --progress source dest
```

## Download Youtube Videos as Audios Files

- Install youtube-dl package
    - can install it with brew
- Use ```youtube-dl -F url ``` to check what format is available on youtube
- Useful options include --ignore-errors \ --no-playlist \ --yes-playlit \ --embed-thumbnail \ -x (output audio)

I use:
```bash
youtube-dl -f 140 --yes-playlist --embed-thumbnail --ignore-errors url
```
to download the playlist with specifying file format code as 140 (.m4a file).

If the file code 140 is not available, one might consider use -x option to force ffmpeg to convert an audio fril.
So iCloud Music Library doesn't support the aac and opus from youtube-dl. Maybe the mp3 format will work. So it does work. Here is the recommanded commands for mp3 transformation:
```bash
youtube-dl -x --audio-format mp3 --yes-playlist --embed-thumbnail --ignore-errors url
```

## Locally set bash as login sheel from sh

[From here](https://stackoverflow.com/questions/33292541/how-do-i-change-my-default-shell-in-ubuntu-when-not-in-etc-passwd):

If you want to fix it locally, you may have to modify your .profile to execute the shell you want:

```bash
if [ "$SHELL" != "/bin/bash" ]
then
    export SHELL="/bin/bash"
    exec /bin/bash -l    # -l: login shell again
fi
```

## Trim every file under a given directory

```
ls | xargs -I@ convert @ -trim @
```

## Automatically watch and show the latest logfile in the folder

```
watch -n 1 cat $(ls -1t | head -1)/logfile
```

```
tail -f $(ls -1t | head -1)/logfile
```


```
watch -n 1 tail $(ls -1t | head -1)
```

ref: https://superuser.com/questions/117596/how-to-tail-the-latest-file-in-a-directory

## Show disk usage

```bash
du --exclude={relative/path,path2} -cBM --max-depth=1 2> >(grep -v 'Permission denied') | sort -n
```
or
``` bash
ncdu
```


## Assign GPU in Jupyter Notebook

Do
```python
%env CUDA_DEVICE_ORDER=PCI_BUS_ID
%env CUDA_VISIBLE_DEVICES=0
```

or

```python
import os
os.environ["CUDA_VISIBLE_DEVICES"]="0"
```

Use the following to test
```python
from tensorflow.python.client import device_lib
print device_lib.list_local_devices()
```

## when using THEANO
```
THEANO_FLAGS=device=cuda0
```
```
THEANO_FLAGS=mode=FAST_RUN,floatX=float32,device=cuda0
```
```
THEANO_FLAGS=mode=FAST_RUN,floatX=float32,device=gpu0 python train.py
```
```
THEANO_FLAGS=mode=FAST_RUN,floatX=float32,device=gpu2 python
```

## when using Tensor Flow:

Noted that the order of GPUs is reversed on dd0 and dd1.

```
CUDA_VISIBLE_DEVICES=0,1,2 python
```
```
CUDA_VISIBLE_DEVICES=2 python
```
```
CUDA_VISIBLE_DEVICES=“” python
```

## Forcing Keras to use Theano:
```
KERAS_BACKEND=theano THEANO_FLAGS=mode=FAST_RUN,floatX=float32,device=gpu6 python resnet_custom_dataset_theano.py
```

## Profiling
```
python -m cProfile -s cumtime lwn2pocket.py
```

Use this if want to write to file
```
python -m cProfile -s cumtime lwn2pocket.py > profile.txt
```

Use this if want to duplicate the output to file while keep the one on screen.
```
python -m cProfile -s cumtime lwn2pocket.py | tee profile.txt
```

Use this to generate the binary output file.
```
python -m cProfile -s cumtime -o profile.txt lwn2pocket.py
```

Read the profile binary file (generated by -o option)
```
python -m cprofilev -f profile.txt  -p 7777
```


## GPU related
```
nvidiashow
```
自定义的显示nvidia-smi 的命令

```
nvidia-smi
```
“the status of nvidia gpu card."

## Forward Tensorboard:
ljubljana.iems.northwestern.edu: 129.105.36.99
开始tensorboard
```
tensorboard --logdir=./log
```
```
ssh -L 16006:127.0.0.1:6006 -i ~/.ssh/ljubljana yma@ljubljana.iems.northwestern.edu
```
refer to http://127.0.0.1:16006/ in a local browser

## Remote ipython/jupyter notebook

Source:https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh

```jupyter notebook --no-browser --port=7777```

## Tmux
```tmux```

```tux new -s svrg```

Dettach: [ctrl+b][d]

```tux attach: tux a -t svrg```

## Environment
```source activate svrg```

## Rsync
```
rsync -avz -e "ssh -i /home/yma/.ssh/celje" yma@celje.iems.northwestern.edu:/home/yma/imagenet /home/yma/imagenet --progress```

rsync with LAN acceleration:
```rsync -aHAXxv --numeric-ids -e "ssh -i /home/yma/.ssh/celje -T -c arcfour -o Compression=no -x" yma@celje.iems.northwestern.edu:/home/yma/imagenet /home/yma/imagenet --progress
```

As you have discovered you cannot use rsync with a remote source and a remote destination. Assuming the two servers can't talk directly to each other, it is possible to use ssh to tunnel via your local machine.

Instead of

```rsync -vuar host1:/var/www host2:/var/www```
you can use this

```ssh -R localhost:50000:host2:22 host1 'rsync -e "ssh -p 50000" -vuar /var/www localhost:/var/www'```
In case you're wondering, the -R option sets up a reverse channel from port 50000 on host1 that maps (via your local machine) to port 22 on host2. There is no direct connection from host1 to host2.

## My Custom Functions on Mac / Alias on Servers:
- “gits” stands for “git status”
- “gitacp “XXX” ” stands for “git add *” + “git commit -m “XXX”” + “git push origin”
- alias sa='source activate dl'
- alias sshcontainer='ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -L 7777:127.0.0.1:7777  ymaab@172.17.0.4'

### tmux

```
function ta() {
        tmux attach
}
```

## My .bashrc profile:

```bash
alias subl='sublime'
alias nvishow='watch -n1 nvidia-smi'
alias tree='tree -N'
alias blogserver='JEKYLL_ENV=production bundle exec jekyll serve -w'
alias atomblog='atom ~/blog'
alias filecount='find . | wc -l'
alias fcount='find . | wc -l'
alias ta='tmux attach'
alias brewup='brew update; brew upgrade; brew cleanup; brew doctor'


alias gits='git status'
# git push
alias gpd="git push origin develop"
alias gpm="git push origin master"
# Remove git from a project
alias ungit="find . -name '.git' -exec rm -rf {} \;"



alias aria2c='aria2c -x 16 -s 16 '
alias ytdl="youtube-dl --rm-cache-dir -i --no-check-certificate --merge-output-format mp4 --format 'bestvideo[ext=mp4]+bestaudio[ext=m4a]' --add-metadata --embed-thumbnail "
alias ygl='you-get -l '
alias yg='you-get '

alias ca='source activate dl'
alias cadl='conda activate dl'
alias ipyserver='jupyter notebook --no-browser --ip=127.0.0.1 --port=8888'
alias tbhere='tensorboard --logdir=$(pwd)'
alias ipyconvert='ipython nbconvert --to script '
alias kalicontainer='sudo docker run -it --net="host" --privileged kali:1 /bin/bash'
alias dockertag='function _dockertag(){ curl -s -S "https://registry.hub.docker.com/v2/repositories/library/$@/tags/" | jq ".\"results\"[][\"name\"]" | sort};_dockertag'
# look and kill for local program that uses certain port
alias findport='function _findport(){ lsof -n -i4TCP:$@ | grep LISTEN};_findport'
alias killport='function _killport(){ lsof -n -i4TCP:$@ | grep LISTEN | awk "{print \$2}" | xargs kill};_killport'


alias untar='sudo tar -xzvf'
alias update='sudo apt-get update; sudo apt-get upgrade'
alias mkcd='foo(){ mkdir -p "$1"; cd "$1" }; foo '
alias install='sudo apt-get install'

# ls better
## Colorize the ls output ##
alias ls='ls --color=auto'
## Use a long listing format ##
alias ll='ls -la'
## Show hidden files ##
alias l.='ls -d .* --color=auto'
alias la="ls -aF"
alias ld="ls -ld"
alias lt='ls -At1 && echo "------Oldest--"'
alias ltr='ls -Art1 && echo "------Newest--"'

alias ..="cd .."
alias ...="cd ..; cd .."
alias cd..='cd ..'
alias ...='cd ../../../'
alias ....='cd ../../../../'
alias .....='cd ../../../../'
alias .4='cd ../../../../'
alias .5='cd ../../../../..'

alias grep='grep --color=auto'
# handy short cuts #
alias h='history'
alias j='jobs -l'
alias path='echo -e ${PATH//:/\\n}'
alias vi=vim
# Stop after sending count ECHO_REQUEST packets #
alias ping='ping -c 5'
# Use netstat command to quickly list all TCP/UDP port on the server:
alias ports='netstat -tulanp'

#16: Add safety nets
# do not delete / or prompt if deleting more than 3 files at a time #
alias rm='rm -Iv --preserve-root'
# confirmation #
alias mv='mv -iv'
alias cp='cp -iv'
alias ln='ln -i'

alias cl="clear;ls;pwd"

alias editsshconfig="subl ~/.ssh/config"
alias editbashrc="subl ~/.bashrc"
alias editrc="subl ~/.zshrc"
alias sourcerc="source ~/.zshrc && source ~/.bashrc"
alias src="source "


## pass options to free ##
alias meminfo='free -m -l -t'
## get top process eating memory
alias psmem='ps auxf | sort -nr -k 4'
alias psmem10='ps auxf | sort -nr -k 4 | head -10'
## get top process eating cpu ##
alias pscpu='ps auxf | sort -nr -k 3'
alias pscpu10='ps auxf | sort -nr -k 3 | head -10'
## Get server cpu info ##
alias cpuinfo='lscpu'
## older system use /proc/cpuinfo ##
##alias cpuinfo='less /proc/cpuinfo' ##
## get GPU ram on desktop / laptop##
alias gpumeminfo='grep -i --color memory /var/log/Xorg.0.log'
# top
alias cpu='top -o cpu'
alias mem='top -o rsize' # memory

#copy output of last command to clipboard
alias cl="fc -e -|pbcopy"
# copy and paste current pwd
alias cppath='pwd|pbcopy'
alias pbpath='cd $(pbpaste)'


#27 Resume wget by default
alias wget='wget -c'

## set some other defaults ##
alias df='df -H'
alias du='du -ch'

alias nvishow='watch -n1 nvidia-smi'
alias ca='conda activate dl'
alias fcount='find . | wc -l'
```

### For server

```
function gitacp() {
    git add *
    git add -u
    git commit -m "$1"
    git push origin
}

function gitacm() {
    git add *
    git commit -m "$1"
    git push origin master
}

function gitacd() {
    git add *
    git commit -m "$1"
    git push origin dev
}

```

```alias sshcontainer='ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -L 7777:127.0.0.1:7777  ymaab@172.17.0.4'```



### For personal laptop:

```bash
alias gc='gcalcli'
alias gca='gcalcli agenda '
alias gcm='gcalcli calm '
alias gcw='gcalcli calw '
```

### Extra setting inside container:

```export HOME=/data/ymaab```

```alias ipyserver='jupyter notebook --no-browser  --ip=127.0.0.1 --port=7777'```

### alias
```alias nvishow='watch -n1 nvidia-smi'```

```alias sa='source activate tf'```

```alias rsynccopy="rsync --partial --progress --append --rsh=ssh -r -h "```

```alias rsyncmove="rsync --partial --progress --append --rsh=ssh -r -h --remove-sent-files"```

## iPython Extensions:
 - http://ipython.readthedocs.io/en/stable/config/extensions/

## Solving local error
 - https://stackoverflow.com/questions/36394101/pip-install-locale-error-unsupported-locale-setting
 - unset LC_ALL
 - export LC_ALL=C

## Install Newer tensorflow on container
https://stackoverflow.com/questions/39817645/cuda-cudnn-installed-but-tensorflow-cant-use-the-gpu

## Manually install ffmpeg (static version)
https://gist.github.com/jmsaavedra/62bbcd20d40bcddf27ac

It turns out that imageio has a function to download this for you. Read the help information!

# Log in to Container:

~~ssh lxe1647 -p 2207~~

~~ssh lxe1646 -p 2201~~

```ssh lxe1646 -p 2200```

The PyTorch Container:
```ssh lxe1646 -p 2203```

# Run Command
## Tensorflow CycleGAN
```python -m CycleGAN_TensorFlow.create_cyclegan_dataset --image_path_a=./CycleGAN_TensorFlow/input/syncars/trainA --image_path_b=./CycleGAN_TensorFlow/input/syncars/trainB --dataset_name="syncars_train" --do_shuffle=0```

```python -m CycleGAN_TensorFlow.create_cyclegan_dataset --image_path_a=./CycleGAN_TensorFlow/input/syncars/testA --image_path_b=./CycleGAN_TensorFlow/input/syncars/testB --dataset_name="syncars_test" --do_shuffle=0```

```CUDA_VISIBLE_DEVICES=3 python -m CycleGAN_TensorFlow.main --to_train=1 --log_dir=CycleGAN_TensorFlow/output/cyclegan/syncars  --config_filename=CycleGAN_TensorFlow/configs/syncars_train.json```

```CUDA_VISIBLE_DEVICES=2 python -m CycleGAN_TensorFlow.main --to_train=0 --log_dir=CycleGAN_TensorFlow/output/cyclegan/syncars  --config_filename=CycleGAN_TensorFlow/configs/syncars_test.json    --checkpoint_dir=CycleGAN_TensorFlow/output/cyclegan/syncars/20171108-105517```

# Linux Bash

## Move Files in batch filtered with name

```
for n in $(seq 9431 9999); do mv "IMG_$n.jpg" some_other_dir; done
```

## Find without permission denied warning

```
find / -name car_damage*  2>&1 | grep -v "Permission denied"
```

# Install Package locally without root
https://askubuntu.com/questions/339/how-can-i-install-a-package-without-root-access

# List for useful Linux programs:

- ncdu
        - visualize the data usage

# Move top 100 files
```
for file in $(ls -p | grep -v / | tail -100)
do
mv $file /other/location
done
```
## Jupyter change Conda kernel extension

For conda kernel support:
https://github.com/Anaconda-Platform/nb_conda_kernels

For solyly extensions:
https://github.com/ipython-contrib/jupyter_contrib_nbextensions

# Pandas Dataframe
[MultiIndex CrossSelection](https://pandas-docs.github.io/pandas-docs-travis/advanced.html#advanced-xs):
The xs method of DataFrame additionally takes a level argument to make selecting data at a particular level of a MultiIndex easier.
```python
df.xs('one', level='second')
```
# Atom

## Atom Shortcuts Collection

[A Table on Git Repo that collects Atom Shortcuts](https://github.com/nwinkler/atom-keyboard-shortcuts)
