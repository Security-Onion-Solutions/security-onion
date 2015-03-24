# Why does Security Onion use UTC? #
When you run Security Onion Setup, it sets the timezone to UTC/GMT because that is the recommended/required setting for Sguil:<br>
<a href='http://osdir.com/ml/security.sguil.general/2008-09/msg00003.html'><a href='http://osdir.com/ml/security.sguil.general/2008-09/msg00003.html'>http://osdir.com/ml/security.sguil.general/2008-09/msg00003.html</a></a><br>
<a href='https://forums.snort.org/forums/linux/topics/barnyard-sguil-time-problem'><a href='https://forums.snort.org/forums/linux/topics/barnyard-sguil-time-problem'>https://forums.snort.org/forums/linux/topics/barnyard-sguil-time-problem</a></a><br>
<br>
Trying to use a non-UTC timezone can result in the following:<br>
- Time zones that have daylight saving time have a one-hour time warp twice a year.  This manifests itself in Sguil not being able to pull transcripts for events within that one-hour time period.  This is avoided by using UTC, since there is no daylight saving time.<br>
- Something similar can happen on a daily basis under certain conditions.  If there is a discrepancy between the OS timezone and the Sguil UTC settings, then Sguil will be unable to pull transcripts for events in a window of time around midnight coinciding with the timezone's offset from UTC.<br>
<br>
Additionally, UTC comes in quite handy when you have sensors in different time zones and/or are trying to correlate events with other systems or teams.<br>
<br>
Our three primary web interfaces (Snorby, Squert, and ELSA) all allow you to render event timestamps in your local timezone.  ELSA by default will render timestamps in the timezone of your local browser (more info below) and Squert and Snorby allow you to change your timezone.  If you set your local timezone in Snorby, make sure that you also set the same timezone in CapMe's timezone.php:<br>
<a href='http://blog.securityonion.net/2014/01/new-capme-package-allows-you-to.html'>http://blog.securityonion.net/2014/01/new-capme-package-allows-you-to.html</a>

<h1>Why are the timestamps in ELSA not in UTC?</h1>

By default, ELSA will display timestamps in the timezone of your local browser.  You can force ELSA to always display timestamps in UTC/GMT by configuring the use_utc setting in your ELSA Preferences panel.<br>
<br>
Known issue in ELSA 713 (old ELSA package):  If you access ELSA from a browser whose local timezone is not UTC <b>and</b> you haven't enabled the use_utc setting in your ELSA Preferences, then each search rolls the From time back the same number of hours as the UTC offset.  For example, suppose you login to ELSA and notice that the From defaults to 2013-05-05 18:01:50. When you then perform a search, the From changes to 2013-05-05 14:01:50.<br>
<br>
The workaround is to enable the use_utc setting in your ELSA<br>
Preferences (which is probably a good idea anyway to ensure that your<br>
timestamps in ELSA match your timestamps in Sguil/Squert/Snorby):<br>
<br>
1. Navigate to ELSA -> Preferences:<br>
<br>
<img src='http://security-onion.googlecode.com/svn/wiki/images/elsa/elsa_prefs.png' />

2. Select Actions -> Add New Preference:<br>
<br>
<img src='http://security-onion.googlecode.com/svn/wiki/images/elsa/elsa_prefs_add.png' />

3. Enter the following into the new Preference:<br>
<pre><code>Type = default_settings<br>
Name = use_utc<br>
Value = 1<br>
</code></pre>
<img src='http://security-onion.googlecode.com/svn/wiki/images/elsa/elsa_prefs_utc.png' />