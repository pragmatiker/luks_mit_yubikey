Yubikey mit LUKS verheirate
```
systemd-cryptenroll /dev/nvme0n1p3 --fido2-device=auto --fido2-with-client-pin=yes
```

Uffmache bei Boot konifguriere
/etc/crypttab
```
nvme0n1p5_crypt UUID=406f3f48-xxx none luks,discard,fido2-device=auto
```

Initramsfs neu baue
```
apt install dracut
apt install fido2-tools
```
```
sudo tee /etc/dracut.conf.d/11-fido2.conf <<EOF
## Spaces in the quotes are critical.
# install_optional_items+=" /usr/lib/x86_64-linux-gnu/libfido2.so.* "

## Ugly workround because the line above doesn't fetch
## dependencies of libfido2.so
install_items+=" /usr/bin/fido2-token "

# Required detecting the fido2 key
install_items+=" /usr/lib/udev/rules.d/60-fido-id.rules /usr/lib/udev/fido_id "
EOF
```
```
dracut -f
```
