# 32-bit vs 64-bit #
All of our packages are available in both 32-bit and 64-bit versions, but 64-bit is highly recommended due to the increased performance.

# RAM #

RAM usage is highly dependent on several variables:
  * the services that you enable
  * the **kinds** of traffic you're monitoring
  * the **actual amount of traffic** you're monitoring (example: you may be monitoring a 1Gbps link but it's only using 200Mbps most of the time)
  * the amount of packet loss that is "acceptable" to your organization

The following RAM estimates are a rough guideline and assume that you're going to be running Snort/Suricata, Bro, and netsniff-ng (full packet capture) and want to minimize/eliminate packet loss.  Your mileage may vary!

If you just want to quickly evaluate Security Onion in a VM, the bare minimum amount of RAM needed is 3GB.  More is obviously better!

If you're deploying Security Onion in production on a small network (50Mbps or less), you should plan on 8GB RAM or more.  Again, more is obviously better!

If you're deploying Security Onion in production to a medium network (50Mbps - 500Mbps), you should plan on 16GB - 128GB RAM or more.

If you're deploying Security Onion in production to a large network (500Mbps - 1000Mbps), you should plan on 128GB - 256GB RAM or more.

If you're buying a new server, go ahead and max out the RAM (it's cheap!).  As always, more is obviously better!

# Storage #
Sensors that have full packet capture (and/or ELSA) enabled need LOTS of storage. For example, suppose you are monitoring a 50 Mb/s link, here are some quick calculations: 50Mb/s = 6.25 MB/s = 375 MB/minute = 22,500 MB/hour = 540,000 MB/day. So you're going to need about 540GB for one day's worth of pcaps (multiply this by the number of days you want to keep on disk for investigative/forensic purposes). Note that this is just pcaps (other logs will take up additional storage), so you may want to round up to the next terabyte to ensure sufficient storage. The more disk space you have, the more log retention you'll have for doing investigations after the fact. Disk is cheap, get all you can!

# NIC #
You'll need at least two network interfaces: one for management (preferably connected to a dedicated management network) and then one or more for sniffing (connected to tap or span).  Make sure you get a good quality network card.  I've had good experiences with Intel.

# Packets #
You need some way of getting packets into your sensor interface(s).  If you're just evaluating Security Onion, you can replay [pcaps](Pcaps.md).  For a production deployment, you'll need a tap or SPAN/monitor port.  Here are some inexpensive tap/span solutions:<br>

Shear simplicity and portability (USB-powered):<br>
<a href='http://www.dual-comm.com/port-mirroring-LAN_switch.htm'>http://www.dual-comm.com/port-mirroring-LAN_switch.htm</a><br>

Dirt cheap and versatile:<br>
<a href='http://www.roc-noc.com/mikrotik/routerboard/RB260GS.html'>http://www.roc-noc.com/mikrotik/routerboard/RB260GS.html</a><br>

Netgear GS105E (requires Windows app for config):<br>
<a href='http://www.netgear.com/business/products/switches/prosafe-plus-switches/gs105e.aspx'>http://www.netgear.com/business/products/switches/prosafe-plus-switches/gs105e.aspx</a><br>

More exhaustive list of enterprise switches with port mirroring:<br>
<a href='http://www.miarec.com/knowledge/switches-port-mirroring'>http://www.miarec.com/knowledge/switches-port-mirroring</a><br>

For more enterprise tap solutions, take a look at Net Optics and Arista.