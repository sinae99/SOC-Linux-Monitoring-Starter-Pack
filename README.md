## SOC - Linux Monitoring Starter Pack

These are basic metrics for Unix-based systems to help start investigations in a SOC environment.

A list for teams new to Linux monitoring, covering elementary things to track from scratch

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
index=linux_index sourcetype=auth_logs "failed password" OR "authentication failure"
| search user="root" OR user="sudo"
| stats count by user, src_ip, _time
```










