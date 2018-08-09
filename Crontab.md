

# Crontab: automatically run scripts at the times of your choices. 

[Cron](https://en.wikipedia.org/wiki/Cron) is a Unix, solaris, Linux utility that allows tasks to be automatically run in the background at regular intervals by the cron daemon. We can use [crontab](http://www.adminschoice.com/crontab-quick-reference) to easily setup a daily scheduled task to manufacture CPU utility on the server. The following 3-step guide show you how to setup a daily schedule to run a certain script on your server, which should maintain a cpu utility rate higher than 1%.

## Step 1: install crontab

All modern Ubuntu comes with crontab so there is no need to install or set up it. 

For centos user, use the following commands to install it. The following code works on Didi's centos image:
``` bash
yum install vixie-cron
yum install cronie
service crond start
chkconfig crond on
```

## Step 2: Write a script that can run CPU

We recommand using [stress](https://linux.die.net/man/1/stress) to impose loads on CPU. If your ubuntu distribution doesn't come with this command, install it with package manager:
``` bash
sudo apt-get install stress
```
or 
```
sudo yum install stress
```
for centos user.


Then create a bash script called "daily_run.sh" at anywhere you like, for example, the "~" directory, with following content:
```
stress --cpu 5 -t 1000
```
This command will run 5 cores of CPU with 100% usage for 1000 seconds. 

Now you should have a daily_run.sh under ~/

## Step 3: Configure crontab to automatically run the script everyday

Run the following command:
``` bash
crontab -e
```

This commands should opens a text file where you will append the following line to this file:
``` bash
14 13 * * * ~/daily_run.sh
```

This line says that we asks the cron service to run the ~/daily_run.sh script for us whenever the system clock hits 13:14:00. Reminds that this is 13:14pm in Beijing time, hence it is around midnight in west coast. You can check [this guide](https://www.debian-tutorials.com/crontab-tutorial-cron-howto) for further instruction. 


## Step 4: Vala! You should be all set!

Now you can forget about the stress of having server shutdown by manager due to inadquate usage.
