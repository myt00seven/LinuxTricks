Here is a brief memo for how I automate my torrent download on PT server.
 
It (should) has the following features:
- automatically listen to RSS feed (No IRC support to the site I need)
- add seed only if have disk space left 
- remove seed when ratio and seeding time meets requirement

# basic setting

So MT doesn't have IRC channel now. We have to use RSS based auto download client. Hence we choose Flexget over autodl-irssi. 

## rotrrent.rc

schedule = watch_directory,5,5,"load.start=~/watch/*.torrent,\"d.delete_tied=\""
schedule = watch_directory_2,5,5,"load.start=~/watch_race/*.torrent,d.set_directory=~/files_race/,d.set_custom1=race"

## RSS seeds

There are many tricks about RSS seeds. For example, you can output only those starred torrents as a separate RSS seed.

# FlexGet

It supports read from RSS, IRC or even from uTorrent. Check the [list for its filters](https://flexget.com/Plugins#filter).

## Installation

Finally I successfully install flexget with virtualenv and python 2.7. After that, we can directly call /home/user/virtualenv2/bin/flexget without activating the virtual environment.

## Automatically run

We can use crontab to run flexget. 

For updates at a custom interval:

```
 */30 * * * * /home/user/virtualenv2/bin/flexget --cron execute
```
where 30 is the number of minutes between executions.

## Other usage

Test execution
```
flexget --test execute
```


Check syntax in config file:
```
flexget check
```

# automatically add

## Filter based on RegExp and torrent name in RSS seed 

First, you need to output the size of torrent in the RSS seed. Then you just need to set up the configure file for FlexGet. Try this [regexp testing webpage](https://regexr.com/).

For example, 

\[([0-9]){2,3}\.([0-9]){1,3} [G][B]\]

# condition

## Linux du space 

```bash
echo "Check Space"
SPACE_USED=$(du -d 0 /home/myt007/files_race | cut -f1)
echo $SPACE_USED
echo $(du -d 0 -h /home/myt007/files_race)
if (( $(echo "$SPACE_USED > 500000000 " |bc -l) )); then
        echo "Folder is bigger than 500 GB"
else
        echo "Folder is smaller than 500 GB"
fi
```

## Flexget Free Space 

[Free Space](https://flexget.com/Plugins/free_space):
This plugin will abort a task if free space on a given drive is getting low. space: Amount of free space (in MB) that you require for new entries to be allowed.


```yaml
free_space:
  path: /location/to/monitor
  space: 500
```

# remove seed

## rTorrent Ratio Group

You can specify the ratio group in rtorrent to automatically delete the files. See [explanation](https://www.torrent-invites.com/forum/seedboxes/seedbox-tutorials/221649-the-rutorrent-ratio-groups-tutorial) here.

https://cheapseedboxes.com/setting-ratio-groups-rutorrent/

If you wish to specify different ratio's for different watch directories, do the following:
https://github.com/rakshasa/rtorrent/wiki/RTorrentRatioHandling

## Flexget Native 

## rtcontrol

rtcontrol from pyrocore is another option. It can 
[Only start items that you have disk space for](https://pyrocore.readthedocs.io/en/latest/custom.html#only-start-items-that-you-have-disk-space-for)
and [Deleting Download Items and Their Data](https://pyrocore.readthedocs.io/en/latest/usage.html#deleting-download-items-and-their-data).

https://www.reddit.com/r/seedboxes/comments/6cr0ap/rtorrent_ratio_groups_question/

### Install rtcontrol

It's easier to use their script and install from Github: https://pyrocore.readthedocs.io/en/latest/installation.html#option-1-installing-from-github.

### Usage 

rtcontrol --cull --yes 'path=*race*' ratio=+2.9

rtcontrol --cull --yes 'path=*race*' is_complete=yes completed=+5d

# My sample FlexGet config file 

```yaml
tasks:
  bluray tasks:
    rss: https://tp.m-team.cc/torrentrss.php?https=1&rows=10&cat421=1&cat431=1&cat432=1&icat=1&ismalldescr=1&isize=1&iuplder=1&linktype=dl&passkey=d2a698764fb6b500a13b96f3a8df3a44
    regexp:
      accept:    
        - \[([0-9]){1,2}\.([0-9]){2,3} [G][B]\]
    limit_new: 1
    download: /home/myt007/watch_race
```
