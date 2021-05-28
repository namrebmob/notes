# Detect graphics card installed in my system

Run

```sh
lspci | grep -i --color 'vga\|3d\|2d'
```

or using egrep

```sh
lspci -v | egrep -i --color 'vga|3d|2d'
```

Sample output:

`00:02.0 VGA compatible controller: Intel Corporation Haswell-ULT Integrated Graphics Controller (rev 09) (prog-if 00 [VGA controller])`

Note `00:02.0` then run

```sh
lspci -v -s 00:02.0
```
