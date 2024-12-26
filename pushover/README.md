# ssh

```
umask 066
echo "TOKEN=token" >> /etc/pushover
echo "USERKEY=userkey" >> /etc/pushover
echo "session optional pam_exec.so /usr/local/bin/alert ssh" >> /etc/pam.d/sshd
```
