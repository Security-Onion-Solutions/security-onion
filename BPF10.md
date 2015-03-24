# Configuration #

## Old Security Onion 10.04 ##

### Snort/Suricata/daemonlogger ###
As of Security Onion 20120329, we have support for a central bpf.conf that is passed to Snort, Suricata, and daemonlogger.

First, make sure you're running Security Onion 20120329 or higher:<br>
<a href='http://securityonion.blogspot.com/2012/03/security-onion-20120329-now-available.html'>http://securityonion.blogspot.com/2012/03/security-onion-20120329-now-available.html</a>

This update will create a bpf.conf file for each sensor interface on your system.  For example, if you have two sensor interfaces (eth0 and eth1), you'll now have two bpf.conf files:<br>
/etc/nsm/$HOSTNAME-eth0/bpf.conf<br>
/etc/nsm/$HOSTNAME-eth1/bpf.conf<br>
<br>
The NSM scripts now pass the "-F /etc/nsm/$HOSTNAME-$INTERFACE/bpf.conf" to Snort and Suricata and "-f /etc/nsm/$HOSTNAME-$INTERFACE/bpf.conf" to daemonlogger.  However, Suricata's afpacket mode currently doesn't support bpf.  I've created Suricata feature request #440 for this:<br>
<a href='https://redmine.openinfosecfoundation.org/issues/440'><a href='https://redmine.openinfosecfoundation.org/issues/440'>https://redmine.openinfosecfoundation.org/issues/440</a></a>

Once you've added your BPF to the proper bpf.conf file(s) for your sensor interface(s), restart the sensor processes using the following command:<br>
<pre><code>sudo nsm_sensor_ps-restart<br>
</code></pre>

<h3>Bro</h3>

Since there is one Bro config for ALL interfaces on the machine (as opposed to Snort/Suricata/Daemonlogger where we have one config for each interface), it cannot use the above configuration.  The recommended way to configure a BPF for Bro is to add the following to /usr/local/share/bro/site/local.bro:<br>
<pre><code>redef cmd_line_bpf_filter = "whatever your filter is";<br>
</code></pre>
Then, push the new configuration and restart Bro:<br>
<pre><code>sudo broctl install<br>
sudo broctl restart<br>
</code></pre>

<h1>BPF Examples</h1>

From Phillip Wang:<br>
<br>
Just to contribute, and for others to reference, here are some examples of what I've got working<br>
<br>
<pre><code>#Nothing from src host to dst port<br>
!(src host xxx.xxx.xxx.xxx &amp;&amp; dst port 161) &amp;&amp;<br>
<br>
#Nothing from src host to dst host and dst port<br>
!(src host xxx.xxx.xxx.xxx &amp;&amp; dst host xxx.xxx.xxx.xxx &amp;&amp; dst port 80) &amp;&amp;<br>
<br>
#Nothing to or from:<br>
!(host xxx.xxx.xxx.xxx) &amp;&amp;<br>
<br>
#Last entry has no final &amp;&amp;<br>
!(host xxx.xxx.xxx.xxx)<br>
</code></pre>

From Seth Hall regarding VLAN tags:<br>
<br>
<pre><code>(not (host 192.168.53.254 or host 192.168.53.60 or host 192.168.53.69 or host 192.168.53.234)) or (vlan and (not (host 192.168.53.254 or host 192.168.53.60 or host 192.168.53.69 or host 192.168.53.234)))<br>
</code></pre>

This amazingly works if you are only using it to restrict the traffic passing through the filter.  The basic template is…<br>
<br>
<pre><code>&lt;your filter&gt; and (vlan and &lt;your filter&gt;)<br>
</code></pre>

Once the "vlan" tag is included in the filter, all subsequent expressions to the right are shifted by four bytes so you need to duplicate the filter on both sides of the vlan keyword.  There are edge cases where this will no longer work and probably edge cases where a few undesired packets will make it though, but it should work in the example case that you've given.<br>
<br>
Also, I'm assuming that any tools you are running will support vlan tags and no tags simultaneously.  Bro 2.0 should work fine at least.<br>
<br>
<h1>Troubleshooting BPF using tcpdump</h1>

<a href='http://taosecurity.blogspot.com/2004/09/understanding-tcpdumps-d-option-have.html'>http://taosecurity.blogspot.com/2004/09/understanding-tcpdumps-d-option-have.html</a>

<a href='http://taosecurity.blogspot.com/2004/12/understanding-tcpdumps-d-option-part-2.html'>http://taosecurity.blogspot.com/2004/12/understanding-tcpdumps-d-option-part-2.html</a>

<a href='http://taosecurity.blogspot.com/2008/12/bpf-for-ip-or-vlan-traffic.html'>http://taosecurity.blogspot.com/2008/12/bpf-for-ip-or-vlan-traffic.html</a>