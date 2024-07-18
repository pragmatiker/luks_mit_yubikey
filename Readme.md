# Switch from initramfs-tools to Dracut to manage initrd
```
apt install dracut fido2-tools
apt purge cryptsetup-initramfs && apt autoremove -purge
echo "hostonly=yes" > /etc/dracut.conf.d/10-hostonly.conf
dracut -f
# Test that the switch to dracut is working by rebooting 😀
reboot
```


# Edit /etc/crypttab
```
# Change the line:
# vda5_crypt UUID=165e9c6c-6277-49b1-ac51-94158b504964 none luks,discard
# to:
# vda5_crypt UUID=165e9c6c-6277-49b1-ac51-94158b504964 none luks,discard,fido2-device=auto
```

# Update initrd to include FIDO2 tools and updated /etc/crypttab
```
dracut -f
```


# Check your FIDO2 device is listed
```
systemd-cryptenroll -fido2-device=list
```

# Enroll your FIDO2 device to unlock Luks volume
systemd-cryptenroll -fido2-device=auto /dev/vda5
# Test if everything works as expected ! 😀
```
reboot
```
