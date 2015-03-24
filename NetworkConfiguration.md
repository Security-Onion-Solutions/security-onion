# Disabling the graphical Network Manager and configuring networking from the command line #

## If you're running Security Onion 12.04, all of this configuration will happen automatically if you choose "Yes, configure /etc/network/interfaces" in the Setup wizard. ##

NOTE! You may lose network connectivity during this process! Have a backup plan if attempting over SSH!<br>
<br>
Stop Network Manager:<br>
<pre><code>sudo /etc/init.d/network-manager stop<br>
</code></pre>
Prevent Network Manager from starting at next boot:<br>
<pre><code>sudo mv /etc/init/network-manager.conf /etc/init/network-manager.conf.DISABLED<br>
</code></pre>
Next, configure your network interfaces in /etc/network/interfaces.<br>
<br>
<h3>Management interface</h3>
You'll want a management interface (preferably connected to a dedicated management network) using either DHCP OR <b>preferably</b> static IP.  If your management interface uses DHCP and you have Bro in cluster mode, it will complain whenever your DHCP address changes and you'll have to update your IP in Bro's node.cfg file.  Static IP is highly recommended to prevent this issue.<br>
<br>
<h3>Sniffing interface(s)</h3>
You'll want one or more interfaces dedicated to sniffing (no IP address).  NIC offloading functions such as tso, gso, and gro should be disabled to ensure that Snort/Suricata get an accurate view of the traffic (see <a href='http://blog.securityonion.net/2011/10/when-is-full-packet-capture-not-full.html'><a href='http://blog.securityonion.net/2011/10/when-is-full-packet-capture-not-full.html'>http://blog.securityonion.net/2011/10/when-is-full-packet-capture-not-full.html</a></a>).<br>
<br>
<h3>Sample /etc/network/interfaces</h3>
<pre><code>auto lo<br>
iface lo inet loopback<br>
<br>
# Management interface using DHCP (not recommended due to Bro issue described above)<br>
auto eth0<br>
iface eth0 inet dhcp<br>
<br>
# OR <br>
<br>
# Management interface using STATIC IP (instead of DHCP)<br>
auto eth0<br>
iface eth0 inet static<br>
  address 192.168.1.14<br>
  gateway 192.168.1.1<br>
  netmask 255.255.255.0<br>
  network 192.168.1.0<br>
  broadcast 192.168.1.255<br>
  # If running Security Onion 12.04, you'll need to configure DNS here<br>
  dns-nameservers 192.168.1.1 192.168.1.2<br>
<br>
# AND one or more of the following<br>
<br>
# Connected to TAP or SPAN port for traffic monitoring<br>
auto eth1<br>
iface eth1 inet manual<br>
  up ifconfig $IFACE -arp up<br>
  up ip link set $IFACE promisc on<br>
  down ip link set $IFACE promisc off<br>
  down ifconfig $IFACE down<br>
  post-up for i in rx tx sg tso ufo gso gro lro; do ethtool -K $IFACE $i off; done<br>
  # If running Security Onion 12.04, you should also disable IPv6 as follows:<br>
  post-up echo 1 &gt; /proc/sys/net/ipv6/conf/$IFACE/disable_ipv6<br>
</code></pre>

You may also want to set the RX buffer size in the post-up command like this:<br>
<pre><code>post-up ethtool -G $IFACE rx 4096; for i in rx tx sg tso ufo gso gro lro; do ethtool -K $IFACE $i off; done<br>
</code></pre>
Note that 4096 is just an example and your NIC may have a different maximum rx size.  To determine the maximum rx setting for your NIC:<br>
<pre><code>ethtool -g ethX<br>
</code></pre>

If necessary, configure DNS in /etc/resolv.conf:<br>
<a href='http://en.wikipedia.org/wiki/Resolv.conf'>http://en.wikipedia.org/wiki/Resolv.conf</a><br>
<a href='http://www.cyberciti.biz/tips/howto-ubuntu-linux-convert-dhcp-network-configuration-to-static-ip-configuration.html'>http://www.cyberciti.biz/tips/howto-ubuntu-linux-convert-dhcp-network-configuration-to-static-ip-configuration.html</a><br>
<a href='http://manpages.ubuntu.com/manpages/lucid/man5/resolver.5.html'>http://manpages.ubuntu.com/manpages/lucid/man5/resolver.5.html</a><br>
<br>
Restart networking:<br>
<pre><code>sudo /etc/init.d/networking restart<br>
</code></pre>

If you already had sensors running on these interfaces, you should restart them:<br>
<pre><code>sudo nsm_sensor_ps-restart<br>
</code></pre>

For more information on network configuration in Ubuntu, please see:<br>
<a href='https://help.ubuntu.com/community/NetworkConfigurationCommandLine/Automatic'><a href='https://help.ubuntu.com/community/NetworkConfigurationCommandLine/Automatic'>https://help.ubuntu.com/community/NetworkConfigurationCommandLine/Automatic</a></a>