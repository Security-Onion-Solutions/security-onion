# Setup #

Setup will automatically ask you how many PF\_RING instances you'd like for Snort/Suricata and Bro (assuming
you choose Advanced Setup and you have multiple cores) and will tell you
how to adjust after the fact.

# Tuning #

If you want to change the number of PF\_RING instances after running Setup, you can do the following.

## Snort/Suricata ##

  * Stop sensor processes:
```
sudo nsm_sensor_ps-stop
```
  * Edit `/etc/nsm/$HOSTNAME-$INTERFACE/sensor.conf` and change the `IDS_LB_PROCS` variable to desired number of cores.
  * Start sensor processes:
```
sudo nsm_sensor_ps-start
```

If running Snort, the script automatically spawns $IDS\_LB\_PROCS instances
of Snort (using PF\_RING), barnyard2, and snort\_agent.<br>
If running Suricata, the script automatically copies $IDS_LB_PROCS into<br>
suricata.yaml and then Suricata spins up the PF_RING instances itself.<br>
<br>
<h2>Bro</h2>
For Bro, you would do the following:<br>
<br>
<ul><li>Stop bro:<br>
<pre><code>sudo nsm_sensor_ps-stop --only-bro<br>
</code></pre>
</li><li>Edit <code>/opt/bro/etc/node.cfg</code> and change the <code>lb_procs</code> variable to the desired number of cores.<br>
</li><li>Start bro:<br>
<pre><code>sudo nsm_sensor_ps-start --only-bro<br>
</code></pre></li></ul>

<h1>Updating</h1>
Please see the <a href='Upgrade.md'>Update</a> page for notes on updating the PF_RING kernel module.