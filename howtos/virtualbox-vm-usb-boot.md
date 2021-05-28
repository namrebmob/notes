# Boot Virtualbox VM from usb

```sh
VMDK_FILENAME=usb.vmdk
sudo vboxmanage internalcommands createrawvmdk \
    -filename ~/${VMDK_FILENAME}\
    -rawdisk /dev/sd[x]
sudo chown $USER:$USER ${VMDK_FILENAME}
sudo usermod -aG vboxusers disk $USER
```
