# /nsm Directory Structure #

## /nsm ##
Backup, Bro, sensor (if configured as sensor), and server (if configured as server) data.

## /nsm/bro ##
Bro IDS logs.

## /nsm/elsa ##
ELSA data.

## /nsm/sensor\_data ##
Sensor data including argus, IDS alerts, and full pcap organized by sensor name ($HOSTNAME-$INTERFACE).

## /nsm/server\_data ##
Server data including IDS rulesets.