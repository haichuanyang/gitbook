# hpss



```
8/1    Up       Forward      10G   Yes 0024.38a8.dd00 L2 BACKBONE: rtsw5.bldc, Eth 8/1 to sw19.bldc, xe-0/0/2 | IPG-BLDC-BLDC-10GE-01796 [MON]
8/2    Up       Forward      10G   Yes 0024.38a8.dd00 L2 BACKBONE: rtsw5.bldc, Eth 8/2 to sw19.bldc, xe-0/0/3 | IPG-BLDC-BLDC-10GE-01797 [MON]

//8/1 and 8/2 are the remaining 2 members up to the SDA cluster


```

juniper switch: show interfaces

salt high state steps are in: [twiki](https://rt-wiki.uits.iu.edu/Storage/RSHostHighstateApply)/RSApplyHighState after editing hosts.yaml and topology/physical.yaml

HP manuals

[https://rt-wiki.uits.iu.edu/Storage/RSServerChassisGuides](https://rt-wiki.uits.iu.edu/Storage/RSServerChassisGuides)

recover -i 1 -l, under recovery, stage fail in logs;  use -r&#x20;

recover -i 1 -r -v D057800

lsvol -i 1 -v D057800; multi-tape file 4 tapes for 2nd

QED code- shared memory segment with a table of uids over quota; 64-bit uids requires 64-bit addressing&#x20;

gtvi -i -f  /tmp/tapes to get import info

gtvi -h for help

gtvi -s -C -t -r -m -R D03777

\-M mount count

\-R reads count

\-m mount time

\-r read time
