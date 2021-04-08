# ans-ios
Ansible scripts for Cisco IOS, IOS-XE
repo: https://github.com/ire-k/ans-ios



      #### memory free low watermark 
      # warnings in logs
      # adj per platform/software
      # some results below
      #
      #          ## reccomended: 20% of current memory
		#   left after: memory reserve critical
# https://community.cisco.com/t5/switching/memory-free-low-watermark-and-processor-settings-for-2960x/td-p/3006999
#https://www.cisco.com/en/US/docs/ios/12_3t/12_3t4/feature/guide/gt_memnt.html
#https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/bsm/configuration/15-2mt/bsm-mem-thresh-note.html          

# Cisco IOS Software, Catalyst 4500 L3 Switch Software (cat4500e-IPBASEK9-M), Version 12.2(54)SG1, RELEASE SOFTWARE (fc1)
##plan_PIK11-SW2-4900#sh proc memory             
#Processor Pool Total:  465346528 Used:  206055968 Free:  259290560
# 20% -> 90000 kB
  #LINX-SW7-4900(config)#memory free low-watermark io 13000
  #%Error: IO memory and processor memory are the same
  #Configure the low water mark for processor memory


## plan_PIK11-SW1-cME3600X#sh proc memory
#Processor Pool Total:  815909596 Used:  169799804 Free:  646109792
#      I/O Pool Total:   67100672 Used:   28245104 Free:   38855568
# processor 20% -> 159000 kB
# i/o 20%       -> 13000 kB

## lab_c3560G
##sh proc mem
#Processor  Pool Total:   76961468 Used:   19430096 Free:   57531372
#I/O        Pool Total:    8388608 Used:    3590640 Free:    4797968
#Driver te  Pool Total:    1048576 Used:         40 Free:    1048536
# 20% proc -> 200 kB --- see below for ME3400 ! use Driver te
# 20% i/o  -> 1600 kB
#
#
#3560(config)#memory free low-watermark io ?
#  <1-4294967295>  low water mark of memory in KB

      # 3560-24PS WS-C3560-24PS-S
#Head    Total(b)     Used(b)     Free(b)   Lowest(b)  Largest(b)
#Processor    17BD718    96741608    24769352    71972256    69495080    68100200
#      I/O    7400000    12574720     9031656     3543064     3503528     3535816

#lab_cME3400#sh proc memory
#Processor Pool Total:   69446432 Used:   24325716 Free:   45120716
#      I/O Pool Total:   12582912 Used:    8537504 Free:    4045408
#Driver te Pool Total:    1048576 Used:         40 Free:    1048536
# 20% pool total = 14000kB
# 20% io = 2400kB
# ####
# After memory reserved critical 4096 on ME3400:
#Processor Pool Total:   69446432 Used:   26609124 Free:   42837308
#      I/O Pool Total:   12582912 Used:   10900292 Free:    1682620
#Critical  Pool Total:    1836280 Used:         40 Free:    1836240
#Critical  Pool Total:    2358024 Used:         40 Free:    2357984
#Driver te Pool Total:    1048576 Used:         40 Free:    1048536

#After mem res cri 2048 on ME3400
#lab_cmp-sw2-cME3400#sh proc mem
#Processor Pool Total:   69446432 Used:   25847956 Free:   43598476
#      I/O Pool Total:   12582912 Used:    9808728 Free:    2774184
#Critical  Pool Total:     918140 Used:         40 Free:     918100
#Critical  Pool Total:    1179012 Used:         40 Free:    1178972
#Driver te Pool Total:    1048576 Used:         40 Free:    1048536


#Feb 17 2021 11:56:46.183 CET: %SYS-4-FREEMEMLOW: Free Memory has dropped below low watermark
#Pool: Processor  Free: 10728268  Threshold: 14336000
#ztm-sw-stoklosy(config)#memory free low-watermark processor 200
#Feb 17 2021 12:07:24.346 CET: %SYS-5-FREEMEMRECOVER: Free Memory has recovered above low watermark
#
#Pool: Processor  Free: 10719416  Threshold: 204800 
# ## use 10% of I/O : 1200 kB ## reserve critical uses memory
# ## low-watermark processor: use 20% of "Driver te" : 200 kB

