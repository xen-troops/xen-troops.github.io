# Measure disk performance by fio

## Installation of test
Download prebuilt binaries of test and required library
* [fio_3.12-r0_aarch64.ipk](fio_3.12-r0_aarch64.ipk)
* [libaio1_0.3.111-r0_aarch64.ipk](libaio1_0.3.111-r0_aarch64.ipk)

Copy them from PC into domd
```
scp fio_3.12-r0_aarch64.ipk root@${BOARD_IP}:/home/root/
scp libaio1_0.3.111-r0_aarch64.ipk root@${BOARD_IP}:/home/root/
```
If you want to copy files into domu (linux guest domain) - just add option `-P 2024` before file name.

Connect to board by minicom or ssh and log into domain. Install test
```
opkg install libaio1_0.3.111-r0_aarch64.ipk
opkg install fio_3.12-r0_aarch64.ipk
```

## Run test
For read test
```
fio --randrepeat=1 --direct=1 --gtod_reduce=1 --name=test --filename=random_read.fio --bs=4k --iodepth=64 --size=200M --readwrite=randread
```
For write test
```
fio --randrepeat=1 --direct=1 --gtod_reduce=1 --name=test --filename=random_read.fio --bs=4k --iodepth=64 --size=200M --readwrite=randwrite
```
In both cases you can change block size `--bs` to measure performance with different settings.
