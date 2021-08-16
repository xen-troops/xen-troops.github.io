# X86 Xen on QEMU (with PVH Dom0 support)
>These instructions allow running a complete QEMU based environment for running X86 Xen.

## Compile QEMU binaries
> You might want trying the QEMU provided by your OS distribution before building your own
```sh
git clone https://github.com/qemu/qemu.git
git checkout origin/stable-5.0 --track
./configure --target-list=x86_64-softmmu
make
```

## Get the image for the Domain-0 root file system
```sh
wget https://cloud.debian.org/images/cloud/buster/20210721-710/debian-10-nocloud-amd64-20210721-710.qcow2
ln -s debian-10-nocloud-amd64-20210721-710.qcow2 dom0.qcow2
qemu-img resize dom0.qcow2 +30G
```

## Prepare and run QEMU
Create run-x86-qemu.sh:
```sh
QEMU_BIN=${PWD}/qemu/x86_64-softmmu/qemu-system-x86_64

$QEMU_BIN -hda dom0.qcow2 -m 8192 \
    -nographic -bios bios-256k.bin \
    -vga none -s -cpu host \
    -enable-kvm -M q35 -smp 4 -device intel-iommu,intremap=off \
    -device ioh3420,id=root_port1,chassis=1 \
    -device x3130-upstream,id=upstream_port1,bus=root_port1 \
    -device xio3130-downstream,id=downstream_port1,bus=upstream_port1 \
    -device e1000e,netdev=net0 \
    -netdev user,id=net0,hostfwd=tcp::2222-:22,net=10.0.3.0/24 \
    -device e1000e,netdev=net1,bus=downstream_port1 \
    -netdev user,id=net1,net=10.0.2.0/24
```
and run it
```sh
sudo ./run-x86-qemu.sh
```
After booting change the default host name for easier guest domain identification:
```sh
root@debian:~# vi /etc/hostname
```
and change default "debian" to "dom0"

## Altering the Domain-0 root file system 
In case you need to modify the root file system use the following commands to mount and umount:
```sh
sudo guestmount -a dom0.qcow2 -i mnt/
sudo guestunmount mnt
```

## Update default Debian environment

Update the kernel to more recent one:
```sh
apt update
apt-get install linux-image-5.10.0-0.bpo.8-amd64 linux-headers-5.10.0-0.bpo.8-amd64 
```

Create a new user:
```sh
adduser a2k
usermod -aG sudo a2k
getent group sudo
```

Allow password authentication for sshd:
```sh
vi /etc/ssh/sshd_config
PasswordAuthentication yes
dpkg-reconfigure openssh-server
systemctl restart ssh
```

And connect to the QEMU with ssh:
```sh
ssh a2k@localhost -p 2222
```

## Install build essentials
To be able to build the essential software install the following packages:
```sh
sudo apt-get install git
sudo apt-get install build-essential linux-source bc kmod cpio flex libncurses5-dev libelf-dev libssl-dev
sudo apt-get build-dep xen python3-dev ninja-build libsystemd-dev 
```

## Build and install Xen
```sh
a2k@debian:~/projects/
git clone https://xenbits.xen.org/git-http/xen.git
git checkout origin/stable-4.15 --track
./configure --disable-docs --disable-stubdom --prefix=/usr/local --libdir=/usr/lib --enable-systemd
CC=gcc make -j4 debball
```

And install it
```sh
sudo dpkg -i dist/xen-upstream-4.15.1-pre.deb

sudo systemctl enable xen-qemu-dom0-disk-backend.service
sudo systemctl enable xen-init-dom0.service
sudo systemctl enable xenconsoled.service
sudo systemctl enable xendomains.service
sudo systemctl enable xen-watchdog.service
sudo systemctl enable xendriverdomain.service
```

Edit /etc/default/grub and update the command line for Xen and the kernel:
```
GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 console=hvc0"
GRUB_CMDLINE_XEN="iommu=no-sharept,verbose,debug dom0_mem=4096M,max:4096M debug=y loglvl=all guest_loglvl=all com1=115200,8n1 console=com1,vga sync_console_to_ring=true sync_console"
```
For PVH Dom0 add "dom0=pvh" to the `GRUB_CMDLINE_XEN` command line.
Set `GRUB_DEFAULT=2` for Xen to boot first

Update the grub and restart:
```sh
sudo update-grub
sudo reboot
```
Test that everything is set up right after the reboot:
```sh
sudo xl info
```
During Xen development use the following to test the changes:
```sh
CC=gcc make -C xen/ -j4
sudo cp xen/xen.gz /boot/xen-4.15.gz
sudo reboot
```
## Download DomU's root file system
```sh
wget https://cloud.debian.org/images/cloud/buster/20210721-710/debian-10-nocloud-amd64-20210721-710.qcow2
ln -s debian-10-nocloud-amd64-20210721-710.qcow2 domu.qcow2
```
## Donwload and build DomU's Linux kernel
```sh
git clone https://github.com/torvalds/linux.git
git checkout v5.10
make x86_64_defconfig
make xen.config
```
Update the configuration to make the network and block device drivers be built in into the kernel:
```s
make menuconfig
XEN_NETDEV_FRONTEND=y
XEN_BLKDEV_FRONTEND=y
make -j4 bzImage
```
## Create configuration for Xen guest DomU guest.cfg 
```sh
kernel='linux/arch/x86/boot/bzImage'
#type="hvm"
name='DomU'
memory='1024'
disk= [ 'format=qcow2, vdev=xvda, access=rw, target=domu.qcow2' ]
cmdline= 'console=hvc0 earlyprintk=xen root=/dev/xvda1'
vcpus=4
passthrough="sync_pt"
pci=["03:00.0,seize=1"] 
```
> Please note, that for PVH Dom0 PCI passthrough is not supported, so the last to lines of the configuration need to be commented.

## Run the guest
```sh
sudo xl create -c guest.cfg
```
After the guests boots login and update its host name:
```sh
root@debian:~# vi /etc/hostname
Set to domu
```
