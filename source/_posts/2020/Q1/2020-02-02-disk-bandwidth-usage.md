---
title: Disk bandwidth utilization
description: Disk bandwidth utilization
date: 2020-02-02 18:50:18
tags:
  - Linux
categories: [Operating system, Linux]
permalink: disk-bandwidth-usage
---

# Disk bandwidth utilization

We can use the following tools to check the disk bandwidth utilization:
- iostat
- dstat
- atop
- nmon

## iostat

```
$ iostat -cdx 2
Linux 4.19.0-7-amd64 (OP-7020-01)   02/02/2020  _x86_64_    (8 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           5.23    0.00    2.83    2.34    0.00   89.60

Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util
sda              0.20    0.05     15.44      5.03     0.00     1.21   0.04  95.79    3.20   17.78   0.00    75.37    94.86   2.11   0.05
sdb              5.29   31.95    225.52    699.95     0.07    11.06   1.28  25.71   23.99   16.88   0.27    42.65    21.90   1.70   6.32
dm-0             5.42   43.00    214.50    726.57     0.00     0.00   0.00   0.00   25.20   22.85   1.12    39.59    16.90   3.46  16.75
dm-1             0.01    0.00      0.67      0.00     0.00     0.00   0.00   0.00   26.36    0.00   0.00    51.04     0.00  21.99   0.03
```

## dstat

```
$ dstat -cdmngys --disk-util --disk-tps --disk-avgqu --disk-svctm

--total-cpu-usage-- -dsk/total- ------memory-usage----- -net/total- ---paging-- ---system-- ----swap--- sda--sdb- -dsk/total- sda--sdb- sda--sdb-
usr sys idl wai stl| read  writ| used  free  buff  cach| recv  send|  in   out | int   csw | used  free|util:util|#read #writ|avgq:avgq|svct:svct
  5   1  64  30   0|8155k 1512k|2389M 11.3G 6044k 2130M|   0     0 |   0     0 |3859    11k|   0    30G|2.84:78.6| 142    62 |0.06:4.26|4.63:3.96
  6   2  68  23   0|  92k 4508k|2401M 11.3G 6044k 2130M|7398B 7963B|   0     0 |9825    31k|   0    30G|   0:37.2|   9   446 |   0:1.64|   0:0.82
  5   2  66  27   0|   0  4180k|2371M 11.3G 6044k 2123M|9217B 6428B|   0     0 |7640    22k|   0    30G|   0:43.6|   0   646 |   0:3.77|   0:0.67
  3   2  78  17   0| 200k 2652k|2373M 11.3G 6044k 2123M|7483B 3764B|   0     0 |5446    14k|   0    30G|   0:69.6|   2   471 |   0:3.68|   0:1.47
  9   6  69  17   0|8444k  892k|2405M 11.3G 6044k 2131M|1628B 6406B|   0     0 |  11k   41k|   0    30G|   0:41.6|  86   191 |   0:0.59|   0:1.50
 10   4  63  23   0|7132k 1728k|2404M 11.3G 6044k 2139M|2324B 2479B|   0     0 |9384    33k|   0    30G|   0:60.8|  81   326 |   0:1.66|   0:1.49
  4   2  74  20   0|5292k 2032k|2408M 11.3G 6044k 2144M|2640B 6795B|   0     0 |6113    16k|   0    30G|   0:76.0|  52   364 |   0:2.86|   0:1.82
  6   3  56  35   0|  11M  412k|2434M 11.2G 6044k 2155M|  10k   15k|   0     0 |8876    29k|   0    30G|   0:60.0| 115    85 |   0:1.03|   0:3.03
  3   1  70  26   0|  24M   68k|2446M 11.2G 6044k 2179M|  14k   34k|   0     0 |5907    16k|   0    30G|   0:88.0| 263    17 |   0:1.78|   0:3.14
  5   1  89   4   0|2616k   52k|2449M 11.2G 6044k 2181M|7083B   36k|   0     0 |5078    14k|   0    30G|   0:20.4|  33    17 |   0:0.23|   0:4.08
  8   3  87   2   0|2332k  128k|2457M 11.2G 6044k 2184M|4222B   18k|   0     0 |8514    34k|   0    30G|   0:12.0|  26    28 |   0:0.14|   0:2.22
  4   1  95   1   0|   0    36k|2457M 11.2G 6044k 2184M|2941B 4573B|   0     0 |4024    11k|   0    30G|   0:   0|   0     9 |   0:   0|   0:   0
  5   2  92   2   0| 240k  100k|2458M 11.2G 6044k 2184M|9440B   17k|   0     0 |5460    15k|   0    30G|   0:8.80|   5    23 |   0:0.09|   0:3.14
  4   2  92   2   0| 168k   60k|2457M 11.2G 6044k 2184M|  29k   38k|   0     0 |6146    15k|   0    30G|   0:7.20|   6    17 |   0:0.07|   0:3.13
  5   2  92   2   0|  24k  120k|2463M 11.2G 6044k 2184M|5677B   12k|   0     0 |6024    20k|   0    30G|   0:4.40|   1    34 |   0:0.06|   0:1.26
  4   2  94   1   0|   0    68k|2460M 11.2G 6044k 2184M|  11k   11k|   0     0 |5086    16k|   0    30G|   0:   0|   0    15 |   0:   0|   0:   0
  4   1  94   1   0|  16k   84k|2461M 11.2G 6044k 2184M|4656B   11k|   0     0 |4808    13k|   0    30G|   0:2.80|   1    23 |   0:0.05|   0:1.17
  2   1  96   0   0|   0    32k|2461M 11.2G 6044k 2184M|7677B 9178B|   0     0 |3956    11k|   0    30G|   0:   0|   0    10 |   0:   0|   0:   0
  4   1  95   1   0|   0    24k|2461M 11.2G 6044k 2184M|7404B 7848B|   0     0 |4227    11k|   0    30G|   0:   0|   0     8 |   0:   0|   0:   0
  3   1  94   1   0|   0    40k|2461M 11.2G 6044k 2184M|3431B 8807B|   0     0 |4554    12k|   0    30G|   0:   0|   0    14 |   0:   0|   0:   0
  8   4  86   2   0| 500k  212k|2462M 11.2G 6044k 2185M|4894B 4823B|   0     0 |8808    34k|   0    30G|   0:10.4|   6    49 |   0:0.11|   0:1.89
  2   1  95   2   0|  16k   24k|2462M 11.2G 6044k 2185M|3174B 7933B|   0     0 |3310  8956 |   0    30G|   0:6.00|   1     9 |   0:0.07|   0:6.00
  2   1  96   1   0|   0    24k|2462M 11.2G 6044k 2185M|4609B   15k|   0     0 |4202    11k|   0    30G|   0:   0|   0     6 |   0:   0|   0:   0
  3   1  96   1   0|   0    44k|2462M 11.2G 6044k 2185M|6262B 8518B|   0     0 |4008    11k|   0    30G|   0:   0|   0    15 |   0:   0|   0:
```

