# Introduction #

**NOTE!** We're no longer in Beta.  Please see the [Installation page](Installation.md).

All of our packages are in an Ubuntu Launchpad PPA.  That means you can start with your preferred flavor of Ubuntu 12.04, add our PPA and packages, and be up and running in just a few minutes!

# Warning! #
Please keep in mind that this is not ready for production usage yet.  We're still in BETA and I reserve the right to break all the packages in the "test" repo at any time!

# RAM Minimum Requirements #
512MB - 1GB RAM for the core OS (512MB for Ubuntu Server with no GUI)<br>
+512MB RAM for the server components (apache, sguild, Snorby, ELSA web, etc.)<br>
+1GB RAM for EACH network interface that you choose to monitor <br>
+ >=512MB RAM if you choose to enable ELSA<br>
<br>
<h1>Hardware Recommendations</h1>
<ul><li>64-bit<br>
</li><li>Intel NICs<br>
</li><li>as much RAM as your server holds!<br>
</li><li>as much Disk as your server holds!<br></li></ul>

<h1>Ubuntu 12.04 vs Ubuntu 12.10</h1>
Ubuntu 12.04 is an LTS (Long Term Support) release and is fully tested/supported for Security Onion.<br>

Ubuntu 12.10 is NOT an LTS release and is NOT tested/supported for Security Onion.<br>
<br>
<h1>Installation Instructions</h1>

<ul><li>Please read these instructions CAREFULLY and follow them EXACTLY.  Commands have been formatted for easy copy/paste to avoid typos.<br>
</li><li>Download your desired flavor of Ubuntu 12.04.  You should be able to use 32-bit or 64-bit, but 64-bit is highly recommended (assuming your hardware supports it):<br>
<ul><li>Ubuntu Server (no GUI)- <a href='http://www.ubuntu.com/download/server'>http://www.ubuntu.com/download/server</a>
</li><li>Ubuntu Desktop - <a href='http://www.ubuntu.com/download/desktop'>http://www.ubuntu.com/download/desktop</a>
</li><li>Kubuntu - <a href='http://www.kubuntu.org/getkubuntu/download'>http://www.kubuntu.org/getkubuntu/download</a>
</li><li>Xubuntu - <a href='http://xubuntu.org/getxubuntu/'>http://xubuntu.org/getxubuntu/</a>
</li><li>Lubuntu - <a href='https://help.ubuntu.com/community/Lubuntu/GetLubuntu'>https://help.ubuntu.com/community/Lubuntu/GetLubuntu</a>
</li></ul></li><li>Verify the checksum of the downloaded ISO image.<br>
</li><li>Boot the ISO image.<br>
</li><li>Proceed through the Ubuntu installer.  If prompted to select additional packages, only choose openssh-server (specifically do NOT choose MySQL).  Our packages will automatically install any dependencies later.<br>
</li><li>Reboot into your new installation.<br>
</li><li>If you installed a GUI, login using the username/password you specified in the Ubuntu installer.  If you installed Ubuntu Server (no GUI), you'll want to SSH into your new installation from another machine (with an X server) using the -X option to enable X-Forwarding:<br>
<pre><code>ssh -X username@ip.of.securityonion.server<br>
<br>
# Once logged in, you may get a message concerning .Xauthority.  <br>
# This file should be created automatically, but if it doesn't (occasionally this has happened on AWS instances), you can create it manually:<br>
touch ~/.Xauthority<br>
</code></pre>
</li><li>IMPORTANT! Apply any available Ubuntu updates (including new kernel packages) and REBOOT (this is required for the pf_ring DKMS module to build properly):<br>
<pre><code>sudo apt-get update &amp;&amp; sudo apt-get dist-upgrade &amp;&amp; sudo reboot<br>
</code></pre>
</li><li>If you're running a VM, now would be a good time to snapshot it so you can revert later if you need to.<br>
</li><li>Log back in (over "ssh -X" if remote) and configure MySQL not to prompt for root password:<br>
<pre><code>echo "debconf debconf/frontend select noninteractive" | sudo debconf-set-selections  <br>
</code></pre>
</li><li>Add our test repo and install ALL Security Onion packages.  This could take 15 minutes or more depending on the speed of your CPU and Internet connection:<br>
<pre><code>sudo apt-get -y install python-software-properties<br>
sudo add-apt-repository -y ppa:securityonion/test<br>
sudo apt-get update<br>
sudo apt-get -y install securityonion-all <br>
</code></pre>
</li><li>If you got any errors when installing the Security Onion packages, please see the DKMS/pf_ring known issue below.<br>
</li><li>Run the Setup wizard (if you get a zenity Gtk-WARNING, then you need to make sure you used "ssh -X" to enable X-Forwarding):<br>
<pre><code>sudo sosetup<br>
</code></pre>
</li><li>The Setup wizard will walk you through configuring /etc/network/interfaces and will then reboot.<br>
</li><li>After rebooting, log back in (over "ssh -X" if remote) and start the Setup wizard again.  It will detect that you have already configured /etc/network/interfaces and will walk your through the rest of the configuration.<br>
</li><li>Once you've completed the Setup wizard, open the Chromium web browser and go to <a href='https://localhost'>https://localhost</a> for links to Squert, Snorby, and ELSA.<br>
</li><li>To launch the Sguil client, find the icon in the Security Onion menu (Xubuntu/Lubuntu) or just type:<br>
<pre><code>sguil.tk<br>
</code></pre></li></ul>

