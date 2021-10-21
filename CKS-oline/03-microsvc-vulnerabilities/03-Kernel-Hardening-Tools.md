# Kernel Hardening Tools




```



root@master:~/cks/runtime-security# aa-status 
apparmor module is loaded.
6 profiles are loaded.
6 profiles are in enforce mode.
   /sbin/dhclient
   /usr/lib/NetworkManager/nm-dhcp-client.action
   /usr/lib/NetworkManager/nm-dhcp-helper
   /usr/lib/connman/scripts/dhclient-script
   /usr/sbin/tcpdump
   docker-default
0 profiles are in complain mode.
10 processes have profiles defined.
10 processes are in enforce mode.
   docker-default (26146) 
   docker-default (26164) 
   docker-default (26184) 
   docker-default (26480) 
   docker-default (27226) 
   docker-default (32926) 
   docker-default (47085) 
   docker-default (47820) 
   docker-default (47906) 
   docker-default (48662) 
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.


root@master:~/cks/runtime-security# apt-get install apparmor-utils


root@master:~/cks/runtime-security# aa-
aa-audit           aa-complain        aa-enabled         aa-genprof         aa-remove-unknown  aa-update-browser  
aa-autodep         aa-decode          aa-enforce         aa-logprof         aa-status          
aa-cleanprof       aa-disable         aa-exec            aa-mergeprof       aa-unconfined   


root@master:~/cks/runtime-security# aa-genprof curl  



root@master:~/cks/runtime-security# aa-status 
apparmor module is loaded.
7 profiles are loaded.
7 profiles are in enforce mode.
   /sbin/dhclient
   /usr/bin/curl
   /usr/lib/NetworkManager/nm-dhcp-client.action
   /usr/lib/NetworkManager/nm-dhcp-helper
   /usr/lib/connman/scripts/dhclient-script
   /usr/sbin/tcpdump
   docker-default


root@master:~/cks/runtime-security# cd /etc/apparmor.d/
root@master:/etc/apparmor.d# ls
abstractions  cache  disable  force-complain  local  sbin.dhclient  tunables  usr.bin.curl  usr.sbin.rsyslogd  usr.sbin.tcpdump
root@master:/etc/apparmor.d# cat usr.bin.curl 
# Last Modified: Mon May 24 23:11:35 2021
#include <tunables/global>

/usr/bin/curl {
  #include <abstractions/base>

  /usr/bin/curl mr,

}



root@master:/etc/apparmor.d# aa-logprof 
Reading log entries from /var/log/syslog.
Updating AppArmor profiles in /etc/apparmor.d.
Enforce-mode changes:

Profile:        /usr/bin/curl
Network Family: inet
Socket Type:    dgram

 [1 - #include <abstractions/nameservice>]
  2 - network inet dgram, 
(A)llow / [(D)eny] / (I)gnore / Audi(t) / Abo(r)t / (F)inish
Adding #include <abstractions/nameservice> to profile.

Profile:  /usr/bin/curl
Path:     /etc/ssl/openssl.cnf
Mode:     r
Severity: 2

  1 - #include <abstractions/openssl> 
  2 - #include <abstractions/ssl_keys> 
 [3 - /etc/ssl/openssl.cnf]
(A)llow / [(D)eny] / (I)gnore / (G)lob / Glob with (E)xtension / (N)ew / Abo(r)t / (F)inish / (M)ore
Adding /etc/ssl/openssl.cnf r to profile

= Changed Local Profiles =

The following local profiles were changed. Would you like to save them?

 [1 - /usr/bin/curl]
(S)ave Changes / Save Selec(t)ed Profile / [(V)iew Changes] / View Changes b/w (C)lean profiles / Abo(r)t
Writing updated profile for /usr/bin/curl.



root@master:/etc/apparmor.d# cat usr.bin.curl 
# Last Modified: Mon May 24 23:18:29 2021
#include <tunables/global>

/usr/bin/curl {
  #include <abstractions/base>
  #include <abstractions/nameservice>

  /etc/ssl/openssl.cnf r,
  /usr/bin/curl mr,

}



root@master:/etc/apparmor.d# curl killer.sh -v
* Rebuilt URL to: killer.sh/
*   Trying 35.227.196.29...
* TCP_NODELAY set
* Connected to killer.sh (35.227.196.29) port 80 (#0)
> GET / HTTP/1.1
> Host: killer.sh
> User-Agent: curl/7.58.0
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Cache-Control: private
< Content-Type: text/html; charset=UTF-8
< Referrer-Policy: no-referrer
< Location: https://killer.sh/
< Content-Length: 215
< Date: Tue, 25 May 2021 06:19:36 GMT
< 
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://killer.sh/">here</A>.
</BODY></HTML>
* Connection #0 to host killer.sh left intact




```
