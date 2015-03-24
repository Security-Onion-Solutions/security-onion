Please note that this is all subject to change!
  * January 2015
    * [Issue 655](https://code.google.com/p/security-onion/issues/detail?id=655): Suricata 2.0.5
    * [Issue 658](https://code.google.com/p/security-onion/issues/detail?id=658): NSM: fix umask on Snort unified2 output
    * [Issue 548](https://code.google.com/p/security-onion/issues/detail?id=548): NSM: run barnyard2 as non-root user
    * [Issue 649](https://code.google.com/p/security-onion/issues/detail?id=649): nsm\_all\_del\_quick: check for /etc/nsm/servertab and /etc/nsm/sensortab before trying to read
    * [Issue 598](https://code.google.com/p/security-onion/issues/detail?id=598): so-snorby-wipe
    * [Issue 610](https://code.google.com/p/security-onion/issues/detail?id=610): NSM: ossec\_agent alert level should be configurable
    * [Issue 660](https://code.google.com/p/security-onion/issues/detail?id=660): Setup: add OSSEC\_AGENT\_LEVEL to /etc/nsm/securityonion.conf
    * [Issue 656](https://code.google.com/p/security-onion/issues/detail?id=656): ELSA: update parser for bro\_conn to parse country code
    * [Issue 659](https://code.google.com/p/security-onion/issues/detail?id=659): securityonion-web-page: add ELSA query for bro\_conn groupby:resp\_country\_code
    * [Issue 667](https://code.google.com/p/security-onion/issues/detail?id=667): New packages for shellshock and malware-traffic-analysis samples
    * [Issue 673](https://code.google.com/p/security-onion/issues/detail?id=673): Suricata 2.0.6
    * [Issue 642](https://code.google.com/p/security-onion/issues/detail?id=642): Update Salt packages/scripts to 2014.7.0
    * [Issue 619](https://code.google.com/p/security-onion/issues/detail?id=619): Onionsalt: backup /opt/onionsalt/pillar/top.sls
    * [Issue 661](https://code.google.com/p/security-onion/issues/detail?id=661): Onionsalt: replicate /usr/local/lib/snort\_dynamicrules/
    * [Issue 672](https://code.google.com/p/security-onion/issues/detail?id=672): sguil-db-purge: check for UNCAT\_MAX
    * [Issue 663](https://code.google.com/p/security-onion/issues/detail?id=663): sosetup: sosetup.conf SGUIL\_CLIENT\_PASSWORD\_1 should say Sguil/Squert/ELSA/Snorby
    * [Issue 664](https://code.google.com/p/security-onion/issues/detail?id=664): sosetup: run Bro as non-root user
    * [Issue 666](https://code.google.com/p/security-onion/issues/detail?id=666): sostat: run Bro as non-root user
    * [Issue 665](https://code.google.com/p/security-onion/issues/detail?id=665): NSM: run Bro as non-root user
    * [Issue 676](https://code.google.com/p/security-onion/issues/detail?id=676): NSM: run Sguil as non-root user
    * [Issue 671](https://code.google.com/p/security-onion/issues/detail?id=671): NSM: /etc/cron.d/sensor-clean needs 2>&1
  * February 2015
    * [Issue 668](https://code.google.com/p/security-onion/issues/detail?id=668): ELSA: pdbtool errors
    * [Issue 669](https://code.google.com/p/security-onion/issues/detail?id=669): ELSA: update parsers for Bro DNS and BIND
    * [Issue 670](https://code.google.com/p/security-onion/issues/detail?id=670): securityonion-web-page: add queries for updated bro\_dns parser
    * [Issue 685](https://code.google.com/p/security-onion/issues/detail?id=685): securityonion-web-page: update links
    * [Issue 684](https://code.google.com/p/security-onion/issues/detail?id=684): NSM: nsm\_server\_ps-start needs to create /var/log/sguild/ if it doesn't already exist
    * [Issue 686](https://code.google.com/p/security-onion/issues/detail?id=686): NSM: nsm\_server\_ps-start needs to set permissions on /var/log/nsm/so-elsa/ properly
    * [Issue 687](https://code.google.com/p/security-onion/issues/detail?id=687): NSM: nsm\_sensor\_ps-start should set permissions on /var/log/nsm/ properly
    * [Issue 689](https://code.google.com/p/security-onion/issues/detail?id=689): NSM: add USE\_DNS option to ossec\_agent.conf
    * [Issue 688](https://code.google.com/p/security-onion/issues/detail?id=688): ossec\_agent: add option to disable DNS lookups
    * [Issue 680](https://code.google.com/p/security-onion/issues/detail?id=680): Bro 2.3.2
    * [Issue 683](https://code.google.com/p/security-onion/issues/detail?id=683): securityonion-et-rules: update for new ISO
    * [Issue 632](https://code.google.com/p/security-onion/issues/detail?id=632): ISO: add bridge-utils
    * [Issue 601](https://code.google.com/p/security-onion/issues/detail?id=601): ISO: add foremost
    * [Issue 614](https://code.google.com/p/security-onion/issues/detail?id=614): ISO: add securityonion-samples-shellshock
    * [Issue 662](https://code.google.com/p/security-onion/issues/detail?id=662): ISO: add securityonion-samples-mta
    * [Issue 675](https://code.google.com/p/security-onion/issues/detail?id=675): ISO: add xfsprogs
    * [Issue 602](https://code.google.com/p/security-onion/issues/detail?id=602): 12.04.5.1 ISO image
  * March 2015
    * [Issue 695](https://code.google.com/p/security-onion/issues/detail?id=695): Suricata 2.0.7
    * [Issue 696](https://code.google.com/p/security-onion/issues/detail?id=696): ELSA custom menu
    * [Issue 691](https://code.google.com/p/security-onion/issues/detail?id=691): NSM: chown -R $BRO\_USER:$BRO\_GROUP /nsm/bro >/dev/null 2>&1
    * [Issue 698](https://code.google.com/p/security-onion/issues/detail?id=698): NSM: nsm\_server\_del line 170 echo\_msg 0 "Deleting server: $SERVER\_NAME"
    * [Issue 699](https://code.google.com/p/security-onion/issues/detail?id=699): NSM: Bro node.cfg host=localhost
    * [Issue 700](https://code.google.com/p/security-onion/issues/detail?id=700): Setup: Bro node.cfg host=localhost
    * [Issue 702](https://code.google.com/p/security-onion/issues/detail?id=702): Snort 2.9.7.2
  * April 2015
    * [Issue 703](https://code.google.com/p/security-onion/issues/detail?id=703): Move from Google Code to Github
    * [Issue 705](https://code.google.com/p/security-onion/issues/detail?id=705): ossec\_agent: improvements from Brian Kellogg
    * [Issue 681](https://code.google.com/p/security-onion/issues/detail?id=681): rule-update: wipe snort\_dynamicrules directory on sensor
    * [Issue 677](https://code.google.com/p/security-onion/issues/detail?id=677): rule-update: create /usr/local/lib/snort\_dynamicrules/ if it doesn't already exist
    * [Issue 678](https://code.google.com/p/security-onion/issues/detail?id=678): rule-update: /etc/cron.d/rule-update should have 2>&1
    * [Issue 697](https://code.google.com/p/security-onion/issues/detail?id=697): rule-update: log snorby reference table update to barnyard2-snorby.log
    * [Issue 682](https://code.google.com/p/security-onion/issues/detail?id=682): rule-update: update Snorby signature\_lookup
    * [Issue 679](https://code.google.com/p/security-onion/issues/detail?id=679): rule-update: /etc/cron.d/rule-update should run pulledpork as unprivileged user
    * [Issue 690](https://code.google.com/p/security-onion/issues/detail?id=690): http\_agent: ---disable-inotify
    * [Issue 615](https://code.google.com/p/security-onion/issues/detail?id=615): NSM: add "exit $RET" where necessary
    * [Issue 392](https://code.google.com/p/security-onion/issues/detail?id=392): Patch for lib-nsm-common-utils from Mark Seiden
    * [Issue 588](https://code.google.com/p/security-onion/issues/detail?id=588): NSM: purge old OSSEC logs
    * [Issue 561](https://code.google.com/p/security-onion/issues/detail?id=561): nsm\_server\_backup-config should check FORCE\_YES
    * [Issue 523](https://code.google.com/p/security-onion/issues/detail?id=523): sensor-clean: add option to skip removal of bro or argus logs
    * [Issue 534](https://code.google.com/p/security-onion/issues/detail?id=534): NSM: Patches for adding PCAP snap length for Netsniff-NG
    * [Issue 645](https://code.google.com/p/security-onion/issues/detail?id=645): NSM scripts don't check if a sensor is disabled before performing operations when a --sensor-name= is specified
    * [Issue 652](https://code.google.com/p/security-onion/issues/detail?id=652): NSM: barnyard sending blank interface to ELSA
    * [Issue 653](https://code.google.com/p/security-onion/issues/detail?id=653): NSM: nsm\_sensor\_ps-stop should kill the processes tailing the snort.stats files
    * [Issue 654](https://code.google.com/p/security-onion/issues/detail?id=654): NSM: set SNORT\_PERF\_STATS in snort\_agent.conf to 0 when ENGINE=suricata
    * [Issue 643](https://code.google.com/p/security-onion/issues/detail?id=643): Rotate logs in /var/log/nsm/
    * [Issue 571](https://code.google.com/p/security-onion/issues/detail?id=571): securityonion-web-page: add Security Onion cheat sheet PDF
    * [Issue 644](https://code.google.com/p/security-onion/issues/detail?id=644): sostat-quick: check server/sensor
    * [Issue 692](https://code.google.com/p/security-onion/issues/detail?id=692): sostat: list number of ELSA buffers in queue and warn if higher than 10
    * [Issue 701](https://code.google.com/p/security-onion/issues/detail?id=701): sostat: include number of CPU cores
    * [Issue 591](https://code.google.com/p/security-onion/issues/detail?id=591): Bro Intel Whitelist
    * [Issue 418](https://code.google.com/p/security-onion/issues/detail?id=418): netsniff-ng 0.58 or higher
    * [Issue 492](https://code.google.com/p/security-onion/issues/detail?id=492): CapMe needs to handle UDP better
    * [Issue 493](https://code.google.com/p/security-onion/issues/detail?id=493): CapMe: send credentials interactively to avoid exposing on command line
    * [Issue 608](https://code.google.com/p/security-onion/issues/detail?id=608): Update bash scripts to use /bin/sh
    * [Issue 547](https://code.google.com/p/security-onion/issues/detail?id=547): sosetup: if enabling salt on a sensor, check top.sls to make sure it doesn't already exist
    * [Issue 304](https://code.google.com/p/security-onion/issues/detail?id=304): sosetup: support unique interface names
    * [Issue 605](https://code.google.com/p/security-onion/issues/detail?id=605): sosetup: replace tmp with mktemp
    * [Issue 596](https://code.google.com/p/security-onion/issues/detail?id=596): sosetup: sensor should stop/disable Apache and Snorby worker
    * [Issue 693](https://code.google.com/p/security-onion/issues/detail?id=693): sosetup: improve input validation for email address
    * [Issue 592](https://code.google.com/p/security-onion/issues/detail?id=592): sosetup: add -y option
    * [Issue 593](https://code.google.com/p/security-onion/issues/detail?id=593): sosetup: checking for Internet access takes a while if DNS doesn't immediately fail
    * [Issue 480](https://code.google.com/p/security-onion/issues/detail?id=480): sosetup: running on sensor should automatically create autossh account on server
    * [Issue 500](https://code.google.com/p/security-onion/issues/detail?id=500): sosetup: restart starman (may not be necessary)
    * [Issue 504](https://code.google.com/p/security-onion/issues/detail?id=504): sosetup: avoid running securityonion\_elsa\_register.rb twice
    * [Issue 532](https://code.google.com/p/security-onion/issues/detail?id=532): sosetup: Limit what autossh keys can do
    * [Issue 559](https://code.google.com/p/security-onion/issues/detail?id=559): sosetup: support for NIC bonding configuration
  * May 2015
    * [Issue 603](https://code.google.com/p/security-onion/issues/detail?id=603): securityonion-bro-scripts: drwatson
    * [Issue 604](https://code.google.com/p/security-onion/issues/detail?id=604): ELSA: parsers for Bro drwatson logs
    * [Issue 558](https://code.google.com/p/security-onion/issues/detail?id=558): Add VirusTotal uploader
    * [Issue 369](https://code.google.com/p/security-onion/issues/detail?id=369): Arpwatch
    * [Issue 657](https://code.google.com/p/security-onion/issues/detail?id=657): ELSA: email check
    * [Issue 651](https://code.google.com/p/security-onion/issues/detail?id=651): ELSA: starman restart doesn't work properly
    * [Issue 464](https://code.google.com/p/security-onion/issues/detail?id=464): Consider changing default query\_timeout in /etc/elsa\_web.conf
    * [Issue 331](https://code.google.com/p/security-onion/issues/detail?id=331): securityonion-elsa: update dependencies
    * [Issue 336](https://code.google.com/p/security-onion/issues/detail?id=336): When configuring ELSA log node, change MySQL port using /etc/mysql/conf.d/
    * [Issue 385](https://code.google.com/p/security-onion/issues/detail?id=385): Additional log parsers (non-Bro) for ELSA
    * [Issue 447](https://code.google.com/p/security-onion/issues/detail?id=447): ELSA syslog-ng.conf rewrite r\_pipes
    * [Issue 512](https://code.google.com/p/security-onion/issues/detail?id=512): ELSA syslog-ng.conf filter f\_bro\_headers
    * [Issue 467](https://code.google.com/p/security-onion/issues/detail?id=467): ELSA dashboard for Snort performance
    * [Issue 594](https://code.google.com/p/security-onion/issues/detail?id=594): securityonion-sudoers: 10\_securityonion