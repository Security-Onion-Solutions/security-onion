# NOTE: This FAQ was originally written for Security Onion 10.04.  Not all FAQ entries have been updated for Security Onion 12.04 yet. #

# Why won't the ISO image boot on my machine? #
[TroubleBooting](TroubleBooting.md)

# What's the recommended procedure for installing Security Onion? #
[Installation Procedure](Installation.md)

# How do I install Security Onion updates? #
[Upgrade Procedure](Upgrade.md)

# Why do I get Snorby/Suricata/Bro errors after upgrading the kernel and pfring packages? #
[Updating](Upgrade.md)

# What do I need to do if I'm behind a proxy? #
[Proxy Configuration](Proxy.md)

# What is the password for root/mysql/Sguil/Squert/Snorby/ELSA? #
[Passwords](Passwords.md)

# I've forgotten my Snorby password.  How do I reset it? #
[Passwords](Passwords.md)

# How do I configure email for alerting and reporting? #
[Email](Email.md)

# Where can I read more about the tools contained within Security Onion? #
[Tools](Tools.md)

# How do I configure a BPF for Snort/Suricata/Bro? #
[BPF](BPF.md)

# Where can I find interesting pcaps to replay? #
[Pcaps](Pcaps.md)

# What are the default firewall settings and how do I change them? #
[Firewall](Firewall.md)

# How can I add and test local rules? #
[Adding local rules and testing them with scapy](AddingLocalRules.md)

# Can I be alerted when an interface stops receiving traffic? #
[Interface stops receiving traffic](SensorStopsSeeingTraffic.md)

# What do I need to modify in order to have the log files stored on a different mount point? #
[Adding a New Disk for /nsm](NewDisk.md)

# How do I disable the graphical Network Manager and configuring networking from the command line? #
[Network Configuration](NetworkConfiguration.md)

# Where do I send questions/problems/suggestions? #
[security-onion Google Group](MailingLists.md)

# I submitted a message to the security-onion Google Group.  Why isn't it showing up? #
https://code.google.com/p/security-onion/wiki/MailingLists#Moderation

# What's the directory structure of /nsm? #
[/nsm Directory Structure](DirectoryStructure.md)

# Why does Security Onion use UTC? #
[UTC and Time Zones](TimeZones.md)

# Why are the timestamps in ELSA not in UTC? #
[UTC and Time Zones](TimeZones.md)

# How do I enable/disable processes? #
[Disabling Processes](DisablingProcesses.md)

# Where do I put my custom ELSA parsers? #
[CustomELSAParsers](CustomELSAParsers.md)

# How do I wipe the Snorby database? #
[Wiping Snorby database](WipingSnorby.md)

# I disabled some Sguil agents but they still appear in Sguil's "Agent Status" tab. #
https://code.google.com/p/security-onion/wiki/DisablingProcesses#Sguil_Agent

# Is commercial support available for Security Onion? #
Yes, please see:
http://securityonionsolutions.com

# I'm running the Security Onion 12.04.5 ISO image and Chromium crashes and/or displays a black screen. #
This is a known issue with certain versions of VMware.  You can either:

- go into the VM configuration and disable 3D in the video adapter

OR

- upgrade the VM hardware level (may require upgrading to a new version of VMware)

# Why does soup fail with an error message like "find: `/usr/lib/python2.7/dist-packages/salt/': No such file or directory"? #
This is a bug in the salt packages that can manifest when skipping salt versions.  Resolve with the following:
```
sudo mkdir -p /usr/lib/python2.7/dist-packages/salt/
sudo apt-get -f install
sudo soup
```

# I recently updated barnyard and now I'm not getting any Snort alerts. #
Some users running the Snort engine with the VRT ruleset are experiencing barnyard2 failing with errors like "Returned signature\_id is not equal to updated signature\_id".  This is due to some wrong entries in the database left by the previous version of barnyard2.  One of the barnyard2 developers wrote a MySQL script to fix these entries and I've packaged it into a shell script called so-snorby-fix-sigs and included it in the rule-update package.  If you're running the Snort engine with the VRT ruleset, please run so-snorby-fix-sigs and follow the directions (including shutting down all barnyard2 instances before proceeding with the database changes).

