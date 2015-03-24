# Changing the default ssh listening port #

By default secure shell (ssh) listens on tcp port 22. If you want to obfuscate it by changing the listening port from port 22 to port 31337, you can do so in the sshd\_config file.

note: you can use your favorite text editor (e.g., vi, gedit, nano, emacs) to edit the sshd\_conf file, but for the purpose of this example "vi" will be used.


```
sudo vi /etc/ssh/sshd_conf
```

<img src='http://i1165.photobucket.com/albums/q581/labrams132/ScreenShot2012-04-07at105424PM.png'>

<pre><code>Change the port to 31337<br>
</code></pre>

<img src='http://i1165.photobucket.com/albums/q581/labrams132/ScreenShot2012-04-07at104048PM.png'>

Perform a graceful restart on the ssh daemon so that it will now start listening on port 31337.<br>
<br>
<pre><code>sudo killall -HUP sshd <br>
</code></pre>

Verify that ssh is listening on the new port.<br>
<br>
<br>
<pre><code>netstat -nltp | grep sshd<br>
</code></pre>

<img src='http://i1165.photobucket.com/albums/q581/labrams132/ScreenShot2012-04-07at105303PM.png'>


<b>note:  make sure you allow the new ssh port in the ufw if you're using the firewall.</b>

<pre><code>sudo ufw allow 31337/tcp<br>
</code></pre>

<img src='http://i1165.photobucket.com/albums/q581/labrams132/ScreenShot2012-04-07at103846PM.png'>