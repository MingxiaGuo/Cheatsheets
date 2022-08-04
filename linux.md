# Linux

#
```shell
cd 
ls
wget
mkdir upper lower merged work # 同时创建四个文件 upper lower merged work
scp
ssh
ps -ef
```

# systemctl 
systemctl may be used to introspect and control the state of the "systemd" system and service manager.

systemctl cheatsheet: https://access.redhat.com/sites/default/files/attachments/12052018_systemd_6.pdf
```bash
systemctl status service   # See if service is running/enabled
systemctl stop service     # Stop a running service
systemctl start service    # Start a service
systemctl restart service  # Restart a running service
systemctl enable service   # Enable a service to start on boot
systemctl disable service  # Disable service--won’t start at boot
```
# journalctl
VIEWING LOG MESSAGES
```bash
journalctl Show all collected log messages
journalctl -u network.service See network service messages
journalctl -f Follow messages as they appear
journalctl -k Show only kernel messages
```

## Change or Set Password
```bash
root@test-gmx-stunnel:~# passwd
New password:
Retype new password:
passwd: password updated successfully
root@test-gmx-stunnel:~#
```


## Vi
### 快速移动光标

移动光标至行首：shift + 6；Home键=Fn+左方向

移动光标至行尾: shift + 4；End键=Fn+右方向

向前移动一个单词:

向后移动一个单词
