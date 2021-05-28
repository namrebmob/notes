# Detect execution in a virtualized environment

1. [virt-what](https://manpages.ubuntu.com/manpages/bionic/man1/virt-what.1.html)
2. Reading dmesg
```sh
sudo dmesg |grep -i virtual
```
3. Using `dmidecode`:
```sh
sudo dmidecode | egrep -i manuf
```
4. Listing `/dev/disk`
```sh
ls -l /dev/disk/by-id
```
5. Use `systemd-detect-virt`:
```sh
sudo systemd-detect-virt
```
6. SCSI
```sh
cat /proc/scsi/scsi
```
7. Using `hostnamectl`
```sh
hostnamectl status
```
