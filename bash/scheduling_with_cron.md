### Scheduling with Cron
This section gives an overview how to schedule processes with Cron.

#### Cron table - for users
Every user has his own Cron table which is the table of scheduled processes. They can be find at `/var/spool/cron/crontabs`. Users included in `/etc/cron.allow` can use crontab. Or users included in `/etc/cron.deny` cannot use crontab.<br/><br/>
**Commands**<br/>
- `crontab -l`: listing schedules processes
- `crontab -e`: edit crontab
  - fields order in crontab:<br/>
  `minute` `hour` `dayOfMonth` `monthOfYear` `numOfWeekDay` `command`
  - every day, month, etc.. indicated with: **\***
  - we can define a range with: **-**
  - we can define a list separated  with: **,**
  - an example:
    - `15 10 1-10/2 * 5 echo "valami"` -> at 10:15 on 1,3,5,7,9 day of every month on Friday


#### Cron table - system wide
It can be find at `/etc/crontab`. It has an extra users field. <br/>
`minute` `hour` `dayOfMonth` `monthOfYear` `numOfWeekDay` `user` `command`
