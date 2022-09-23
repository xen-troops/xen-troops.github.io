# What to check if you see errors with optee/keymaster/gatekeeper

**Step 1.** Check your loaders

If you see no log messages after "BL31" - you have incorrect optee-os flashed. This is 'tee.srec' or 'tee.bin' depending on way of flashing. If optee-os is OK you will see messages from u-boot. This doesn't mean that your optee-os supports virtualization or is up-to-date, but at least it provides basic functionality to boot the board.
Also check build date and versions reported of loaders - this may help in debug of further errors.

**Step 2.** Check your XEN

There should be following string in boot-up logs:
```
(XEN) Using TEE mediator for OP-TEE
```

**Step 3.** Check your domain config

Let's assume that we use android as guest, so check `/xt/dom.cfg/doma.cfg` and find in it strings with `firmware#1` in kernel command-line and separate string with `tee=native` or `tee=optee`.

What to expect - `native` or `optee` depends on version of xen, but in common case you should see `tee=optee` if your build of prod-devel was made after start of 2020. For older prod-devel's and some other products you may see `tee=native`. In any case it need to be there.

**Step 4.** Check your domain's virtual device

Log into guest domain and check presence of required virtual devices.
```
xl console DomA
ls -la /dev/tee*
```
You should see at least two devices `/dev/tee0` and `/dev/teepriv0`:
```
console:/ $ ls -la /dev/tee*
crw-rw---- 1 system keystore 249,   0 2020-04-13 18:12 /dev/tee0
crw-rw---- 1 system keystore 249,  16 2020-04-13 18:12 /dev/teepriv0
```
If all check are OK, you may run tests for KeyMaster and GateKeeper. But this is separate story for dedicated page.
