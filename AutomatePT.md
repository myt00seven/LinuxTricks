Here is a brief memo for how I automate my torrent download on PT server.
 
It (should) has the following features:
- automatically listen to RSS feed (No IRC support to the site I need)
- add seed only if have disk space left 
- remove seed when ratio and seeding time meets requirement

# basic setting

So MT doesn't have IRC channel now. We have to use RSS based auto download client. Hence we choose Flexget over autodl-irssi. 

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

# condition

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

## Flexget Native 

## rtcontrol

rtcontrol from pyrocore is another option. It can 
[Only start items that you have disk space for](https://pyrocore.readthedocs.io/en/latest/custom.html#only-start-items-that-you-have-disk-space-for)
and [Deleting Download Items and Their Data](https://pyrocore.readthedocs.io/en/latest/usage.html#deleting-download-items-and-their-data).

# My sample FlexGet config file 
