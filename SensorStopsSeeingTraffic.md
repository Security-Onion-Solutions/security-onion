Just like in everything, there's always more than one way to do it!<br>
<br>
Here are a few options:<br>
<br>
<h1>OSSEC</h1>
OSSEC checks your sniffing interfaces every 10 minutes.  If no packets have been received within that 10 minute window, then OSSEC will generate an alert.  This alert can be found in Sguil, Squert, and ELSA.  If you'd like OSSEC to email you, then configure it for email as shown here:<br>
<a href='https://code.google.com/p/security-onion/wiki/Email#How_do_I_configure_OSSEC_to_send_emails?'>https://code.google.com/p/security-onion/wiki/Email#How_do_I_configure_OSSEC_to_send_emails?</a>

<h1>Bro</h1>
Bro will automatically email you when it stops seeing traffic on an interface.  All you have to do is configure Bro per the <a href='Email.md'>Email</a> page:<br>
<a href='https://code.google.com/p/security-onion/wiki/Email#How_do_I_configure_Bro_to_send_emails?'>https://code.google.com/p/security-onion/wiki/Email#How_do_I_configure_Bro_to_send_emails?</a>

<h1>Script to check for lack of IDS alerts</h1>
Here's another option contributed by Jerry Shenk:<br>
<pre><code>#!/bin/sh<br>
#script to monitor Security Onion activity for the past hour to alert on inactivity<br>
#Inactivity could be due to a connection having been removed or some process failing<br>
MAILTO=idsadmin@copmany.com<br>
DATE=`date`<br>
SUBJECT="`hostname` Security Onion inactivity alert `date`"<br>
LIMIT=5<br>
REPORT=/root/so-lasthour.txt<br>
<br>
echo $SUBJECT &gt; /root/edgerouter.log<br>
<br>
if test ` mysql -N -B --user root --database securityonion_db -e<br>
"SELECT COUNT(signature)as cnt, signature FROM event WHERE status&lt;&gt;1<br>
and timestamp&gt;=date_sub(now(), interval 3 hour) GROUP BY signature<br>
ORDER BY cnt DESC LIMIT 20;" | grep -c .` -le $LIMIT<br>
  then<br>
     echo "Too few events"<br>
<br>
     echo "non-URL signatures" &gt; $REPORT<br>
     mysql -N -B --user root --database securityonion_db -e "SELECT<br>
COUNT(signature)as cnt, signature FROM event WHERE status&lt;&gt;1 and<br>
timestamp&gt;=date_sub(now(), interval 3 hour) GROUP BY signature ORDER<br>
BY cnt DESC LIMIT 20;" &gt;&gt; $REPORT<br>
     echo "" &gt;&gt; $REPORT<br>
     echo "URL signatures" &gt;&gt; $REPORT<br>
     mysql -N -B --user root --database securityonion_db -e "SELECT<br>
COUNT(signature)as cnt, signature FROM event WHERE status=1 and<br>
timestamp&gt;=date_sub(now(), interval 3 hour) GROUP BY signature ORDER<br>
BY cnt DESC LIMIT 20;" &gt;&gt; $REPORT<br>
     cat $REPORT | mail -s "$SUBJECT" $MAILTO<br>
<br>
  else<br>
     echo "Acceptible number of events"<br>
  fi<br>
</code></pre>