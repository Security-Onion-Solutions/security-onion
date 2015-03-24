# mysqltuner #

Run mysqltuner to get some initial recommendations:
```
# Install mysqltuner if you haven't already
sudo apt-get install mysqltuner

# Run mysqltuner
sudo mysqltuner
```

# /etc/mysql/my.cnf vs /etc/mysql/conf.d/ #

Implement mysqltuner's recommendations in /etc/mysql/my.cnf or create a new file in /etc/mysql/conf.d/ with the changes.  We recommend /etc/mysql/conf.d/ so that your changes don't get overwritten during MySQL package upgrades.

# Restart MySQL #
Changes don't take effect until MySQL is restarted and you should ensure that Sguil and other services aren't using MySQL before shutting it down.

# Variables #

The first variable that you'll probably need to tune is open-files-limit:
<a href='http://nsmwiki.org/Sguil_FAQ#I.27m_seeing_error_code_24_from_MySQL._How_do_I_fix_that.3F'>error_code_24 out-of-resources</a>

Here are some other common variables that will probably need to be tuned for your system:

  * table\_cache
  * key\_buffer
  * max\_connections

# MySQL slow to start on boot #

At boot time, MySQL checks all tables, which can take a long time.  If you wish to disable this check, comment out "check\_for\_crashed\_tables" in /etc/mysql/debian-start.