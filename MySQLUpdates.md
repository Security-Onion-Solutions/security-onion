# Use Soup #

It's no longer necessary to perform the steps listed here as long as you use "soup" to perform updates.  For more information, please see the [Updating](Upgrade.md) page.

# Introduction #

Updating the Ubuntu MySQL packages can be problematic due to autossh port forwarding and a bug in our current version of ELSA.  Here's the recommended procedure to ensure a smooth update.

# Procedure #

  1. Stop all relevant services:
```
sudo service nsm stop
sudo service syslog-ng stop
sudo service apache2 stop
sudo pkill autossh
sudo pkill perl
```
  1. Check the process listing and verify all nsm/syslog-ng/apache/autossh/perl processes have stopped:
```
ps aux
```
  1. Install the MySQL updates.  Other updates (such as securityonion-snorby) may require MySQL to be running, so only update the MySQL server packages as shown below.  If you're prompted concerning locally modified config files, keep your currently-installed version.  This is especially important in the case of ELSA log nodes where we've changed the MySQL port from its default of 3306 to 50000.
```
sudo apt-get update && sudo apt-get install mysql-server mysql-server-core-5.5 mysql-server-5.5
```
  1. Reboot:
```
sudo reboot
```