# Flash image to eMMC from SD card

## Long story short
1. Boot from SD into dom0 of image that you need to flash
2. Destroy all user domains `xl destroy DomA`, DomU, DomF etc
3. Switch to domd `xl console DomD` and run in it:
```
systemctl isolate rescue
mount -o remount,ro /
dd if=/dev/mmcblk2 of=/dev/mmcblk0 bs=1M count=9216 status=progress
```

## Detailed instruction for manual flashing
1) Run `minicom`
2) Turn on board with SD card inserted
3) Press Enter repeatedly until u-boot's prompt `=>` appears
```
Hit any key to stop autoboot:  0 
=> 
=> 
=> 
=> 
=> 
```
4) Run two commands in minicom
```
setenv boot_dev mmcblk1
run bootcmd_mmc0
```
This will continue regular boot from SD.
5) Log in into dom0: `root`
Wait for messages
```
Parsing config from /xt/dom.cfg/doma.cfg
(XEN) Added GSX guest domain 2
(XEN) ipmmu: /soc/mmu@e67b0000: d2: Set IPMMU context 1 (pgd 0x75dc02000)
(XEN) ipmmu: /soc/gsx_domu: Initialized master device (IPMMUs 4 micro-TLBs 4)
(XEN) ipmmu: /soc/gsx_domu: IPMMU1: /soc/mmu@fd800000
(XEN) ipmmu: /soc/gsx_domu: IPMMU2: /soc/mmu@fd950000
(XEN) ipmmu: /soc/gsx_domu: IPMMU3: /soc/mmu@fd960000
(XEN) ipmmu: /soc/gsx_domu: IPMMU4: /soc/mmu@fd970000
(XEN) ipmmu: /soc/gsx_domu: Using IPMMU context 1
xl destroy DomA(XEN) printk: 18 messages suppressed.
(XEN) d2v0: vGICD: unhandled word write 0xffffffff to ICACTIVER4
(XEN) d2v0: vGICD: unhandled word write 0xffffffff to ICACTIVER8
(XEN) d2v0: vGICD: unhandled word write 0xffffffff to ICACTIVER12
(XEN) d2v0: vGICD: unhandled word write 0xffffffff to ICACTIVER16
(XEN) d2v0: vGICD: unhandled word write 0xffffffff to ICACTIVER0
(XEN) d2v1: vGICD: unhandled word write 0xffffffff to ICACTIVER0
(XEN) d2v2: vGICD: unhandled word write 0xffffffff to ICACTIVER0
```
and run commands:
```
xl destroy DomA
xl console DomD
root
```
Wait up to 3 minutes until cluster appears (applicable only if cluster is present in your image), then
```
systemctl isolate rescue
```
Wait up to 3 minutes until you see `h3ulcb-4x2g-kf-xt-domd login:` then log in and run commands:
```
root
mount -o remount,ro /
dd if=/dev/mmcblk1 of=/dev/mmcblk0 bs=1M count=9216 status=progress
```
Wait 10 minutes.
Turn off board.
Remove SD card.
Turn on board.

## Flashing with script for minicom
Download [script](update_emmc_from_sd.script).
It's plain text, no execution rights needed.

1) Insert SD card.
2) Run minicom in script execution mode (use your ttyUSB* name)
```
minicom --script=./update_emmc_from_sd.script -D /dev/ttyUSB0
```
3) Turn on board and check that you see logs.
4) Script will switch board to load from SD, wait for domains, destroy domA, switch to domD and do all steps according from manual instruction above.
5) You should wait about 15 minutes. There are two places with long awaiting:
* "Waiting for DomA to start (up to 5 minutes)..."
* "Flashing of image takes about 9 minutes, so set timeout to 600 sec" with progress output
6) If everything is OK, board will reboot and boot from eMMC. SD card need to be removed, to be sure that it is not used.
