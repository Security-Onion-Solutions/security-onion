# Please read the following before downloading! #

This page is for the old Security Onion based on Ubuntu 10.04.  You shouldn't be using this old version anymore.  Please use the new Security Onion 12.04.  See the [Installation page](Installation.md).

# What's the recommended procedure for installing Security Onion? #

## If you just want to quickly evaluate Security Onion, follow these steps: ##
  1. Hardware requirements: you'll need at least 1GB RAM for each monitored network interface.  Be aware that full packet capture may fill your disk quickly, so size your storage appropriately.<br>
<ol><li><a href='http://sourceforge.net/projects/security-onion/files/'>Download the ISO image</a>, verify its checksum, and boot from it.<br>
</li><li>Run through the Xubuntu installer.<br>
</li><li>Reboot into your new installation and double-click the Setup shortcut.  Follow the prompts.<br>
</li><li>Analyze alerts using Sguil, Squert, or Snorby.<br>
<br>
<h2>If you're going to be deploying Security Onion in production, follow these steps:</h2>
</li><li>Before you start, make sure your sensor(s) are capable.  Check the <a href='Hardware.md'>Hardware</a> page.<br>
</li><li><a href='http://sourceforge.net/projects/security-onion/files/'>Click here and scroll to the bottom of the page.</a>  Click the ISO image to begin the download.  Also download/record the MD5/SHA1 checksums for the ISO image. There is an accompanying .md5 file or you can use the MD5/SHA1 checksums that Sourceforge displays when clicking the Information (view details) button to the right of the ISO image (it's a circle with an "i"). Once the ISO image download has completed, verify the checksum.<br>
</li><li>(If deploying a master server and one or more slave sensors, you'll need to perform the remaining steps on the server and all sensors, but make sure you install/configure the master first.)<br>
</li><li>Boot the ISO and select Install (either from the boot menu or from the Live desktop).<br>
</li><li>Follow the prompts in the installer.  When asked about partitioning, there are a few things to keep in mind:<br>
<ul><li>If you have more than 2TB of disk space, you will probably want to create a dedicated /boot partition at the beginning of the disk to ensure that you don't have any Grub booting issues.<br>
</li><li>The Sguil database on the server (doesn't exist on sensor-only installations) can grow fairly large (100GB or more for decent-size networks).  It's stored at /var/lib/mysql/, so you may want to put /var on a dedicated partition/disk and assign a good amount of disk space to it.  Also see the DAYSTOKEEP instructions near the end of this procedure.<br>
</li><li>Sensors store full packet captures at /nsm/sensor_data/, so you may want to put /nsm on a dedicated partition/disk and assign as much disk space as possible (1TB or more).<br>
</li></ul></li><li>Reboot into your new installation and login with the credentials you specified in the installer.<br>
</li><li><a href='http://code.google.com/p/security-onion/wiki/NetworkConfiguration'>Configure networking.</a><br>
</li><li>If you're behind a proxy, <a href='http://code.google.com/p/security-onion/wiki/Proxy'>configure your proxy settings</a>.<br>
</li><li>If your machine has more than 4GB of RAM, <a href='https://help.ubuntu.com/community/EnablingPAE'>install the PAE kernel</a>.<br>
</li><li>Increase kernel network buffers:<br>
<pre><code>echo "net.core.netdev_max_backlog = 10000<br>
net.core.rmem_default = 50000000<br>
net.core.rmem_max = 50000000<br>
net.ipv4.tcp_mem = 194688 259584 389376<br>
net.ipv4.tcp_rmem = 1048576 4194304 33554432" | sudo tee /etc/sysctl.d/30-securityonion.conf<br>
sudo sysctl -p /etc/sysctl.d/30-securityonion.conf<br>
</code></pre>
</li><li>Install Ubuntu updates using the Update Manager in the upper right corner OR from the command-line using the following:<br>
<pre><code>sudo apt-get update &amp;&amp; sudo apt-get dist-upgrade<br>
</code></pre>
<ul><li><a href='http://code.google.com/p/security-onion/wiki/tcl'>tcl/tk updates are on hold and you should NOT attempt to manually install any tcl/tk updates!</a><br>
</li><li>You may want to <a href='https://help.ubuntu.com/community/AutomaticSecurityUpdates'>configure Ubuntu to automatically install updates</a>.<br>
</li><li>Now that Ubuntu 12.04.1 has been released, it will be offered as an upgrade.  <a href='http://securityonion.blogspot.com/2012/08/security-onion-and-ubuntu-12041.html'>Do NOT accept this offer to upgrade</a>.<br>
</li></ul></li><li><a href='http://code.google.com/p/security-onion/wiki/Upgrade'>Install Security Onion updates</a>.<br>
</li><li>Reboot.<br>
</li><li>Double-click the Setup shortcut on the Desktop and follow the prompts.  If this is a sensor that will report to a master server, then Setup will prompt you for an SSH account on the master server that has sudo privileges.  (Note: the management interface on the sensor must be able to SSH to the management interface on the server, so please make sure that you have network connectivity and no firewall rules that would block this traffic.)  Consider creating a separate SSH account on the master server for each sensor so that if a sensor is ever compromised, its individual account can be disabled without affecting the other sensors.  To do this, create a new user using the adduser command and then add the user to the admin group.  Once Setup is complete, the user can be removed from the admin group.  For example, suppose you're setting up a server and two separate sensors:<br>
<ul><li>SERVER<br>
<ul><li>run through Setup<br>
</li></ul></li><li>FIRST SENSOR<br>
<ul><li>create an account on the SERVER and add it to the admin group<br>
</li><li>run through Setup on the SENSOR<br>
</li><li>on the SERVER, remove the account from the admin group, but leave the account active<br>
</li></ul></li><li>SECOND SENSOR<br>
<ul><li>create a second account on the SERVER and add it to the admin group<br>
</li><li>run through Setup on the second SENSOR<br>
</li><li>on the SERVER, remove the account from the admin group, but leave the account active<br>
</li></ul></li></ul></li><li>If you're monitoring IP address ranges other than the private RFC1918 address space (192.168.0.0/16,10.0.0.0/8,172.16.0.0/12), you should update your sensor configuration with the correct IP ranges.  Sensor configuration files can be found in /etc/nsm/HOSTNAME-INTERFACE/.  Modify either snort.conf or suricata.yaml (depending on which IDS engine you chose during Setup) and update the HOME_NET variable.  Also update the HOME_NET variable in sancp.conf (may just want to use 0.0.0.0) and the network variable in pads.conf.  Then update Bro's network configuration in /usr/local/etc/networks.cfg.  Finally, restart the sensor processes:<br>
<pre><code>sudo nsm_sensor_ps-restart<br>
</code></pre>
</li><li>If you have Internet access, create an IDS alert by typing the following at a terminal:<br>
<pre><code>curl http://testmyids.com<br>
</code></pre>
</li><li>Full-time analysts should run Security Onion in a VM on their workstation, then launch the Sguil client and connect to the IP/hostname of their production Sguil sensor.  This allows you to investigate pcaps without fear of impacting your production server/sensors.  To change the resolution of your Security Onion VM, install the Virtual Tools for your virtualization solution or use xrandr.  For a list of available screen resolutions, simply execute "xrandr".  To set the screen resolution (replace W and H with the actual Width and Height desired):<br>
<pre><code>xrandr -s WxH<br>
</code></pre>
</li><li>Login to Sguil, Squert (<a href='https://server/squert/'>https://server/squert/</a>), or Snorby (<a href='https://server:3000'>https://server:3000</a>) and review your IDS alerts.  Use the additional NSM data types in Sguil and Bro logs (/nsm/bro/logs/) for in-depth analysis.<br>
</li><li>Be aware that Snorby uses highcharts.  Read the <a href='http://shop.highsoft.com/highcharts.html'>highcharts license</a> to determine if you need to purchase a commercial license.  If you feel that you would be required to purchase a commercial license but are unwilling/unable to, please <a href='http://code.google.com/p/security-onion/wiki/FAQ'>disable/remove Snorby</a> (See: "How do I disable Snorby?").<br>
</li><li>Harden your server and sensors by disabling any unneeded services and <a href='Firewall.md'>firewalling</a> off any unused ports.<br>
</li><li>Run the following to see how your sensor is coping with the load.  You should check this on a daily basis to make sure your sensor is not dropping packets.  Consider adding it to a cronjob and having it emailed to you (see the "configure email" link below).<br>
<pre><code>sudo sostat | less<br>
</code></pre>
</li><li>Please note that any IDS/NSM system needs to be tuned for the network it's monitoring.  Please see <a href='ManagingAlerts.md'>ManagingAlerts</a>.  You should only run the signatures you really care about.<br>
</li><li>Also note that you should be looking at and categorizing events every day with the goal being to categorize all events every day.  Even if you don't use the Sguil console for your primary analysis, you need to log into into it periodically and F8 old events to keep the RealTime queue from getting too big.  Neglecting to do so may result in database/Sguil issues as the number of uncategorized events continues to increase on a daily basis.  Please see the <a href='http://nsmwiki.org/Sguil_Client'>Sguil client page on NSMwiki</a>.<br>
</li><li>On the server running the Sguil database, set the DAYSTOKEEP variable in /etc/nsm/securityonion.conf to however many days you want to keep in your archive.  The default is 365, but you may need to adjust it based on your organization's detection/response policy and your available disk space.<br>
</li><li>You should also tune <a href='http_agent.md'>http_agent</a>.<br>
</li><li>Optional: Exclude unnecessary traffic from your monitoring using <a href='BPF.md'>BPF</a>.<br>
</li><li>Optional: add new Sguil user accounts with the following:<br>
<pre><code>sudo /usr/local/sbin/nsm_server_user-add<br>
</code></pre>
</li><li>Optional, but highly recommended: <a href='Email.md'>configure email for alerting and reporting</a>
</li><li>Optional: Need "remote desktop" access to your Security Onion sensor? Install <a href='FreeNX.md'>FreeNX</a> or <a href='http://www.xrdp.org/'>xrdp</a> (sudo apt-get install xrdp).<br>
</li><li>Read more about the tools contained in Security Onion: <a href='Tools.md'>Tools</a>
</li><li>Refer back to this document often, as it is updated quite regularly.