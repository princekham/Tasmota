#### To check if crontab is working
```
sudo systemctl status cron
```
#### If the service is not running, you can start it with:
  ``` sudo systemctl start cron.service ```
#### And enable it to start on boot with:
``` sudo systemctl enable cron.service ```
####  Examine cron logs:
Cron logs its activity to the system log, which you can check to confirm that your jobs are being executed.
``` grep 'cron' /var/log/syslog ```
#### List cron jobs for the current user:
``` crontab -l ```

