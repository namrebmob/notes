# How to create the hashed password

```sh
printf "thisismypassword" | sha256sum | cut -d' ' -f1
```
