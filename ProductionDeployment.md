## If you're going to be deploying Security Onion in production, follow these steps: ##
  1. First, check the [Hardware Requirements](Hardware.md) page.
  1. Download the ISO image for your preferred flavor of Ubuntu 12.04 32-bit/64-bit or download our Security Onion ISO image using the instructions [here](QuickISOImage.md). Also download/record the MD5/SHA1 checksums for the ISO image and verify the checksum when the download has completed.<br>
<ol><li>If deploying a distributed environment (a master server and one or more slave sensors), you’ll need to perform the remaining steps on the server and all sensors, but make sure you install/configure the master server first.  For best performance, the master server should be dedicated to just being a server for the other sensor boxes (the master server should have no sniffing interfaces of its own).  Please note that sensors need to connect to the master server on ports 22 and 7736.  If you choose to enable salt for sensor management, they will also need to be able to connect to the master server on ports 4505 and 4506.<br>
</li><li>Using the downloaded ISO, install the operating system. If prompted with an "encrypt home folder" option, DO NOT enable this feature.  If asked about automatic updates, DO NOT enable automatic updates.  If prompted to install any additional packages, the only option you should choose is OpenSSH Server (openssh-server). Specifically, do NOT choose MySQL. All other required dependencies will be installed automatically.<br>
</li><li>When asked about partitioning, there are a few things to keep in mind:<br>
<ul><li>If you have more than 2TB of disk space, you will probably want to create a dedicated /boot partition at the beginning of the disk to ensure that you don’t have any Grub booting issues.<br>
</li><li>The Sguil database on the server (doesn’t exist on sensor-only installations) can grow fairly large (100GB or more for decent-size networks). It’s stored at /var/lib/mysql/, so you may want to put /var on a dedicated partition/disk and assign a good amount of disk space to it. Also see the DAYSTOKEEP instructions on the Post-Installation page.<br>
</li><li>Sensors store full packet captures at /nsm/sensor_data/, so you may want to put /nsm on a dedicated partition/disk and assign as much disk space as possible (1TB or more). For larger volumes you might also consider using XFS for the /nsm partition.<br>
</li></ul></li><li>When installation completes, reboot into your new installation and login with the credentials you specified during installation.<br>
</li><li>If you’re running a VM, now would be a good time to snapshot it so you can revert later if you need to.<br>
</li><li>Verify that you have Internet connectivity. If necessary, configure your <a href='Proxy.md'>proxy settings</a>.<br>
</li><li>If you installed from the Security Onion Live 12.04 Distribution ISO, run "sudo soup", reboot if prompted, and then skip to the "Setup wizard" step below.<br>
</li><li>If your machine is running a 32 bit version of Ubuntu and you have more than 4GB of RAM, <a href='https://help.ubuntu.com/community/EnablingPAE'>install the PAE kernel</a>.<br>
</li><li>Install all Ubuntu updates and reboot.<br>
</li><li>Log back in (over “ssh -X” if remote) and configure MySQL not to prompt for root password:<br>
<pre><code>echo "debconf debconf/frontend select noninteractive" | sudo debconf-set-selections<br>
</code></pre>
<ul><li>If using “-X”, Once logged in, you may get a message concerning the .Xauthority file. This file should be created automatically, but if it doesn't (occasionally this has happened on AWS instances), you can create it manually:<br>
<pre><code>touch ~/.Xauthority<br>
</code></pre>
</li></ul></li><li>Install python-software-properties if it's not already installed:<br>
<pre><code>sudo apt-get -y install python-software-properties<br>
</code></pre>
</li><li>Add the Security Onion stable repository:<br>
<pre><code>sudo add-apt-repository -y ppa:securityonion/stable<br>
</code></pre>
</li><li>If you got a ValueError when running the previous command, please see the following note from jonh; otherwise, continue to the next step.  "when I did the <code>add-apt-repository -y ppa:securityonion/stable</code> command it gave a bunch of lines of errors with the final line being <code>ValueError: cannot convert float NaN to integer</code>. This appears to be a known bug: <a href='https://bugs.launchpad.net/ubuntu/+source/software-properties/+bug/1063350'><a href='https://bugs.launchpad.net/ubuntu/+source/software-properties/+bug/1063350'>https://bugs.launchpad.net/ubuntu/+source/software-properties/+bug/1063350</a></a>. The reported workaround of downgrading python-software-properties got me past that error."<br>
<pre><code># Only run this if you got a ValueError in the previous step!<br>
sudo apt-get install python-software-properties=0.82.7<br>
</code></pre>
</li><li>Update:<br>
<pre><code>sudo apt-get update<br>
</code></pre>
</li><li>Install the securityonion-all metapackage (or one of the more focused <a href='MetaPackages.md'>metapackages</a>). This could take 15 minutes or more depending on the speed of your CPU and Internet connection.<br>
<pre><code>sudo apt-get -y install securityonion-all<br>
</code></pre>
</li><li>OPTIONAL: If you want to use Salt to manage your deployment, also install securityonion-onionsalt.  You can do this before or after Setup, but it's much easier if you do it before Setup. <a href='https://code.google.com/p/security-onion/wiki/Salt'>https://code.google.com/p/security-onion/wiki/Salt</a><br>
<pre><code>sudo apt-get -y install securityonion-onionsalt<br>
</code></pre>
</li><li>Run the Setup wizard (if you get a zenity Gtk-WARNING, then you need to make sure you used "ssh -X" to enable X-Forwarding):<br>
<pre><code>sudo sosetup<br>
</code></pre>
</li><li>The Setup wizard will walk you through configuring /etc/network/interfaces and will then reboot.<br>
<ul><li>When prompted whether you would like to configure /etc/network/interfaces now, choose “Yes, configure /etc/network/interfaces!.”<br>
</li><li>If you have more than one network interface, you’ll be asked which to specify which one should be the management interface.<br>
</li><li>You’ll then be asked to choose DHCP or static addressing for the management interface. It is highly recommended you choose static.<br>
</li><li>Choosing static, you’ll be prompted to enter a static IP address for your management interface, the network’s subnet mask, gateway IP address, DNS server IP addresses (separated by spaces), and your local domain.<br>
</li><li>You’ll then be prompted to select any additional interfaces that will be used for sniffing/monitoring network traffic.<br>
</li><li>When prompted, choose “Yes, make changes!"<br>
</li><li>If you need to adjust any network settings manually (e.g. MTU), you may edit /etc/network/interfaces before rebooting.<br>
</li><li>When ready to reboot, click "Yes, reboot!”<br>
</li></ul></li><li>After rebooting, log back in (over “ssh -X” if remote) and start the Setup wizard again (sudo sosetup). It will detect that you have already configured /etc/network/interfaces and will walk you through the rest of the configuration.<br>
</li><li>Select Advanced Setup.<br>
</li><li>Choose whether the host being configured will be Standalone, Server, or Sensor.  If deploying a distributed environment (a master server and one or more slave sensors), the master server should be dedicated to just being a server for the other sensor boxes (the master server should have no sniffing interfaces of its own).  So the first box should be configured using "Server" and the remaining boxes should be configured using "Sensor".<br>
<ul><li>Standalone<br>
<ul><li>You will be prompted to specify which IDS Engine (Snort or Suricata) you would like to use.<br>
</li><li>If you have multiple CPU cores available:<br>
<ul><li>You will be prompted to designate how many IDS processes you would like to run. (This setting can be modified later by changing the IDS_LB_PROCS variable in /etc/nsm/HOSTNAME-INTERFACE/sensor.conf).<br>
</li><li>You will be prompted to designate how many Bro processes you would like to run. (This setting can be modified later by changing the lb_procs variable in /opt/bro/etc/node.cfg).<br>
</li></ul></li><li>You’ll be asked which IDS ruleset you would like to use.<br>
</li><li>You will then be prompted for user account information for Sguil, Squert, ELSA and Snorby.<br>
</li><li>You’ll be asked whether you want to enable ELSA.<br>
</li><li>You’ll be prompted to proceed with making the changes to setup Security Onion.<br>
</li></ul></li><li>Server<br>
<ul><li>You will be prompted to specify which IDS Engine (Snort or Suricata) you would like to use.<br>
</li><li>If you have multiple CPU cores available:<br>
<ul><li>You will be prompted to designate how many IDS processes you would like to run. (This setting can be modified later by changing the IDS_LB_PROCS variable in /etc/nsm/HOSTNAME-INTERFACE/sensor.conf).<br>
</li><li>You will be prompted to designate how many Bro processes you would like to run. (This setting can be modified later by changing the lb_procs variable in /opt/bro/etc/node.cfg).<br>
</li></ul></li><li>You’ll be asked which IDS ruleset you would like to use.<br>
</li><li>You will then be prompted for user account information for Sguil, Squert, ELSA and Snorby.<br>
</li><li>You’ll be asked whether you want to enable ELSA.<br>
</li><li>You’ll be prompted to proceed with making the changes to setup Security Onion.<br>
</li></ul></li><li>Sensor<br>
<ul><li>You will be prompted for an SSH account on the master server that has sudo privileges. (Note: the management interface on the sensor must be able to SSH to the management interface on the server, so please make sure that your server has been set up and you have network connectivity and no firewall rules that would block this traffic.) Consider creating a separate SSH account on the master server for each sensor so that if a sensor is ever compromised, its individual account can be disabled without affecting the other sensors. To do this, create a new user using the adduser command and then add the user to the sudo group. Once Setup is complete, the user can be removed from the sudo group. For example, suppose you’re setting up a server and two separate sensors:<br>
<ul><li>SERVER<br>
<ul><li>run through sosetup<br>
</li></ul></li><li>FIRST SENSOR<br>
<ul><li>create an account on the SERVER and add it to the sudo group<br>
<ul><li>The account created must have a full home directory. If you do not create it when you create the account, copy /etc/skel to /home/$user and do chown -R $user:$user /home/$user , where "$user" is the username of the new account. This is needed so the .ssh directory may be created to manage the connection.<br>
</li></ul></li><li>run through sosetup on the SENSOR<br>
</li><li>on the SERVER, remove the account from the sudo group, but leave the account active<br>
</li></ul></li><li>SECOND SENSOR<br>
<ul><li>create a second account on the SERVER and add it to the sudo group<br>
</li><li>run through SETUP on the second SENSOR<br>
</li><li>on the SERVER, remove the account from the sudo group, but leave the account active<br>
</li></ul></li></ul></li><li>You’ll be asked which network interface should be monitored.<br>
</li><li>If you have multiple CPU cores available:<br>
<ul><li>You will be prompted to designate how many IDS processes you would like to run. (This setting can be modified later by changing the IDS_LB_PROCS variable in /etc/nsm/HOSTNAME-INTERFACE/sensor.conf).<br>
</li><li>You will be prompted to designate how many Bro processes you would like to run. (This setting can be modified later by changing the lb_procs variable in /opt/bro/etc/node.cfg).<br>
</li></ul></li><li>You’ll be asked whether you want to enable ELSA. (ELSA is a distributed log archiving platform, so each sensor’s events stored in ELSA will be stored locally at each sensor and queried from the server which acts as a central interface.)</li></ul></li></ul></li></ol>

Proceed to <a href='PostInstallation.md'>Post Installation</a>.