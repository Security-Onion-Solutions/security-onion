## If you want to quickly evaluate Security Onion on your preferred flavor of Ubuntu 12.04 32-bit/64-bit (not using our ISO image), follow these steps: ##
  1. First, check the [Hardware Requirements](Hardware.md) page.
  1. Download the ISO image for your preferred flavor of Ubuntu 12.04.5, verify its checksum,  and boot from it.<br>
<ol><li>Follow the prompts in the installer, but see the two notes below first.<br>
<ul><li>When prompted to "encrypt home folder" option, DO NOT enable this feature.<br>
</li><li>When asked about automatic updates, DO NOT enable automatic updates.<br>
</li></ul></li><li>Reboot into your new installation.<br>
</li><li>Login using the username/password you specified during installation.<br>
</li><li>Verify that you have Internet connectivity.  If necessary, configure your <a href='Proxy.md'>proxy settings</a>.<br>
</li><li>Log back in (using “ssh -X” if you’re installing on Ubuntu Server or a headless distro).<br>
</li><li>Configure MySQL not to prompt for root password:<br>
<pre><code>echo "debconf debconf/frontend select noninteractive" | sudo debconf-set-selections<br>
</code></pre>
</li><li>Add the Security Onion stable repository:<br>
<pre><code>sudo apt-get -y install python-software-properties<br>
sudo add-apt-repository -y ppa:securityonion/stable<br>
sudo apt-get update<br>
</code></pre>
</li><li>Install the securityonion-all metapackage:<br>
<pre><code>sudo apt-get -y install securityonion-all<br>
</code></pre>
</li><li>Run the Setup wizard:<br>
<pre><code>sudo sosetup<br>
</code></pre>
</li><li>Follow the prompts.<br>
</li><li>Analyze alerts using the Sguil client, or open a browser to <a href='https://localhost'>https://localhost</a> where you can access Squert, Snorby, and ELSA.<br>
</li><li>Follow the <a href='Upgrade.md'>Upgrade</a> process.</li></ol>

Please review the <a href='PostInstallation.md'>Post Installation</a> page.