#  disable

```


root@node2:~# apt-get install snapd
root@node2:~# systemctl start snapd
root@node2:~# systemctl status snapd
● snapd.service - Snap Daemon
   Loaded: loaded (/lib/systemd/system/snapd.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2021-05-25 05:18:53 PDT; 1s ago
 Main PID: 16265 (snapd)
    Tasks: 9 (limit: 2333)
   CGroup: /system.slice/snapd.service
           └─16265 /usr/lib/snapd/snapd

May 25 05:18:53 node2 systemd[1]: Starting Snap Daemon...
May 25 05:18:53 node2 snapd[16265]: AppArmor status: apparmor is enabled and all features are available
May 25 05:18:53 node2 snapd[16265]: patch.go:64: Patching system state level 6 to sublevel 1...
May 25 05:18:53 node2 snapd[16265]: patch.go:64: Patching system state level 6 to sublevel 2...
May 25 05:18:53 node2 snapd[16265]: patch.go:64: Patching system state level 6 to sublevel 3...
May 25 05:18:53 node2 snapd[16265]: daemon.go:347: started snapd/2.49.2+18.04 (series 16; classic) ubuntu/16.04 (amd64) linux/4.4.0-142-generic.
May 25 05:18:53 node2 snapd[16265]: daemon.go:440: adjusting startup timeout by 30s (pessimistic estimate of 30s plus 5s per snap)
May 25 05:18:53 node2 systemd[1]: Started Snap Daemon.


root@node2:~# systemctl list-units --type=service --state=running |grep snap
snapd.service               loaded active running Snap Daemon    


root@node2:~# systemctl stop snapd
Warning: Stopping snapd.service, but it can still be activated by:
  snapd.socket
root@node2:~# systemctl disable snapd
Removed /etc/systemd/system/multi-user.target.wants/snapd.service.





```