## atop

```
ATOP - OP-7020-01                     2020/02/02  16:18:56                     --------------                     3h27m17s elapsed
PRC | sys   22m11s  | user  49m20s |  #proc    345 | #trun      1 |  #tslpi  1365 | #tslpu     0  | #zombie    0 |  #exit      1 |
CPU | sys      17%  | user     42% |  irq       5% | idle    717% |  wait     19% | ipc notavail  | curf 1.36GHz |  curscal  34% |
cpu | sys       2%  | user      5% |  irq       0% | idle     89% |  cpu002 w  3% | ipc notavail  | curf 1.68GHz |  curscal  42% |
cpu | sys       2%  | user      5% |  irq       0% | idle     90% |  cpu006 w  2% | ipc notavail  | curf 1.05GHz |  curscal  26% |
cpu | sys       2%  | user      5% |  irq       3% | idle     88% |  cpu000 w  2% | ipc notavail  | curf 1.77GHz |  curscal  44% |
cpu | sys       2%  | user      5% |  irq       0% | idle     90% |  cpu003 w  2% | ipc notavail  | curf 1.12GHz |  curscal  28% |
cpu | sys       2%  | user      5% |  irq       0% | idle     90% |  cpu007 w  2% | ipc notavail  | curf  859MHz |  curscal  21% |
cpu | sys       2%  | user      5% |  irq       1% | idle     89% |  cpu001 w  3% | ipc notavail  | curf 2.48GHz |  curscal  62% |
cpu | sys       2%  | user      5% |  irq       0% | idle     90% |  cpu004 w  2% | ipc notavail  | curf  973MHz |  curscal  24% |
cpu | sys       2%  | user      5% |  irq       0% | idle     91% |  cpu005 w  2% | ipc notavail  | curf  946MHz |  curscal  23% |
CPL | avg1    0.82  | avg5    1.22 |  avg15   1.21 |              |  csw 415298e3 | intr 68192e3  |              |  numcpu     8 |
MEM | tot    15.6G  | free    6.8G |  cache   5.0G | buff   25.4M |  slab  650.6M | shmem 390.9M  | vmbal   0.0M |  hptot   0.0M |
SWP | tot    29.8G  | free   29.8G |               |              |               |               | vmcom  14.7G |  vmlim  37.6G |
LVM |      os-root  | busy     17% |  read   66844 | write 533385 |  KiB/w     16 | MBr/s    0.2  | MBw/s    0.7 |  avio 3.46 ms |
LVM |      os-swap  | busy      0% |  read     163 | write      0 |  KiB/w      0 | MBr/s    0.0  | MBw/s    0.0 |  avio 22.0 ms |
DSK |          sdb  | busy      6% |  read   65234 | write 397037 |  KiB/w     21 | MBr/s    0.2  | MBw/s    0.7 |  avio 1.69 ms |
DSK |          sda  | busy      0% |  read    2527 | write    654 |  KiB/w     94 | MBr/s    0.0  | MBw/s    0.0 |  avio 2.11 ms |
NET | transport     | tcpi 2847603 |  tcpo 2798383 | udpi  115650 |  udpo   99884 | tcpao 226492  | tcppo   9402 |  tcprs 197790 |
NET | network       | ipi  3010484 |  ipo  3124406 | ipfrw  32674 |  deliv 2977e3 |               | icmpi  10928 |  icmpo    957 |
NET | wlx6c5a   0%  | pcki 1421819 |  pcko 1518298 | sp  108 Mbps |  si  487 Kbps | so  139 Kbps  | erri       0 |  erro       0 |
NET | calib1d   0%  | pcki   40428 |  pcko   24341 | sp   10 Gbps |  si    1 Kbps | so    5 Kbps  | erri       0 |  erro       0 |
NET | calibeb   0%  | pcki   40341 |  pcko   24273 | sp   10 Gbps |  si    1 Kbps | so    5 Kbps  | erri       0 |  erro       0 |
NET | calic62   0%  | pcki    5750 |  pcko    6529 | sp   10 Gbps |  si    0 Kbps | so    2 Kbps  | erri       0 |  erro       0 |
NET | cali7d4   0%  | pcki    9853 |  pcko   10632 | sp   10 Gbps |  si    0 Kbps | so    0 Kbps  | erri       0 |  erro       0 |
NET | lo      ----  | pcki 1547372 |  pcko 1547372 | sp    0 Mbps |  si  300 Kbps | so  300 Kbps  | erri       0 |  erro       0 |
NET | tunl0   ----  | pcki       0 |  pcko     434 | sp    0 Mbps |  si    0 Kbps | so    0 Kbps  | erri       0 |  erro       0 |
                                          *** system and process activity since boot ***
  PID    SYSCPU    USRCPU     VGROW     RGROW    RUID        EUID        ST    EXC     THR    S    CPUNR     CPU    CMD      1/346
 3163     4m44s     6m35s    539.4M    365.9M    root        root        N-      -      22    S        3      6%    kube-apiserver
```