http://blog.securityonion.net/2014/06/new-securityonion-rule-update-package.html

# Why does barnyard2 keep failing with errors like "Returned signature\_id is not equal to updated signature\_id"? #
Please see:
http://blog.securityonion.net/2014/06/new-securityonion-rule-update-package.html

# Ubuntu is asking if I want to upgrade to Ubuntu 14.04.  Should I? #
No, please do not upgrade to Ubuntu 14.04, as it is incompatible with Security Onion.

# Will Security Onion move to Ubuntu 14.04? #
We have no immediate plans to move to Ubuntu 14.04.  Ubuntu 12.04 should be fully supported until April 2017: https://wiki.ubuntu.com/Releases

# Ubuntu is saying that my kernel has reached EOL (End Of Life).  Should I update to the newer HWE stack? #
Please see:
http://blog.securityonion.net/2014/08/ubuntu-hardware-enablement-hwe-stacks.html

# I just updated Snort and it's now saying 'ERROR: The dynamic detection library "/usr/local/lib/snort\_dynamicrules/chat.so" version 1.0 compiled with dynamic engine library version 2.1 isn't compatible with the current dynamic engine library "/usr/lib/snort\_dynamicengine/libsf\_engine.so" version 2.4.' #

Run the following:
```
sudo rule-update
```

For more information, please see:

http://blog.securityonion.net/2014/12/new-version-of-securityonion-rule.html

# Why does sostat show a high number of ELSA Buffers in Queue? #
There are usually 2 main reasons for this:

- low on RAM

OR

- ungraceful shutdown (perhaps power outage) resulted in database corruption

Take a look at the ELSA log files in /nsm/elsa/data/elsa/log/ and look for errors.  If there are errors related to MySQL, please see this thread:

https://groups.google.com/d/topic/security-onion/O3uBjCR5jYk/discussion

# Why does rule-update show barnyard2 errors? #

rule-update now runs barnyard2 to update Snorby's reference table:

http://blog.securityonion.net/2014/06/new-barnyard2-nsm-rule-update-and.html

Since barnyard2 isn't processing any actual unified2 data, it outputs the following errors:
```
ERROR: Unable to open directory '' (No such file or directory)
ERROR: Unable to find the next spool file!
```

This is normal.

# I just rebooted and it looks like the services aren't starting automatically. #
/etc/init/securityonion.conf waits 60 seconds after boot to ensure network interfaces are fully initialized before starting services.

# It looks like ELSA is purging data before I hit log\_size\_limit. #
Please see:

https://code.google.com/p/enterprise-log-search-and-archive/wiki/Documentation#Low_volume_configuration_tuning

https://groups.google.com/forum/#!searchin/enterprise-log-search-and-archive/%22allowed_temp_percent%22/enterprise-log-search-and-archive/auUSYj77ctw/mzF-YqVa5KMJ

https://groups.google.com/d/topic/security-onion/xLxTGQs30ho/discussion

# What can I do to decrease the size of my securityonion\_db (sguild) MySQL database? #
You can lower the DAYSTOKEEP setting in /etc/nsm/securityonion.conf.  Also see UNCAT\_MAX:
http://blog.securityonion.net/2015/01/new-version-of-sguil-db-purge-helps.html

# Why is my disk filling up? #
Sguil uses netsniff-ng to record full packet captures to disk.  These pcaps are stored in /nsm/sensor\_data/HOSTNAME-INTERFACE/dailylogs/.  /etc/cron.d/sensor-clean is a cronjob that runs every minute that should delete old pcaps when the disk reaches your defined disk usage threshold (90% by default).  It's important to properly size your disk storage so that you avoid filling the disk to 100% between purges.<br>

<h1>What does it mean if I have a high number of Sguil Uncategorized Events?</h1>

