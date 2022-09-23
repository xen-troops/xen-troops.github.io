# Start network (eth0) in dom0

At first you need to build product with four commits, starting with `[Dom0 net]` from https://github.com/andr2000/meta-xt-prod-devel/commits/dom0_netfront.

After successful build, and flash to board, login into dom0 and run two commands
```
xl network-attach Domain-0 backend=DomD
ifconfig eth0 192.168.0.7
```
