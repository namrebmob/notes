# xfconf commandline utility

## Pointers on/off

```sh
# list pointers properties
xfconf-query --list --channel pointers
```

```sh
# disable device
xfconf-query \
    --channel pointers \
    -p /ETPS2_Elantech_Touchpad/Properties/Device_Enabled \
    -s 0
```

```sh
# enable device
xfconf-query \
    --channel pointers \
    -p /ETPS2_Elantech_Touchpad/Properties/Device_Enabled \
    -s 1
```
