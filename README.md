# LinuxTricks
Record some ueful Linux Tricks that I have been using

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

```function ta() {
        tmux attach
}```

### for github

```
function gitacp() {
    git add *
    git add -u
    git commit -m "$1"
    git push origin
}
```

```
function gitacp_master() {
    git add *
    git commit -m "$1"
    git push origin master
}
```

```
function gitacp_dev() {
    git add *
    git commit -m "$1"
    git push origin dev
}
```

```function gits(){
    git status
}
```

```function gpus(){while sleep 1; do nvidia-smi; done}```

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
