# Measure GPU performance by glmark2

We use glmark2 to measure performance of GPU in guest domains (linux and android) and driver domain.

## Run glmark2 in domd
Download and unpack prebuilt binaries [glmark2_domd.zip](glmark2_domd.zip)

Copy files from PC to domd on board
```
scp -r glmark2_domd/usr/ root@${BOARD_IP}:/usr/
```

Open minicom or connect by ssh and login to domd. Run test
```
root@salvator-x-h3-4x2g-xt-domd:~# glmark2-es2-wayland --size 1920x1080
```
You shall see in console
```
=======================================================
    glmark2 2017.07
=======================================================
    OpenGL Information
    GL_VENDOR:     Imagination Technologies
    GL_RENDERER:   PowerVR Rogue GX6650
    GL_VERSION:    OpenGL ES 3.2 build 1.10@5187610
```
Test runs few minutes.

## Run glmark2 in domu
You can follow all steps for domd with one change - use dedicated port for copying files into domu
```
scp -r -P 2024 glmark2_domd/usr/ root@${BOARD_IP}:/home/root/usr/
```

## Run glmark2 in doma
Download [glmark2.apk](glmark2.apk)

Install application
```
adb install glmark2.apk
```
Connect to device by adb, clear logcat, and run test
```
adb connect ${BOARD_IP}
adb logcat -c
adb shell am start org.linaro.glmark2/.Glmark2Activity
adb logcat
```
Also you can monitor logcat in separate window during running of test. Just `adb logcat` in one terminal and after that `adb shell am start org.linaro.glmark2/.Glmark2Activity` in another terminal.
