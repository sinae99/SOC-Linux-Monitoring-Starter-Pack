## SOC - Linux Monitoring Starter Pack

These are basic metrics that I think can be useful in the first steps of setting up Linux monitoring and starting investigations in a SOC environment

A list for teams covering elementary things to track from scratch

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Critical Services Stop"
Monitoring the stopping of critical services like `auditd`, `sshd`, `httpd` or `syslog`

```
index=linux_logs sourcetype=authentication_logs ("sudo service" AND ("stop" OR "failed"))
| search service_name="auditd" OR service_name="sshd" OR service_name=rest "
| stats count by service_name, host, _time
```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### "Services Stopped but didn't Started Back"
services that have been stopped but fail to restart automatically again

```sudo systemctl stop httpd```

```sudo systemctl status httpd```  -----> NOT UP

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Critical Servers Shutdown and Reboot"
Tracking the shutdown and reboot of critical servers 

to identify system failures or unauthorized actions


```sudo shutdown -h now```

``` sudo init 6```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Services Stop and Start"
A complete overview of stopping and starting services per host. 

This helps us to understand what is happening and gives feedback on company services trend

```sudo service named stop```

```sudo service named start```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Super User Login Fails"
Detecting potential brute force for root account

```
index=linux_index sourcetype=authentication_logs "failed password" OR "authentication failure"
| search user="root" OR user="sudo"
| stats count by user, src_ip, _time
```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "High Number of Failed Logins in a Short Time"
based on your company threshold

```
index=linux_logs sourcetype=authentication_logs "Failed password"
| bin _time span=10m
| stats count by _time, user, host
| where count > 10
```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Adding, Removing, and Modifying Cron Jobs"

```crontab -e ```

```sudo crontab -r  ```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "Any Change in sudoers File"
One of the most important files in the system

```
index=linux_logs sourcetype=authentication_logs "sudoers" OR "visudo"
| stats count by user, action, _time, host
```
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "New User Created or Deleted"
For detecting unauthorized changes, especially those made without permission or out of company plan 

```sudo useradd JavadBellingham ```

```sudo userdel JavadBellingham ```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### "New Services Installed or Modified"
For detecting unauthorized software installation

```sudo apt-get install apache2```

```sudo dnf install mysql-server```


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------









