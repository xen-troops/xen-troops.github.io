# Flash image to SD card with bmaptool

**NOTE 1**: in following examples we assume that SD card is present as linux device `/dev/sdX`, so you need to edit provided commands according to your setup.

**NOTE 2**: Be careful with following commands. Using one incorrect symbol MAY ERASE your working disk!

In case you need to write image to SD card, you always can use `dd` like this
```
sudo dd if=img.img of=/dev/sdX bs=1M status=progress
```
This works always but takes quite long time up to 20 minutes depending on SD card.

In order to speed up writing to SD we can use bmaptool. Main idea implemented by this utility - write only non-zero blocks. So, depending on image file this can be few faster.

Install it.
```
sudo apt install bmap-tools
```
And use like this
```
bmaptool create -o img.img.bmap ./img.img
sudo bmaptool copy --bmap img.img.bmap ./img.img /dev/sdX
```
Pay attention that you need to use `create` only if image is changed. And you have to use 'create' if image is changed. When you `create`, utility scans image and generate a map of empty blocks. It's just xml so you can check it.
Also you can use `--nobmap` option like this
```
sudo bmaptool copy --nobmap ./img.img /dev/sdX
```
but I found that it takes longer than two above mentioned commands `create`+`copy`. I don't know why, just trust me - I'm an engineer.