Sguild has to load uncategorized events into memory when it starts and it won't accept connections until that's complete.<br>
<br>
You can either:<br>
<br>
- wait for sguild to start up (may take a LONG time), then log into Sguil, and F8 LOTS of events<br>
<br>
OR<br>
<br>
- stop sguild (sudo nsm_server_ps-stop) and manually categorize events using mysql (see <a href='http://taosecurity.blogspot.com/2013/02/recovering-from-suricata-gone-wild.html'>http://taosecurity.blogspot.com/2013/02/recovering-from-suricata-gone-wild.html</a>) OR lower your DAYSTOKEEP setting in /etc/nsm/securityonion.conf and run "sudo sguil-db-purge".<br>
<br>
To keep Uncategorized Events from getting too high, you should log into Sguil/Squert on a daily/weekly basis and categorize events.  If you don't ever want to use Sguil/Squert, you can create an autocat to automatically categorize incoming events.  See <a href='https://code.google.com/p/security-onion/wiki/ManagingAlerts#Autocategorize_events'>https://code.google.com/p/security-onion/wiki/ManagingAlerts#Autocategorize_events</a>.<br>
<br>
Also see UNCAT_MAX: <a href='http://blog.securityonion.net/2015/01/new-version-of-sguil-db-purge-helps.html'>http://blog.securityonion.net/2015/01/new-version-of-sguil-db-purge-helps.html</a>

<h1>Can Security Onion run in IPS mode?</h1>
Running Security Onion as an IPS requires manual configuration and is not supported.  I talked about this on the Packet Pushers podcast:<br>
<a href='http://packetpushers.net/show-95-security-onion-with-doug-burks-or-why-ids-rules-and-ips-drools/'>http://packetpushers.net/show-95-security-onion-with-doug-burks-or-why-ids-rules-and-ips-drools/</a>

<h1>Where can I get the source code?</h1>
You can download the full source code for any of our packages like this:<br>
<pre><code>apt-get source PACKAGE-NAME<br>
</code></pre>
where PACKAGE-NAME is usually something like securityonion-snort.  Here's a list of all of our packages:<br>
<a href='https://launchpad.net/~securityonion/+archive/stable'>https://launchpad.net/~securityonion/+archive/stable</a>

<h1>Why does my VMware image rename eth0 to eth1?</h1>
Usually this happens when you clone a VM.  VMware asks if you moved it or copied it.  If you select "copied", it will change the MAC address to avoid duplication.  At the next boot, Ubuntu's udev will see a new MAC address and create a new network interface (eth1).  To fix this:<br>
<pre><code>sudo rm /etc/udev/rules.d/70-persistent-net.rules<br>
sudo reboot<br>
</code></pre>

<h1>How do I change the fonts in the Sguil client?</h1>

In the Sguil client, click the File menu and then go to "Change Font".  You can change both the Standard and Fixed fonts.<br>
<br>
<h1>How do I boot Security Onion to text mode (CLI instead of GUI)?</h1>
In /etc/default/grub, change this line:<br>
<pre><code>GRUB_CMDLINE_LINUX_DEFAULT="splash quiet"<br>
</code></pre>
to:<br>
<pre><code>GRUB_CMDLINE_LINUX_DEFAULT="text"<br>
</code></pre>

Then run:<br>
<pre><code>sudo update-grub<br>
</code></pre>

For more information, please see:<br>
<a href='http://ubuntuforums.org/showthread.php?t=1690118'><a href='http://ubuntuforums.org/showthread.php?t=1690118'>http://ubuntuforums.org/showthread.php?t=1690118</a></a>

In Security Onion 12.04, you can install our packages on top of Ubuntu Server (minimal installation, no GUI).<br>
<br>
<h1>How do I get Security Onion to recognize more than 4GB of RAM?</h1>
If you have a 64-bit machine, use our 64-bit ISO image or use a 64-bit version of Ubuntu.<br>
<br>
If you have a 32-bit machine, you'll need to use a 32-bit version of Ubuntu and then install the PAE kernel as described here:<br>
<a href='https://help.ubuntu.com/community/EnablingPAE'><a href='https://help.ubuntu.com/community/EnablingPAE'>https://help.ubuntu.com/community/EnablingPAE</a></a>

<h1>I'm running Security Onion in a VM and the screensaver is using lots of CPU.  How do I change/disable the screensaver?</h1>
<ol><li>Click Applications.<br>
</li><li>Click Settings.<br>
</li><li>Click Screensaver.<br>
</li><li>Screensaver Preferences window appears.  Click the Mode dropdown and select "Disable Screen Saver" or "Blank Screen Only".<br>
</li><li>Close the Screensaver Preferences window.<br></li></ol>

<h1>I'm currently running Snort.  How do I switch to Suricata?</h1>
<pre><code>sudo nsm_sensor_ps-stop<br>
sudo sed -i 's|ENGINE=snort|ENGINE=suricata|g' /etc/nsm/securityonion.conf<br>
sudo rule-update <br>
sudo nsm_sensor_ps-start<br>
</code></pre>

<h1>I'm currently running Suricata.  How do I switch to Snort?</h1>
<pre><code>sudo nsm_sensor_ps-stop<br>
sudo sed -i 's|ENGINE=suricata|ENGINE=snort|g' /etc/nsm/securityonion.conf<br>
sudo rule-update<br>
sudo nsm_sensor_ps-start<br>
</code></pre>

<h1>How do I add a new user to Sguil?</h1>
You can add new Sguil user accounts with the following:<br>
<pre><code>sudo nsm_server_user-add<br>
</code></pre>

<h1>How do I get ELSA to display bar charts?</h1>
ELSA's bar charts require flash, so one option would be to replace Chromium with <a href='http://www.google.com/chrome/'>Google Chrome</a> (which includes flash).  If you install Chrome on Security Onion and want to make it your default browser so that you can pivot from Sguil to Chrome, do the following:<br>
<pre><code>cd /etc/alternatives<br>
sudo rm x-www-browser<br>
sudo ln -s /usr/bin/google-chrome-stable x-www-browser<br>
</code></pre>
<h1>I get periodic MySQL crashes and/or error code 24 "out of resources" when searching in Sguil.  How do I fix that?</h1>
Recent versions of Setup should set MySQL's open-files-limit to 90000 to avoid this problem:<br>
<a href='http://blog.securityonion.net/2014/02/new-securityonion-setup-package.html'>http://blog.securityonion.net/2014/02/new-securityonion-setup-package.html</a>

If you ran Setup before February 2014, you can set this manually as follows.<br>
<br>
First, stop sguil and mysql:<br>
<pre><code>sudo nsm_server_ps-stop<br>
sudo service mysql stop<br>
</code></pre>
Next, edit /etc/mysql/my.cnf and add the following in the <a href='mysqld.md'>mysqld</a> section (please use hyphens not underscores):<br>
<pre><code>open-files-limit        = 90000<br>
</code></pre>
Finally, start mysql and sguil:<br>
<pre><code>sudo service mysql start<br>
sudo nsm_server_ps-start<br>
</code></pre>
For more information, please see:<br>
<a href='http://nsmwiki.org/Sguil_FAQ#I.27m_seeing_error_code_24_from_MySQL._How_do_I_fix_that.3F'><a href='http://nsmwiki.org/Sguil_FAQ#I.27m_seeing_error_code_24_from_MySQL._How_do_I_fix_that.3F'>http://nsmwiki.org/Sguil_FAQ#I.27m_seeing_error_code_24_from_MySQL._How_do_I_fix_that.3F</a></a><br>

<h1>How can I remote control my Security Onion box?</h1>
A few options:<br>
<ul><li>"ssh -X" - any program started in the SSH session will be displayed on your local desktop (requires a local X server)<br>
</li><li><a href='http://www.xrdp.org/'>xrdp</a> - sudo apt-get install xrdp - requires an rdp client<br>
</li><li>You can use <a href='FreeNX.md'>FreeNX</a> but we don't recommend or support it</li></ul>

<h1>Why does the Snorby dashboard show all zeroes?</h1>
Please do the following:<br>
<ul><li>click "More Options"<br>
</li><li>click "Force Cache Update"<br>
</li><li>Dashboard will display "Current caching"<br>
</li><li>once that disappears, then refresh the Dashboard page</li></ul>

If that doesn't help, try rebooting.<br>
<br>
If that still doesn't help, please see:<br>
<a href='https://github.com/Snorby/snorby/issues/340'>https://github.com/Snorby/snorby/issues/340</a>

<h1>Why isn't Snorby showing GeoIP data properly?</h1>
Try forcing the GeoIP job to run by doing the following.  First, open the Rails console:<br>
<pre><code>cd /opt/snorby/<br>
sudo RAILS_ENV=production bundle exec rails c<br>
</code></pre>
In the Rails console, initiate the GeoIP job:<br>
<pre><code>Snorby::Jobs::GeoipUpdatedbJob.new(true).perform<br>
quit<br>
</code></pre>

<h1>How do I disable Snorby?</h1>
<ol><li>Disable Snorby in the Apache configuration:<br>
<pre><code>sudo a2dissite snorby<br>
</code></pre>
</li><li>Reload Apache configuration:<br>
<pre><code>sudo service apache2 reload<br>
</code></pre>
</li><li>Prevent Snorby worker from starting on boot by setting SNORBY_ENABLED=no in /etc/nsm/securityonion.conf.<br>
</li><li>Comment out the output database line in all barnyard2.conf files on all sensors:<br>
<pre><code>sudo sed -i 's|output database: alert, mysql, user=root dbname=snorby host=127.0.0.1|#output database: alert, mysql, user=root dbname=snorby host=127.0.0.1|g' /etc/nsm/*/barnyard2*.conf<br>
</code></pre>
</li><li>Restart barnyard2 on all sensors:<br>
<pre><code>sudo nsm_sensor_ps-restart --only-barnyard2<br>
</code></pre></li></ol>

<h1>Why does Snort segfault every day at 7:01 AM?</h1>
7:01 AM is the time of the daily PulledPork rules update.  If you're running Snort with the VRT ruleset, this includes updating the SO rules.  There is a known issue when running Snort with the VRT ruleset and updating the SO rules:<br>
<a href='https://groups.google.com/d/topic/pulledpork-users/1bQDkh3AhNs/discussion'><a href='https://groups.google.com/d/topic/pulledpork-users/1bQDkh3AhNs/discussion'>https://groups.google.com/d/topic/pulledpork-users/1bQDkh3AhNs/discussion</a></a><br>
After updating the rules, Snort is restarted, and the segfault occurs in the OLD instance of Snort (not the NEW instance).  Therefore, the segfault is merely a nuisance log entry and can safely be ignored.<br>
<br>
<h1>Barnyard2 is failing with an error like "ERROR: sguil: Expected Confirm 13324 and got: Failed to insert 13324: mysqlexec/db server: Duplicate entry '9-13324' for key 'PRIMARY'".  How do I fix this?</h1>

Sometimes, just restarting Barnyard will clear this up:<br>
<pre><code>sudo nsm_sensor_ps-restart --only-barnyard2<br>
</code></pre>

Other times, restarting Sguild and then restarting Barnyard will clear it up:<br>
<pre><code>sudo nsm_server_ps-restart<br>
sudo nsm_sensor_ps-restart --only-barnyard2<br>
</code></pre>

If that doesn't work, then try also restarting mysql:<br>
<pre><code>sudo service mysql restart<br>
sudo nsm_server_ps-restart<br>
sudo nsm_sensor_ps-restart --only-barnyard2<br>
</code></pre>

If that still doesn't fix it, you may have to perform MySQL surgery on the database "securityonion_db" as described in the Sguil FAQ:<br>
<a href='http://nsmwiki.org/Sguil_FAQ#Barnyard_dies_at_startup.2C_with_.22Duplicate_Entry.22_error'><a href='http://nsmwiki.org/Sguil_FAQ#Barnyard_dies_at_startup.2C_with_.22Duplicate_Entry.22_error'>http://nsmwiki.org/Sguil_FAQ#Barnyard_dies_at_startup.2C_with_.22Duplicate_Entry.22_error</a></a>

<h1>Why does the Snorby worker fail with "ERROR: Process ID out of range."?</h1>
The root cause is that the Snorby worker got into a failed state with the /opt/snorby/tmp/pids/delayed_job.pid file being empty. Snorby reads in the contents of this pid file and if it's empty, it results in "ERROR: Process ID out of range.".  Removing the empty pid file allows Snorby to start correctly.<br>
<pre><code>sudo mv /opt/snorby/tmp/pids/delayed_job.pid /tmp/<br>
sudo reboot<br>
</code></pre>
For more info, please see:<br>
<a href='https://groups.google.com/d/topic/snorby/te4R1E6-VH4/discussion'>https://groups.google.com/d/topic/snorby/te4R1E6-VH4/discussion</a>

We now delete the pid file at boot by default, so you should just be able to reboot to resolve this issue:<br>
<a href='http://blog.securityonion.net/2013/11/new-snort-nsm-and-sostat-packages.html'>http://blog.securityonion.net/2013/11/new-snort-nsm-and-sostat-packages.html</a>

<h1>How do I access Xplico with a hostname instead of IP address?</h1>
From Gianluca Costa:<br>
<br>
Xplico has embedded (in its PHP code) a Http-proxy, this proxy is used to show the web pages, emulating, for example, the original cache of the user.<br>
By default the XI url must be an IP address (wiki: <a href='http://wiki.xplico.org/doku.php?id=interface#browser'>http://wiki.xplico.org/doku.php?id=interface#browser</a> ), the only exception to this rule is the url <a href='http://demo.xplico.org'>http://demo.xplico.org</a> (for obvious reasons).<br>
If you use as url a name (not an ip) then XI give you a blank page, because XI searches your url in the decoded data.<br>
<br>
To change this behavior you must modify the PHP code:<br>
<blockquote>- file /opt/xplico/xi/cake/dispatcher.php<br>
- replace demo.xplico.org with your host name (used in the url)<br></blockquote>

<h1>Why does Bro log "Failed to open GeoIP database" and "Fell back to GeoIP Country database"?</h1>

The GeoIP CITY database is not free and thus we cannot include it in the distro.  Bro fails to find it and falls back to the GeoIP COUNTRY database (which is free).  As long as you are seeing some country codes in your conn.log, then everything should be fine.  If you really need the CITY database, see this thread for some options:<br>
<a href='https://groups.google.com/d/topic/security-onion-testing/gtc-8ZTuCi4/discussion'>https://groups.google.com/d/topic/security-onion-testing/gtc-8ZTuCi4/discussion</a>

<h1>What's the difference between a "server" and a "sensor"?</h1>
box<br>
Definition: A physical or virtual machine running the Security Onion operating system.<br>
<br>
server<br>
Definition: A set of processes that receive data from sensors and allow analysts to see and investigate that data.  The set of processes includes sguild, mysql, and snorby.  The server is also responsible for ruleset management.<br>
Naming convention: The collection of server processes has a server name separate from the hostname of the box.  Security Onion always sets the server name to "securityonion".<br>
Configuration files: /etc/nsm/securityonion/<br>
Controlled by:  /usr/sbin/nsm_server <br>
<br>
server box<br>
Definition: A machine running the server processes.  May optionally be running sensor processes.<br>
Example 1: User runs Quick Setup on machine with hostname securityonion and two ethernet interfaces.  Setup creates a server and two sensors (securityonion-eth0 and securityonion-eth1).<br>
Example 2: User runs Advanced Setup and chooses Server.  Setup creates a server only (no sensor processes).<br>
<br>
sensor<br>
Definition: A set of processes listening on a network interface.  The set of processes currently includes Snort/Suricata, netsniff-ng, argus, prads, and bro (although this is in constant flux as we add new capabilities and find better tools for existing capabilities).<br>
Naming convention: $HOSTNAME-$INTERFACE<br>
Configuration files: /etc/nsm/$HOSTNAME-$INTERFACE/<br>
Example: sensor1-eth0<br>
Controlled by:  /usr/sbin/nsm_sensor<br>
<br>
sensor box<br>
Definition: A machine having one or more sensors that transmit to a central server.  Does not run server processes.  Pulls ruleset from server box.  (In some contexts, I refer to this a slave pulling rules from the master.)<br>
Example: A machine named sensor1 having sensors sensor1-eth0 and sensor1-eth1.<br>

<h1>Why does the ELSA web interface not recognize one of my ELSA log nodes even though the APIKEY is correct?</h1>
Could be due to clocks not matching between ELSA log node and ELSA web interface.  Please see: <a href='https://groups.google.com/d/topic/security-onion/K_5vWQpd8VM/discussion'>https://groups.google.com/d/topic/security-onion/K_5vWQpd8VM/discussion</a>

<h1>Why do apt-get and the Update Manager show tcl8.5 as held back?</h1>
<a href='tcl.md'>tcl notes</a>

<h1>Why do I get the following error when starting Sguil?</h1>
<pre><code>Application initialization failed: no display name and no $DISPLAY environment variable<br>
ERROR: Cannot fine the Iwidgets extension.<br>
The iwidgets package is part of the incr tcl extension and is<br>
available as a port/package most systems.<br>
See http://www.tcltk.com/iwidgets/ for more info.<br>
</code></pre>
This is related to the previous question.  See <a href='tcl.md'>tcl notes</a>.<br>
<br>
<h1>Why do I get segfaults when booting on VMware ESX?</h1>
This is a known issue with Ubuntu 10.04 and ESXi 4.1 and is unrelated to Security Onion.  Please see:<br>
<a href='http://ubuntuforums.org/showthread.php?t=1674759'>http://ubuntuforums.org/showthread.php?t=1674759</a><br>
<a href='https://bugs.launchpad.net/ubuntu/+source/linux/+bug/659422'>https://bugs.launchpad.net/ubuntu/+source/linux/+bug/659422</a><br>

<h1>Why is the Snorby dashboard not displaying correctly?  Why do I have to restart the Sensor Cache job periodically?</h1>
All known issues with the dashboard and sensor cache job have been resolved with the release of Snorby 2.5.1 in Security Onion 20120321:<br>
<a href='http://blog.securityonion.net/2012/03/security-onion-20120321-now-available.html'><a href='http://blog.securityonion.net/2012/03/security-onion-20120321-now-available.html'>http://blog.securityonion.net/2012/03/security-onion-20120321-now-available.html</a></a>

<h1>How do I run ntop on Security Onion?</h1>
In the old Security Onion 10.04, Snorby was running on port 3000 (which ntop also defaults to).  In Security Onion 12.04, Snorby no longer runs on port 3000, so this shouldn't be an issue.<br>
<br>
ntop defaults to port 3000, which is already being used by Snorby.  You can change ntop's port by editing /etc/default/ntop like this:<br>
<pre><code>GETOPT="-w 0 -W 4000"<br>
</code></pre>
This will disable http and enable https on port 4000.  Thanks to Rod Green for the tip!<br>
<br>
<h1>Why can't I upgrade past version 20110628?</h1>

If your /etc/nsm/securityonion.conf says:<br>
<pre><code>VERSION=20110628<br>
</code></pre>
<br>
and if running the upgrade says:<br>
<pre><code>Your Security Onion installation is up to date.<br>
</code></pre>
<br>
then a previous upgrade from 20110628 to 20110709 was interrupted.  This upgrade changed the config file format from spaces to equal signs.  Edit the VERSION in /etc/nsm/securityonion.conf as follows:<br>
<pre><code>VERSION=20110709<br>
</code></pre>
<br>
and then re-run the upgrade.<br>