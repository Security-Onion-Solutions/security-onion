# Introduction #

The tcl/tk packages are "on hold".  apt-get will report that tcl8.5 has been held back.  Update Manager will show tcl8.5 as grayed out and unavailable.  This is intentional and you should not try to manually install tcl8.5.

# Details #

Sguil is not compatible with tcl threading.  In 20110607, I compiled and deployed tcl8.5 WITHOUT threading and put the normal tcl8.5 (WITH theading) on hold to prevent it from being installed.  I did this using the command:<br>
wajig hold tcl8.5 tk8.5 tcl8.4 itcl3 itk3 iwidgets4<br>
<br>
If you force tcl8.5 to install, it will install the threaded version and Sguil will break.  Please do not change the tcl/tk packages!<br>
<br>
<h1>Fix</h1>
If you accidentally installed the threaded version of tcl8.5, you should be able to get back to a working configuration using the following steps:<br>
<pre><code># get the correct version for SO<br>
wget http://sourceforge.net/projects/security-onion/files/20110607/tcl8.5_8.5.8-2_i386.deb<br>
<br>
# remove the wrong version<br>
sudo dpkg -r tcl tcl8.3-dev tk8.4 itcl3 itk3 iwidgets4<br>
<br>
# install the correct version<br>
sudo dpkg -i tcl8.5_8.5.8-2_i386.deb<br>
sudo apt-get -y install tk8.5 itcl3 itk3 iwidgets4 expect<br>
<br>
# set the default tcl version<br>
sudo update-alternatives --set tclsh /usr/bin/tclsh8.5 <br>
<br>
# put the packages back on hold<br>
sudo wajig hold tcl8.5 tk8.5 tcl8.4 itcl3 itk3 iwidgets4<br>
</code></pre>

<h1>See Also</h1>
Also see the page on <a href='FreeNX.md'>FreeNX</a> and in particular this:<br>
<br>
"Now that the FreeNX Server is up and running if you were to attempt to launch Sguil from the desktop link you'll notice that nothing happens. This is due to a symlink change made during the installation that affects the execution of the 'wish' command. Execution of 'wish' launches /usr/bin/wish which is a symlink to /etc/alternatives/wish. Prior to the FreeNX Server installation the symlink /etc/alternatives/wish pointed to /usr/bin/wish8.5 and now points to a newly created symlink /usr/bin/wish-default which points to /usr/bin/wish8.4. You need to change it back so that exection of 'wish', by Sguil, will launch tk8.5 and not tk8.4."<br>
<pre><code>sudo ln -sf /usr/bin/wish8.5 /etc/alternatives/wish<br>
</code></pre>