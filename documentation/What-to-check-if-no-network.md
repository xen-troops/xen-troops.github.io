# What to check if no network

This page is very draft and early version of document

## domD
* ping google.com
* ping google by IP (get address on your PC)
If you can ping by IP but can't by name - you have DNS related issue. 

Check addresses of name servers used
```
cat /etc/resolv.conf
```
If you see '8.8.8.8' - this is Google and this is OK in MOST cases, but not OK if you are inside network which use it's own name servers, so check DNS on working PC in same network.

You can check name servers used for specific interface and fallback servers by
```
systemd-resolve --status --no-pager
```

In some cases you may get additional info using some of following commands
```
journalctl -u systemd-networkd.service --no-pager
journalctl -u systemd-resolved.service --no-pager
systemctl status systemd-resolved --no-pager
systemctl status dnsmasq --no-pager
```

Check that the external interface is properly initialized: `networkctl` should display eth0 as `configured`, not `configuring`.
If interface stuck in `configuring` this blocks the start of `systemd-networkd-wait-online.service`. 

Check output of `journalctl` for possible duplication of MAC of your board.

## domA
If you see exclamation sign near clock in the top line - you have no network.

Connect by adb (usually you can do this if network works in domD), or just by serial console to domA. And do following:
* ping google.com
* ping google by IP (get address on your PC)
If you can ping by IP but can't by name - you have DNS related issue and in most cases you need to fix this in domD.

In some cases you may check info regarding android's subsystems (use `adb shell` or minicom)
```
dumpsys netd
dumpsys network_management  ## dump DHCP communications
```