## nmon

```

┌nmon─16i─────────────────────Hostname=OP-7020-01───Refresh=10secs ───16:23.01───────────────────────────────────────────────────┐
│ Disk I/O ──/proc/diskstats────mostly in KB/s─────Warning:contains duplicates───────────────────────────────────────────────────│
│DiskName Busy  Read WriteKB|0          |25         |50          |75       100|                                                  │
│sda        0%    0.0    0.0|>                                                |                                                  │
│sda1       0%    0.0    0.0|>                                                |                                                  │
│sda2       0%    0.0    0.0|>                                                |                                                  │
│sda3       0%    0.0    0.0|>                                                |                                                  │
│sda4       0%    0.0    0.0|>                                                |                                                  │
│sda5       0%    0.0    0.0|>                                                |                                                  │
│sda6       0%    0.0    0.0|>                                                |                                                  │
│sdb        1%    0.0   96.4|>                                                |                                                  │
│sdb1       0%    0.0    0.0|>                                                |                                                  │
│sdb2       0%    0.0    0.0|>                                                |                                                  │
│sdb3       1%    0.0   96.4|>                                                |                                                  │
│sdb4       0%    0.0    0.0|>                                                |                                                  │
│dm-0      12%    0.0  102.0|WWWWW>                                           |                                                  │
│dm-1       0%    0.0    0.0|>                                                |                                                  │
│Totals Read-MB/s=0.0      Writes-MB/s=0.3      Transfers/sec=65.4                                                               │
│────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────│
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
│                                                                                                                                │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## ioping

```
$ ioping -c 9 /tmp/
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=1 time=332.0 us (warmup)
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=2 time=447.4 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=3 time=561.7 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=4 time=329.4 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=5 time=399.0 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=6 time=368.5 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=7 time=195.1 us (fast)
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=8 time=376.2 us
4 KiB <<< /tmp/ (xfs /dev/dm-0): request=9 time=356.8 us

--- /tmp/ (xfs /dev/dm-0) ioping statistics ---
8 requests completed in 3.03 ms, 32 KiB read, 2.64 k iops, 10.3 MiB/s
generated 9 requests in 8.00 s, 36 KiB, 1 iops, 4.50 KiB/s
min/avg/max/mdev = 195.1 us / 379.3 us / 561.7 us / 97.1 us
```
