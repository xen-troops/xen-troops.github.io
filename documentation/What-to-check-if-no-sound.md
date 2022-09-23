# What to check if no sound

This document is draft and contains just list of notes.

## In domD
Check presence of audio devices
```
ls -la /dev/pcm*
```

Check that devices are visible to ALSA
```
aplay -l
```

Check playback
```
cat /dev/urandom | aplay -f S24_LE -c 2
```
Also you can check ALSA playback without Pulse plugin, directly to hardware
```
systemctl stop pulseaudio
cat /dev/urandom | aplay -f S24_LE -c 2 -D hw:0,0
```

Check volume settings
```
amixer -c0 set "DVC Out" 1%
```
Pay attention that value higher than 3% results in audio distortions.

## In domA
At first you need to provide some .wav file to domA
```
adb connect <...>
adb root && adb remount
adb push 48000_16_2ch.wav /data/
adb shell sync
```

After that you can use `tinyplay` to check playback without Android's audio stack. Login into domA and use
```
tinyplay /data/48000_16_2ch.wav
```