<h1>Metapackages</h1>
The instructions above used the securityonion-all metapackage to install ALL of our individual packages (about 350MB).  We have some more focused metapackages that you can use for a more focused deployment:<br>
<ul><li>securityonion-client (about 50MB)<br>
</li><li>securityonion-sensor (about 50MB)<br>
</li><li>securityonion-elsa and securityonion-elsa-extras (about 50MB)<br>
</li><li>securityonion-server (about 200MB)</li></ul>

<h1>Known Issues</h1>

<h3>DKMS unable to build pf_ring kernel module</h3>
If you forgot to run the following command before installing our packages:<br>
<pre><code>sudo apt-get update &amp;&amp; sudo apt-get dist-upgrade &amp;&amp; sudo reboot<br>
</code></pre>
then you could get into a situation where DKMS is unable to build your pf_ring kernel module because you're not running the most recent kernel.  This will result in cascading errors in our package installation.  The good news is it looks like if you then run that command, it should sort everything out:<br>
<pre><code>sudo apt-get update &amp;&amp; sudo apt-get dist-upgrade &amp;&amp; sudo reboot<br>
</code></pre>
After the machine restarts, check to see if the pf_ring module got built and loaded correctly:<br>
<pre><code>lsmod |grep pf_ring<br>
</code></pre>
If there is no output (the pf_ring module didn't get built/loaded), you can try reinstalling the pfring-module package:<br>
<pre><code>sudo apt-get install --reinstall securityonion-pfring-module<br>
</code></pre>
If it still fails, start over with a fresh Ubuntu installation and follows the steps at the top of the page exactly.<br>
<br>
<h3>Kernel panics when running Suricata with multiple PF_RING threads</h3>
Some users have reported kernel panics when running Suricata with multiple PF_RING threads.  If you experience this, try changing IDS_LB_PROCS in /etc/nsm/$SENSOR/sensor.conf to 1 (and restart Suricata) until we've discovered the root cause of this.<br>
<br>
<h3>Proxy configuration</h3>
If you use a proxy on your network for outbound Internet connections, you may need to configure Ubuntu for your proxy to allow it to download updates.  Please see our <a href='Proxy.md'>Proxy</a> page and make sure you set all 3 variables (http_proxy, https_proxy, and ftp_proxy).  If you still have issues downloading updates through your proxy, please see this note from Brian Sims:<br>
"it doesn't look like add-apt-repository, as noted in the install instructions plays nice, by default, with http proxies.   With both the http_proxy environment variable configured as well as being in apt.conf (apt itself works, as does wget, etc), add-apt-respository eventually just fails with a cryptic "Error reading /securityonion/test/ubuntu: couldn't connect to host".    The repo is added to /etc/apt/sources.list.d, but the part that fails is the key retrieval.  Looks like a fairly common problem, solution seems to be to use apt-key afterwards, where you can specify a proxy as an argument on the command line, last argument is the SO keyid."<br>
<pre><code>sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --keyserver-options http-proxy="http://user:pass@foo.bar:3128" --recv-keys E1E6759023F386C7<br>
</code></pre>

<h3>OSSEC Upgrade</h3>
If you're currently testing the new Security Onion 12.04 Beta and installed the packages before October 4 and do an "apt-get upgrade", the OSSEC package may require some manual intervention:<br>
<ul><li>if prompted about /etc/ossec-init.conf, just press Enter to accept the default of "N, keep the current version"<br>
</li><li>afterwards, if OSSEC isn't running, then you'll need to purge the package and reinstall it:<br>
<pre><code>sudo apt-get purge ossec-hids-server <br>
</code></pre>
</li><li>this will remove some securityonion metapackages, so let's reinstall them (and OSSEC):<br>
<pre><code>sudo apt-get -y install securityonion-all <br>
</code></pre></li></ul>

<h3>PRADS</h3>
PRADS is a replacement for both PADS and SANCP.  Known issues with PRADS:<br>
<a href='https://github.com/gamelinux/prads/issues/19'>https://github.com/gamelinux/prads/issues/19</a><br>

<h3>netsniff-ng</h3>
We've replaced daemonlogger with netsniff-ng for higher performance, but it's currently running as root.  The next version of netsniff-ng will support running as a non-root user.<br>
<br>
If you have any problems with netsniff-ng starting, you may still have the old netsniff-ng package.  You can remove it by doing the following:<br>
<pre><code>sudo apt-get update &amp;&amp; sudo apt-get dist-upgrade &amp;&amp; sudo apt-get remove --purge netsniff-ng<br>
</code></pre>
Also be aware that netsniff-ng will fail on startup if it doesn't detect link on the NIC.<br>
<br>
<h3>sphinxsearch</h3>
There is a known issue with sphinxsearch (used by ELSA) not starting properly on boot.  As a quick-and-dirty bandaid until we have a proper fix, I modified my /etc/init.d/sphinxsearch as follows:<br>
changed the "start)" case to "oldstart)" and then made "start" a synonym for restart by changing:<br>
<pre><code>restart|reload|force-reload) <br>
</code></pre>
to:<br>
<pre><code>start|restart|reload|force-reload) <br>
</code></pre>

<h3>ELSA</h3>
The following ELSA issues are known and are being worked on:<br>
<ul><li>GeoIP<br>
</li><li>Incorrect or missing parsers for Bro's dns.log, http.log, notice.log, capture_loss.log and software.log<br>
</li><li>UnixDate error message</li></ul>

<h1>Feedback</h1>
If you have questions or problems, please send a detailed email to our TESTING mailing list (NOT our normal 10.04 mailing list):<br>
<a href='MailingLists.md'>Security Onion Mailing Lists</a>