#ztm-sw-stoklosy(config)#memory reserve critical 4096
#Feb 17 2021 12:11:00.741 CET: %SYS-4-FREEMEMLOW: Free Memory has dropped below low watermark
#Pool: I/O  Free: 1645556  Threshold: 2457600


#ztm-sw-stoklosy(config)#memory reserve critical 2048
#Memory Pool:Processor already has critical memory reserved
#New configuration will take effect only after the next reload upon saving
#the current configuration
#Memory Pool:I/O already has critical memory reserved
#New configuration will take effect only after the next reload upon saving
#the current configuration


      ## 3750G
      #inventory
      #NAME: "1", DESCR: "WS-C3750-24PS"
      #PID: WS-C3750-24PS-S  , VID: V08  , SN: FOC1446Y14S
      #
      #Processor Pool Total:   71359432 Used:   31742852 Free:   39616580
      #      I/O Pool Total:   12582912 Used:    8668732 Free:    3914180
      #Driver te Pool Total:    1048576 Used:         40 Free:    1048536
      #
      #mem fre low wat proc -> ME3400 -> use Driver te Pool Total * 20% = 200kB
      #20% of IO: 2457 kB 
      #
      #afert memory reserve crit 2048:
      #Processor Pool Total:   71359432 Used:   32709040 Free:   38650392
      #I/O Pool Total:   12582912 Used:    9816420 Free:    2766492
      #Critical  Pool Total:     941316 Used:         40 Free:     941276
      #Critical  Pool Total:    1147644 Used:         40 Free:    1147604
      #Driver te Pool Total:    1048576 Used:         40 Free:    1048536


#	Before 'mem reserv crit 2048'
#        Processor Pool Total:   71359432 Used:   31742852 Free:   39616580
#              I/O Pool Total:   12582912 Used:    8668732 Free:    3914180
#        Driver te Pool Total:    1048576 Used:         40 Free:    1048536
#


########################### ASR 1002 RP1 ESP10 #################################

 On the ASR1K platform, the lsmpi_io pool has little free memory â generally less than 1000 bytes â which is normal. Cisco recommends that you disable monitoring of the LSMPI pool by the network management applications in order to avoid false alarms.
https://www.cisco.com/c/en/us/support/docs/routers/asr-1000-series-aggregation-services-routers/116777-technote-product-00.html

## ASR 1002 ( without -X) RP1 can have only 4GB RAM, about 2GB will be reserved for IOS (IOSd), the rest will take the main operating system: linux

LINX-RT10-cASR1002_2#sh proc mem
Load for five secs: 12%/4%; one minute: 17%; five minutes: 13%
Time source is NTP, 15:18:34.441 CEST Thu Apr 8 2021

Processor Pool Total: 1681142684 Used:  361999712 Free: 1319142972
 lsmpi_io Pool Total:    6295088 Used:    6294116 Free:        972
...
This command only shows processes inside the IOS daemon.
Please use 'show processes memory platform'
to show processes from the underlying operating system.

LINX-RT10-cASR1002_2#sh mem
Load for five secs: 11%/4%; one minute: 16%; five minutes: 13%
Time source is NTP, 15:18:45.108 CEST Thu Apr 8 2021

                Head    Total(b)     Used(b)     Free(b)   Lowest(b)  Largest(b)
Processor   300CD008   1681142684   361994516   1319148168   489193972   489863348
 lsmpi_io   945681D0     6295088     6294120         968         968         968


This command only shows processes inside the IOS daemon.
Please use 'show memory platform'
to show processes from the underlying operating system.

