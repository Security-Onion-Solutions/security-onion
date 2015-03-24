# Introduction #

Adding local rules in Security Onion is a rather straightforward process.  However, generating custom traffic to test the alert can sometimes be a challenge.  Here, we will show you how to add the local rule and then use the python library scapy to trigger the alert.

# Steps #

  1. Open the /etc/nsm/rules/local.rules file using your favorite text editor.
  1. Let's add a simple rule that will alert on the detection of a string in a tcp session.
```
alert tcp any any -> $HOME_NET 7789 (msg: "Vote for Security Onion Toolsmith Tool of 2011!"; reference: url,http://holisticinfosec.blogspot.com/2011/12/choose-2011-toolsmith-tool-of-year.html; content: "toolsmith"; flow:to_server; nocase; sid:9000547; rev:1)     
```
  1. Update sid-msg.map and restart snort/suricata and barnyard:
```
sudo rule-update
```
  1. If you built the rule correctly, then snort should be back up and running.
  1. Generate some traffic to trigger the alert.  To generate traffic we are going to use the python library scapy to craft packets with specific information to ensure we trigger the alert with the information we want.
```
sudo scapy
```
  1. Enter the following sample in a line at a time.  Any line beginning with "#" can be ignored as it is a comment.
```
# Craft the layer 2 information.
# The ip addresses can be random, but I would suggest sticking to RFC1918
ip = IP()
ip.dst = "192.168.200.4"
ip.src = "192.168.100.3"

# Craft the layer 3 information.
# Since we specified port 7789 in our snort rule, 
tcp = TCP()
tcp.dport = 7789
tcp.sport = 1234

# Set the playload
payload = "Toolsmith"

# Use the / operator to compose our packet and transfer it with the send() method.
send(ip/tcp/payload)
```
  1. Check sguil for the corresponding alert
> > <a href='http://security-onion.googlecode.com/svn/wiki/images/local-rules/sguil-window_verify-alert.png'>
<blockquote><img src='http://security-onion.googlecode.com/svn/wiki/images/local-rules/thumbs/thumb_sguil-window_verify-alert.png'></img>
</blockquote><blockquote></a></blockquote>


> You can see that we have an alert with the IP addresses we specified and the TCP ports we specified.
> If you right click on the **Alert ID** column you can select "Transcript" and verify the payload we sent.
> <a href='http://security-onion.googlecode.com/svn/wiki/images/local-rules/sguil-transcript_check-payload.png'>
<blockquote><img src='http://security-onion.googlecode.com/svn/wiki/images/local-rules/thumbs/thumb_sguil-transcript_check-payload.png'></img>
</blockquote><blockquote></a></blockquote>

  * You can learn more about snort and writing snort signatures from the [Snort Manual](http://manual.snort.org/node26.html)
  * You can learn more about scapy at [secdev.org](http://www.secdev.org/projects/scapy/) and [itgeekchronicles.co.uk](http://itgeekchronicles.co.uk/2012/05/31/scapy-guide-the-release/).

