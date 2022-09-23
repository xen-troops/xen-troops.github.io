# Work with camera in different domains

This page is draft and early version of document

## domD/domU (Linux)
```
media-ctl -d /dev/media0 -l "'rcar_csi2 feaa0000.csi2':1 -> 'VIN5 output':0 [1]"
media-ctl -d /dev/media0 -l "'rcar_csi2 feaa0000.csi2':2 -> 'VIN4 output':0 [1]"
# 640x480p
media-ctl -d /dev/media0 -V "'rcar_csi2 feaa0000.csi2':1 [fmt:RGB888_1X24/640x480 field:none]"
media-ctl -d /dev/media0 -V "'adv748x 4-0070 hdmi':1 [fmt:RGB888_1X24/640x480 field:none]"
echo 2 > /sys/class/video4linux/video0/dev_debug && echo 8 > /proc/sys/kernel/printk
v4l2-ctl --set-fmt-video=width=640,height=480,pixelformat=AR24 --stream-mmap --stream-count=100 -d /dev/video0 --stream-to=./z.x
hexdump -C -n 1000 ./z.x

###
media-ctl -r
media-ctl -d /dev/media0 -l "'rcar_csi2 feaa0000.csi2':1 -> 'VIN0 output':0 [1]"
media-ctl -d /dev/media0 --get-v4l2 "'adv748x 4-0070 hdmi':1"
    [fmt:RGB888_1X24/1024x768 field:none]
media-ctl -d /dev/media0 -V "'adv748x 4-0070 hdmi':1 [fmt:RGB888_1X24/1024x768 field:none]"
media-ctl -d /dev/media0 -V "'rcar_csi2 feaa0000.csi2':1 [fmt:RGB888_1X24/1024x768 field:none]"
v4l2-ctl --set-fmt-video=width=1024,height=768,pixelformat=AR24 --stream-mmap --stream-count=100 -d /dev/video0 --stream-to=./z.x
hexdump -x -n 100 ./z.x

vlc --demux rawvideo --rawvid-fps 60 --rawvid-width 1024 --rawvid-height 768 --rawvid-chroma=RV32 /nfs/salvator-x/z.x

### USB camera in domD
v4l2-ctl --set-fmt-video=width=640,height=480,pixelformat=YUYV --stream-mmap --stream-count=100 -d /dev/cam_front --stream-to=./z.x
vlc --demux rawvideo --rawvid-fps 7 --rawvid-width 640 --rawvid-height 480 --rawvid-chroma=YUYV ~/Downloads/z.x
```

## domA (Android)
NOTE. This is not regular android camera, this is Exterior View System. It has different API. So you can't use regular Camera application.
For some simple tests you can use `evs_app_xt`
```
adb shell evs_app_xt --hw --test
```
