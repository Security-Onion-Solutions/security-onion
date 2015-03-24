# Introduction #

This page describes how to configure email for alerting and reporting.  Applications such as Snorby, Sguil, and OSSEC have their own mail configuration and don't rely on a mail server in the OS itself.  However, you may still want to install a mail server in the OS so that you can get daily emails from the sostat script and from Bro.

# How do I configure Snorby to send emails? #
Modify Snorby's mail\_config.rb file on the master server as needed for your mail server:
```
/opt/snorby/config/initializers/mail_config.rb

```
Then restart the Snorby delayed\_job process:
```
sudo pkill -f delayed_job

sudo su www-data -c "cd /opt/snorby; bundle exec rake snorby:update RAILS_ENV=production"

```
You can also modify /opt/snorby/config/snorby\_config.yml and change the "domain" setting in the Production section to be the FQDN or IP address of your Snorby server.  However, the resulting link in the email will still be incorrect since it will be http instead of https and it will be missing the proper port.

# How do I configure Sguil to send alerts via email?<br></h1>
Modify /etc/nsm/securityonion/sguild.email (on the master server) as needed and restart sguild:
```
sudo nsm_server_ps-restart
```
You can then verify the email configuration by looking at the top of sguild's log file:
```
head -20 /var/log/nsm/securityonion/sguild.log
```
For more information, please see:<br>
<a href='http://nsmwiki.org/Sguil_FAQ#Can_sguil_page_me_when_it_sees_a_particular_alert.3F'><a href='http://nsmwiki.org/Sguil_FAQ#Can_sguil_page_me_when_it_sees_a_particular_alert.3F'>http://nsmwiki.org/Sguil_FAQ#Can_sguil_page_me_when_it_sees_a_particular_alert.3F</a></a>

<h1>How do I configure OSSEC to send emails?</h1>
Modify /var/ossec/etc/ossec.conf as follows:<br>
<pre><code>  &lt;global&gt;<br>
    &lt;email_notification&gt;yes&lt;/email_notification&gt;<br>
    &lt;email_to&gt;YourUsername@YourDomain.com&lt;/email_to&gt;<br>
    &lt;smtp_server&gt;YourMailRelay.YourDomain.com&lt;/smtp_server&gt;<br>
    &lt;email_from&gt;ossec@YourDomain.com&lt;/email_from&gt;<br>
    &lt;email_maxperhour&gt;100&lt;/email_maxperhour&gt;<br>
  &lt;/global&gt;<br>
</code></pre>
Then restart OSSEC:<br>
<pre><code>sudo service ossec-hids-server restart<br>
</code></pre>

<h1>How do I configure the OS itself to send emails?</h1>
Install and configure your favorite mail server.  Depending on your needs, this could be something simple like nullmailer (recommended) or something more complex like exim4.<br>
<br>
Here are some nullmailer instructions provided by Michael Iverson:<br>
<pre><code>sudo apt-get install nullmailer<br>
# edit /etc/mailname to hold your "from" domain name. (If you were google, you'd use "gmail.com".)<br>
# edit /etc/nullmailer/adminaddr to contain the address you want mail to root to be routed to.<br>
# edit /etc/nullmailer/remotes to contain the mail server to forward email to. <br>
</code></pre>
Alternatively, here are some instructions for the more complex exim4:<br>
<pre><code>sudo apt-get -y install mailutils<br>
sudo dpkg-reconfigure exim4-config<br>
</code></pre>
Once you've configured your mail server and verified that it can send email properly, you might want to create a daily cronjob to execute /usr/bin/sostat and email you the output:<br>
<pre><code># /etc/cron.d/sostat<br>
#<br>
# crontab entry to run sostat and email its output<br>
<br>
SHELL=/bin/sh<br>
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin<br>
EMAIL=YourUsername@YourDomain.com<br>
<br>
01 12    * * *   root    HOSTNAME=`hostname`; /usr/bin/sostat 2&gt;&amp;1 | mail -s "$HOSTNAME stats" $EMAIL<br>
<br>
</code></pre>

If you don't already have the "mail" utility, you can try installing:<br>
<pre><code>sudo apt-get install mailutils<br>
</code></pre>

<h1>How do I configure Bro to send emails?</h1>
Edit /opt/bro/etc/broctl.cfg and set the following:<br>
<pre><code>MailTo = YourUsername@YourDomain.com<br>
sendmail = /usr/sbin/sendmail<br>
</code></pre>
Then update and restart Bro:<br>
<pre><code>sudo nsm_sensor_ps-restart --only-bro<br>
</code></pre>

You should then start receiving hourly connection summary emails.  If you don't want the connection summary emails, you can add the following to broctl.cfg and update and restart Bro as shown above:<br>
<pre><code>tracesummary=<br>
</code></pre>

You may want to receive emails for Bro notices.  To do that, add the following to /opt/bro/share/bro/site/local.bro and update/restart Bro as shown above:<br>
<pre><code>hook Notice::policy(n: Notice::Info)<br>
            {<br>
            add n$actions[Notice::ACTION_ALARM];<br>
            }<br>
</code></pre>
Also see:<br>
<a href='http://mailman.icsi.berkeley.edu/pipermail/bro/2013-December/007185.html'>http://mailman.icsi.berkeley.edu/pipermail/bro/2013-December/007185.html</a>

<h1>How can I get an email alert when my sensor stops seeing traffic?</h1>
If you configured OSSEC or Bro as shown above, they should automatically do this for you.  Another option can be found on the <a href='SensorStopsSeeingTraffic.md'>SensorStopsSeeingTraffic</a> page.