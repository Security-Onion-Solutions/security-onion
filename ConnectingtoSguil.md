# Connecting to Sguild #

## Introduction ##

This article will show how to connect to the Sguil server to view security alerts in real-time.

## Connect to Sguild Locally ##

Double-click the Sguil icon on the desktop of your SecurityOnion server.

![http://jonschipp.com/seconion/sguil-icon.png](http://jonschipp.com/seconion/sguil-icon.png)

Set the Sguil Host to localhost, enter your credentials, and then click OK.

![http://jonschipp.com/seconion/sguil-local.png](http://jonschipp.com/seconion/sguil-local.png)

After, choose which sensors you would like to monitor for this sguil session and then click Start Sguil.

## Connect Remotely via SSH w/ X11 Forwarding ##

This method requires SSH and an X11 server installed on the machine from which you will be
connecting from.

If you're using OSX install the XQuartz package, Windows try ciXwin, Linux and BSD family use Xorg.

Connect to the SecurityOnion server via SSH while passing the X11 forwarding option ( -X ).

```
ssh -X user@nsm
```

Once logged in as the normal user open the sguil client application.
The display will be sent to your machine using the X11 protocol over SSH.

```
sudo sguil.tk
```

![http://jonschipp.com/seconion/sguil.png](http://jonschipp.com/seconion/sguil.png)

Since we're only forwarding the application window, we're connected locally i.e. as if we were
sitting at the server's console. Because of this we can use localhost as the Sguild Host.

Once logged in we will be able to select which sensors we would like to monitor.

![http://jonschipp.com/seconion/sguil-select.png](http://jonschipp.com/seconion/sguil-select.png)

Finally, select Start Sguil. Now you can view the alerts in real-time, perform advanced SQL queries,
and pivot into a number of applications like Wireshark, ELSA, and Network Minor.

## Directly Connecting to Sguild Remotely ##

To directly connect to a Sguild server one must possess a working Sguil client.
Sguil may not be easy or available for install on certain operating systems. Because of this
I recommend installing SecurityOnion in a virtual machine on your workstation and use that
to connect to sguild on your production SecurityOnion instance.

For this section I will be using VMware Fusion on OSX.

Install SecurityOnion in a Virtual Machine and configure the network adapter to use NAT mode (easiest)
by going to your VM's settings. This will work if you have per-IP/host ACL's too since the same IP address will be used.

![http://jonschipp.com/seconion/sguil-network-adapter.png](http://jonschipp.com/seconion/sguil-network-adapter.png)

![http://jonschipp.com/seconion/sguil-enable-network-adapter.png](http://jonschipp.com/seconion/sguil-enable-network-adapter.png)

Now double-click the Sguil desktop icon to launch the Sguil client.

![http://jonschipp.com/seconion/sguil-icon.png](http://jonschipp.com/seconion/sguil-icon.png)

Fill in the IP address or DNS name
of the SecurityOnion server and apply your credentials.

![http://jonschipp.com/seconion/sguil-vm.png](http://jonschipp.com/seconion/sguil-vm.png)

Then select the sensors to monitor and finally click Start Sguil.