LINX-RT10-cASR1002_2#show memory platform
Load for five secs: 12%/4%; one minute: 14%; five minutes: 13%
Time source is NTP, 15:19:24.139 CEST Thu Apr 8 2021
  Virtual memory   : 3476074496
  Pages resident   : 540630
  Major page faults: 849
  Minor page faults: 4593492383

  Architecture     : ppc
  Memory (kB)
    Physical       : 4127744
    Total          : 3874492
    Used           : 3061176
    Free           : 813316
    Active         : 2040644
    Inactive       : 920520
    Inact-dirty    : 0
    Inact-clean    : 0
    Dirty          : 156
    AnonPages      : 1433804
    Bounce         : 0
    Cached         : 1380240
    Commit Limit   : 1937244
    Committed As   : 2890468
    High Total     : 3191740
    High Free      : 368156
    Low Total      : 682752
    Low Free       : 445160
    Mapped         : 613252
    NFS Unstable   : 0
    Page Tables    : 7344
    Slab           : 62328
    VMmalloc Chunk : 204620
    VMmalloc Total : 233184
    VMmalloc Used  : 24768
    Writeback      : 0

  Swap (kB)
    Total          : 0
    Used           : 0
    Free           : 0
    Cached         : 0

  Buffers (kB)     : 149024

  Load Average
    1-Min          : 0.16
    5-Min          : 0.09
    15-Min         : 0.03

LINX-RT10-cASR1002_2#sh inv
Load for five secs: 29%/11%; one minute: 14%; five minutes: 13%
Time source is NTP, 15:20:10.121 CEST Thu Apr 8 2021
NAME: "Chassis", DESCR: "Cisco ASR1002 Chassis"
PID: ASR1002           , VID: V05, SN: FOX1605G7DD

NAME: "module F0", DESCR: "Cisco ASR1000 Embedded Services Processor, 10Gbps"
PID: ASR1000-ESP10     , VID: V05, SN: JAE151905FL
         
NAME: "Power Supply Module 1", DESCR: "Cisco ASR1002 DC Power Supply"
PID: ASR1002-PWR-DC    , VID: V02, SN: ART1532V02B

NAME: "module 0", DESCR: "Cisco ASR1002 SPA Interface Processor 10"
PID: ASR1002-SIP10     , VID:    , SN:            

NAME: "SPA subslot 0/1", DESCR: "1-port 10 Gigabit Ethernet Shared Port Adapter XFP based"
PID: SPA-1X10GE-L-V2   , VID: V02, SN: JAE11485LCL

NAME: "SPA subslot 0/2", DESCR: "1-port 10 Gigabit Ethernet Shared Port Adapter XFP based"
PID: SPA-1X10GE-L-V2   , VID: V04, SN: SAL183608SF

NAME: "SPA subslot 0/3", DESCR: "1-port 10 Gigabit Ethernet Shared Port Adapter XFP based"
PID: SPA-1X10GE-L-V2   , VID: V02, SN: JAE1245ZKEF

NAME: "module R0", DESCR: "Cisco ASR1002 Route Processor 1"
PID: ASR1002-RP1       , VID: V05, SN: JAE160903DM

show platform software status control-processor brief 
LINX-RT10-cASR1002_2#show platform software status control-processor brief 
Load for five secs: 11%/4%; one minute: 12%; five minutes: 12%
Time source is NTP, 15:30:23.230 CEST Thu Apr 8 2021
Load Average
 Slot  Status  1-Min  5-Min 15-Min
  RP0 Healthy   0.00   0.00   0.00
 ESP0 Healthy   0.00   0.00   0.00
 SIP0 Healthy   0.00   0.00   0.00

Memory (kB)
 Slot  Status    Total     Used (Pct)     Free (Pct) Committed (Pct)
  RP0 Healthy  3874492  3061548 (79%)   812944 (21%)   2890340 (75%)
 ESP0 Healthy  2009416  1068288 (53%)   941128 (47%)    892832 (44%)
 SIP0 Healthy   471820   415076 (88%)    56744 (12%)    411024 (87%)

CPU Utilization
 Slot  CPU   User System   Nice   Idle    IRQ   SIRQ IOwait
  RP0    0  13.00   5.60   0.00  81.00   0.40   0.00   0.00
 ESP0    0   3.90  19.90   0.00  76.00   0.10   0.10   0.00
 SIP0    0   6.10   2.00   0.00  91.90   0.00   0.00   0.00

# also worth seeing:
https://www.cisco.com/c/en/us/support/docs/routers/asr-1000-series-aggregation-services-routers/116777-technote-product-00.html

## memory reserve section:
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/fundamentals/command/cf_command_ref/L_through_mode.html

https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/bsm/configuration/15-2mt/bsm-mem-thresh-note.html
