# Hints regarding backends

We have following backends:

| 'Normal' name   | Service  | Binary    |
|-----------------|----------|-----------|
| sound backend   | sndbe    | snd_be    |
| display backend | displbe  | displ_be  |
| camera backend  | camerabe | camera_be |

Backends run in domD. So all commands mentioned below can be executed only in domD.

**How to check that backend is running?**
```
systemctl status <service_name>
```
Or just grep in list of processes. Like this
```
ps ax | grep snd
```

**How to stop backend?**
```
systemctl stop <service_name>
```
Pay attention that in old builds of prod-devel sndbe was running as user service, not system. So you need to use `--user` option:
```
systemctl --user stop sndbe
```

**How to turn on debug logs?**

Option 1. Directly in command line
```
systemctl stop <service_name>
<binary_name> -v:*.debug
```
Pay attention that you stop service but start binary. As example for sound backend:
```
systemctl stop sndbe
snd_be -v:*.debug
```
Option 2. Change service config

Add `-v:*.debug` to parameters in service config.

Second option is better because stop/start of backend may create some unexpected errors in other subsystems.

If you enable debugging for service, you can check logs using `dmesg`.

Also pay attention that some backends expect some command line parameters. For example for display backend you need to see https://github.com/xen-troops/displ_be#how-to-run
