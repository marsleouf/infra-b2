# Tp3 yes de la commande bash

## 0 - Prérequis


-> go check vagrantfile  
![](../images/doit.gif)  
(use utput.iso.repackage.lnk from tp2 because why not)


```bash
[vagrant@tp3 .ssh]$ sudo systemctl list-unit-files -t service
UNIT FILE                                     STATE
auditd.service                                enabled
auth-rpcgss-module.service                    static
autovt@.service                               enabled
blk-availability.service                      disabled
[...]
vgauthd.service                               enabled
vmtoolsd-init.service                         enabled
vmtoolsd.service                              enabled
wpa_supplicant.service                        disabled

155 unit files listed.
```

```bash
[vagrant@tp3 .ssh]$ sudo systemctl list-unit-files -t service -a
UNIT FILE                                     STATE
auditd.service                                enabled
auth-rpcgss-module.service                    static
autovt@.service                               enabled
blk-availability.service                      disabled
[...]
vgauthd.service                               enabled
vmtoolsd-init.service                         enabled
vmtoolsd.service                              enabled
wpa_supplicant.service                        disabled

155 unit files listed.
```

# I - SS (services systemd)
## 1 - Intro

```bash
[vagrant@tp3 .ssh]$ systemctl list-unit-files
UNIT FILE                                     STATE
proc-sys-fs-binfmt_misc.automount             static
[...]
systemd-tmpfiles-clean.timer                  static

251 unit files listed.

```

There is 251 services systemd available in the machine

![](../images/moyen.jpg)

```bash
[vagrant@tp3 .ssh]$ systemctl | grep running
  session-2.scope                                                                          loaded active running   Session 2 of user vagrant
  auditd.service                                                                           loaded active running   Security Auditing Service
  chronyd.service                                                                          loaded active running   NTP client/server
  crond.service                                                                            loaded active running   Command Scheduler
  dbus.service                                                                             loaded active running   D-Bus System Message Bus
  firewalld.service                                                                        loaded active running   firewalld - dynamic firewall daemon
  getty@tty1.service                                                                       loaded active running   Getty on tty1
  gssproxy.service                                                                         loaded active running   GSSAPI Proxy Daemon
  NetworkManager.service                                                                   loaded active running   Network Manager
  polkit.service                                                                           loaded active running   Authorization Manager
  postfix.service                                                                          loaded active running   Postfix Mail Transport Agent
  rpcbind.service                                                                          loaded active running   RPC bind service
  rsyslog.service                                                                          loaded active running   System Logging Service
  sshd.service                                                                             loaded active running   OpenSSH server daemon
  systemd-journald.service                                                                 loaded active running   Journal Service
  systemd-logind.service                                                                   loaded active running   Login Service
  systemd-udevd.service                                                                    loaded active running   udev Kernel Device Manager
  tuned.service                                                                            loaded active running   Dynamic System Tuning Daemon
  dbus.socket                                                                              loaded active running   D-Bus System Message Bus Socket
  rpcbind.socket                                                                           loaded active running   RPCbind Server Activation Socket
  systemd-journald.socket                                                                  loaded active running   Journal Socket
  systemd-udevd-control.socket                                                             loaded active running   udev Control Socket
  systemd-udevd-kernel.socket                                                              loaded active running   udev Kernel Socket
```

Count the line where `running` appears and you got it. (btw it's 23)

![](../images/moyen.jpg)

```bash
[vagrant@tp3 .ssh]$ systemctl | grep failed
● selinux-policy-migrate-local-changes@targeted.service                                    loaded failed failed    Migrate local SELinux policy changes from the old store structure to the new structure
[vagrant@tp3 .ssh]$ systemctl | grep exited
  kmod-static-nodes.service                                                                loaded active exited    Create list of required static device nodes for the current kernel
  network.service                                                                          loaded active exited    LSB: Bring up/down networking
  NetworkManager-wait-online.service                                                       loaded active exited    Network Manager Wait Online
  rhel-dmesg.service                                                                       loaded active exited    Dump dmesg to /var/log/dmesg
  rhel-domainname.service                                                                  loaded active exited    Read and set NIS domainname from /etc/sysconfig/network
  rhel-readonly.service                                                                    loaded active exited    Configure read-only root support
  systemd-journal-flush.service                                                            loaded active exited    Flush Journal to Persistent Storage
  systemd-random-seed.service                                                              loaded active exited    Load/Save Random Seed
  systemd-remount-fs.service                                                               loaded active exited    Remount Root and Kernel File Systems
  systemd-sysctl.service                                                                   loaded active exited    Apply Kernel Variables
  systemd-tmpfiles-setup-dev.service                                                       loaded active exited    Create Static Device Nodes in /dev
  systemd-tmpfiles-setup.service                                                           loaded active exited    Create Volatile Files and Directories
  systemd-udev-trigger.service                                                             loaded active exited    udev Coldplug all Devices
  systemd-update-utmp.service                                                              loaded active exited    Update UTMP about System Boot/Shutdown
  systemd-user-sessions.service                                                            loaded active exited    Permit User Sessions
  systemd-vconsole-setup.service                                                           loaded active exited    Setup Virtual Console
```

One failed -> selinux because deactivated  
16 exited, why ? 

![](../images/why.jpg)

```bash
[vagrant@tp3 .ssh]$ systemctl list-unit-files -t service | grep enabled
auditd.service                                enabled
autovt@.service                               enabled
chronyd.service                               enabled
crond.service                                 enabled
dbus-org.fedoraproject.FirewallD1.service     enabled
dbus-org.freedesktop.nm-dispatcher.service    enabled
firewalld.service                             enabled
getty@.service                                enabled
irqbalance.service                            enabled
NetworkManager-dispatcher.service             enabled
NetworkManager-wait-online.service            enabled
NetworkManager.service                        enabled
postfix.service                               enabled
qemu-guest-agent.service                      enabled
rhel-autorelabel-mark.service                 enabled
rhel-autorelabel.service                      enabled
rhel-configure.service                        enabled
rhel-dmesg.service                            enabled
rhel-domainname.service                       enabled
rhel-import-state.service                     enabled
rhel-loadmodules.service                      enabled
rhel-readonly.service                         enabled
rpcbind.service                               enabled
rsyslog.service                               enabled
sshd.service                                  enabled
systemd-readahead-collect.service             enabled
systemd-readahead-drop.service                enabled
systemd-readahead-replay.service              enabled
tuned.service                                 enabled
vgauthd.service                               enabled
vmtoolsd-init.service                         enabled
vmtoolsd.service                              enabled
```

There is 32 enabled. No comment

![](../images/transition.JPG)

## 2 - Analyse d'un service

### NGINX

On va aller très vite pour tout ça, faudra que tu survoles le tout, les réponses tu les connaîs de toutes façons. 
Alors son path c'est:

```bash
[vagrant@tp3 ~]$ which nginx
/usr/sbin/nginx
```

On check son status pour obtenir d'autres infos :

```bash
[vagrant@tp3 ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```

On voit que nginx est inactif, *o mon saigneuhre que fèr ?* Pas de panique jeune double de moi que je joue avec des lignes pleine de fautes d'orthographe en italique pour que tu puisses faire la différence. Il suffit de lancer la commande `sudo systemctl start nginx`.

Then,

```bash
[vagrant@tp3 ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-10-09 07:25:50 UTC; 10s ago
  Process: 2337 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 2335 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 2333 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
  Process: 4209 ExecSuceUnQ=/usr/troll/anyway /run/gottagofast.pid (code=code, status=69/TES_NAZE)
 Main PID: 2339 (nginx)
   CGroup: /system.slice/nginx.service
           ├─2339 nginx: master process /usr/sbin/nginx
           └─2340 nginx: worker process
```

On remarque que les lignes `Process: ` donne aussi le path du service, point barre, on avance.

![](../images/transition.JPG)

Bon la suite:

```bash
[vagrant@tp3 ~]$ systemctl cat nginx
# /usr/lib/systemd/system/nginx.service
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
# Nginx will fail to start if /run/nginx.pid already exists but has the wrong
# SELinux context. This might happen when running `nginx -t` from the cmdline.
# https://bugzilla.redhat.com/show_bug.cgi?id=1268621
ExecStartPre=/usr/bin/rm -f /run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/bin/kill -s HUP $MAINPID
KillSignal=SIGQUIT
TimeoutStopSec=5
KillMode=process
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

> - `ExecStart` = chemin d'accès de l'app à lancer pour mettre en route le système attribué (ici nginx)
> - `ExecStartPre` = commandes éxécutées avant ou après `ExecStart` (ça dépend)
> - `PIDFile` = indique le chemin d'accès du `pidfile`, ici sous /run/nginx.pid
> - `Type` = c'est un "type" (bien vu l'artiste) de démarrage possible du service parmis tant d'autres (ex: simple, exec, forking...)
> - `ExecReload` = commande à éxécuter pour `Matrix : reloaded` le système, envoyé via un signal (parce que kill est utilisé dans la commande)
>
>   - `Description` = `¯\_(ツ)_/¯ ça apparait pas`
>   - `After` = `¯\_(ツ)_/¯ (ça non plus)`

Bon... pour lister tout les services qui contiennent la ligne `WantedBy=multi-user.target`, rien de plus compliqué qu'utiliser un bon gros vieux `grep -r "WantedBy=multi-user.target"` à la racine du pc (sauf que non tu vas voir, c'est pas si simple que ça).

```bash
[vagrant@tp3 ~]$ grep -r "WantedBy=multi-user.target" /
[...]
grep: /proc/131/task/131/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/131/task/131/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/131/task/131/net/nf_conntrack: Permission denied
grep: /proc/131/task/131/net/ip_tables_names: Permission denied
grep: /proc/131/task/131/net/ip6_tables_names: Permission denied
grep: /proc/131/task/131/net/ip_tables_matches: Permission denied
grep: /proc/131/task/131/net/ip_tables_targets: Permission denied
grep: /proc/131/task/131/net/ip6_tables_matches: Permission denied
grep: /proc/131/task/131/net/ip6_tables_targets: Permission denied
grep: /proc/131/task/131/net/nf_conntrack_expect: Permission denied
[...]
[...]
[...]
[...]
# (arrête de grogner je t'épargne 5000 ligne de permission denied)
[...]
[...]
[...]
grep: /proc/2282/task/2282/mem: Input/output error
grep: /proc/2282/task/2282/clear_refs: Permission denied
^C
```

So many `Permission denied`. But what if I do it with `sudo` ?

```bash
[vagrant@tp3 ~[vagrant@tp3 ~]$ sudo grep -r "WantedBy=multi-user.target" /
grep: /proc/sys/fs/binfmt_misc/register: Invalid argument
grep: /proc/sys/net/ipv4/route/flush: Permission denied
grep: /proc/sys/net/ipv6/conf/all/stable_secret: Input/output error
grep: /proc/sys/net/ipv6/conf/default/stable_secret: Input/output error
grep: /proc/sys/net/ipv6/conf/eth0/stable_secret: Input/output error
grep: /proc/sys/net/ipv6/conf/eth1/stable_secret: Input/output error
grep: /proc/sys/net/ipv6/conf/lo/stable_secret: Input/output error
grep: /proc/sys/net/ipv6/route/flush: Permission denied
grep: /proc/sys/vm/compact_memory: Permission denied
^C
```

![](../images/kratospc.gif)

It didn't change anything unless now, I know that I have a shitty CLI.

![](../images/anyway.jpg)

Let's try another one: 

```bash
[vagrant@tp3 ~]$ sudo grep -r "WantedBy=multi-user.target" /run/systemd/transient/* /etc/systemd/system/* /run/systemd/generator/* /usr/lib/systemd/system/*
grep: /run/systemd/transient/*: No such file or directory
/etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service:WantedBy=multi-user.target
/usr/lib/systemd/system/auditd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/brandbot.path:WantedBy=multi-user.target
/usr/lib/systemd/system/chronyd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/chrony-wait.service:WantedBy=multi-user.target
/usr/lib/systemd/system/cpupower.service:WantedBy=multi-user.target
/usr/lib/systemd/system/crond.service:WantedBy=multi-user.target
/usr/lib/systemd/system/ebtables.service:WantedBy=multi-user.target
/usr/lib/systemd/system/firewalld.service:WantedBy=multi-user.target
/usr/lib/systemd/system/fstrim.timer:WantedBy=multi-user.target
/usr/lib/systemd/system/gssproxy.service:WantedBy=multi-user.target
/usr/lib/systemd/system/irqbalance.service:WantedBy=multi-user.target
/usr/lib/systemd/system/machines.target:WantedBy=multi-user.target
/usr/lib/systemd/system/NetworkManager.service:WantedBy=multi-user.target
/usr/lib/systemd/system/nfs-client.target:WantedBy=multi-user.target
/usr/lib/systemd/system/nfs-rquotad.service:WantedBy=multi-user.target
/usr/lib/systemd/system/nfs-server.service:WantedBy=multi-user.target
/usr/lib/systemd/system/nfs.service:WantedBy=multi-user.target
/usr/lib/systemd/system/nginx.service:WantedBy=multi-user.target
/usr/lib/systemd/system/postfix.service:WantedBy=multi-user.target
/usr/lib/systemd/system/rdisc.service:WantedBy=multi-user.target
/usr/lib/systemd/system/remote-cryptsetup.target:WantedBy=multi-user.target
/usr/lib/systemd/system/remote-fs.target:WantedBy=multi-user.target
/usr/lib/systemd/system/rhel-configure.service:WantedBy=multi-user.target
/usr/lib/systemd/system/rpcbind.service:WantedBy=multi-user.target
/usr/lib/systemd/system/rpc-rquotad.service:WantedBy=multi-user.target
/usr/lib/systemd/system/rsyncd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/rsyslog.service:WantedBy=multi-user.target
/usr/lib/systemd/system/sshd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/tcsd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/tuned.service:WantedBy=multi-user.target
/usr/lib/systemd/system/vmtoolsd.service:WantedBy=multi-user.target
/usr/lib/systemd/system/wpa_supplicant.service:WantedBy=multi-user.target
```

(cimer le bro qui m'a aidé pour les arguments de commande (ps si tu veux savoir qui cé, il est proche d'un film d'animation avec un roi lémurien))

## 3 - Création d'un servcie
### A -  Serveur web

(merci pour le coup de main comrade)

![](../images/soviet1.png)

```bash
[vagrant@tp3 system]$ python2 -m SimpleHTTPServer 8080
Serving HTTP on 0.0.0.0 port 8080 ...
^CTraceback (most recent call last):
  File "/usr/lib64/python2.7/runpy.py", line 162, in _run_module_as_main
    "__main__", fname, loader, pkg_name)
  File "/usr/lib64/python2.7/runpy.py", line 72, in _run_code
    exec code in run_globals
  File "/usr/lib64/python2.7/SimpleHTTPServer.py", line 220, in <module>
    test()
  File "/usr/lib64/python2.7/SimpleHTTPServer.py", line 216, in test
    BaseHTTPServer.test(HandlerClass, ServerClass)
  File "/usr/lib64/python2.7/BaseHTTPServer.py", line 599, in test
    httpd.serve_forever()
  File "/usr/lib64/python2.7/SocketServer.py", line 236, in serve_forever
    poll_interval)
  File "/usr/lib64/python2.7/SocketServer.py", line 155, in _eintr_retry
    return func(*args)
KeyboardInterrupt


[vagrant@tp3 system]$ firewall-cmd --add-port=8080/tcp

Authorization failed.
    Make sure polkit agent is running or run the application as superuser.


[vagrant@tp3 system]$ sudo !!
sudo firewall-cmd --add-port=8080/tcp

success
[vagrant@tp3 system]$
[vagrant@tp3 system]$ sudo firewall-cmd --add-port=8080/tcp --permanent
^[[A^[[A^[[Asuccess
^[[A[vagrant@tp3 systepython2 -m SimpleHTTPServer 8080
Serving HTTP on 0.0.0.0 port 8080 ...
10.2.1.1 - - [09/Oct/2020 17:07:48] "GET / HTTP/1.1" 200 -
10.2.1.1 - - [09/Oct/2020 17:07:48] "GET / HTTP/1.1" 200 -
10.2.1.1 - - [09/Oct/2020 17:07:49] code 404, message File not found
10.2.1.1 - - [09/Oct/2020 17:07:49] "GET /favicon.ico HTTP/1.1" 404 -
```

And now, for my next number, i would like to return to the classics:

```bash
[vagrant@tp3 system]$ cat pyserveur.service
[Unit]
Description=Un serveur python ?
[Service]
Type=simple
User=claquettechaussette

Environment="PORT=8080"
ExecStartPre=/usr/bin/sudo /usr/bin/firewall-cmd --add-port=${PORT}/tcp
ExecStartPre=/usr/bin/sudo /usr/bin/firewall-cmd --reload
ExecStart=/usr/bin/sudo /usr/bin/python2 -m SimpleHTTPServer ${PORT}
ExecStopPost=/usr/bin/sudo /usr/bin/firewall-cmd --remove-port=${PORT}/tcp
TimeoutSec=15000


[Install]
WantedBy=multi-user.target
```

N'en dis pas plus je sais que tu te souviens de cette fameuse péripétie de `lou clâquettechôsette`, tu vois très bien ce qu'il s'est passé, j'en déduis que par le pouvoir des Niquelalogiquenciens je peux skip les [explications](./explications.md).

Allé tchâo.



En soit ca fonctionne et ça marche, quoi de mieux ?
```bash
[vagrant@tp3 ~]$ sudo systemctl status pyserveur.service
● pyserveur.service - Un serveur python ?
   Loaded: loaded (/etc/systemd/system/pyserveur.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2020-10-14 07:52:02 UTC; 6s ago
  Process: 2337 ExecStartPre=/usr/bin/sudo /usr/bin/firewall-cmd --reload (code=exited, status=0/SUCCESS)
  Process: 2332 ExecStartPre=/usr/bin/sudo /usr/bin/firewall-cmd --add-port=${PORT}/tcp (code=exited, status=0/SUCCESS)
 Main PID: 2368 (sudo)
   CGroup: /system.slice/pyserveur.service
           ‣ 2368 /usr/bin/sudo /usr/bin/python2 -m SimpleHTTPServer 8080

Oct 14 07:51:59 tp3 systemd[1]: Starting Un serveur python ?...
Oct 14 07:51:59 tp3 sudo[2332]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd --add...8080/tcp
Oct 14 07:52:01 tp3 sudo[2337]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd --reload
Oct 14 07:52:02 tp3 systemd[1]: Started Un serveur python ?.
Oct 14 07:52:02 tp3 sudo[2368]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/python2 -m SimpleH...ver 8080
Hint: Some lines were ellipsized, use -l to show in full.
```

### B -Sauvegarde

Le script = ton script sans les `-r` après les `declare`.  
Here le `.service`:

```sh
[Unit]
Description=Le script de sauvegarde qui fonctionne pas depuis 2 tps

[Service]
ExecStart=/home/claquettechaussette/backup.sh /tmp
User=claquettechaussette
Group=wheel
PIDFile=/var/run/backup/pid
Environment=PATH=/usr/bin:/usr/local/bin

[Install]
WantedBy=multi-user.target
```

Here son status une fois lancé:
```bash
[claquettechaussette@tp3 /]$ sudo systemctl status backup.service
● backup.service - Le script de sauvegarde qui fonctionne pas depuis 2 tps
   Loaded: loaded (/etc/systemd/system/backup.service; disabled; vendor preset: disabled)
   Active: inactive (dead)

Oct 14 15:15:33 tp3 systemd[1]: backup.service: main process exited, code=exited, status=1/FAILURE
Oct 14 15:15:33 tp3 systemd[1]: Unit backup.service entered failed state.
Oct 14 15:15:33 tp3 systemd[1]: backup.service failed.
Oct 14 15:16:05 tp3 systemd[1]: Started Le script de sauvegarde qui fonctionne pas depuis 2 tps.
Oct 14 15:16:05 tp3 systemd[1]: backup.service: main process exited, code=exited, status=1/FAILURE
Oct 14 15:16:05 tp3 systemd[1]: Unit backup.service entered failed state.
Oct 14 15:16:05 tp3 systemd[1]: backup.service failed.
Oct 14 15:17:14 tp3 systemd[1]: Started Le script de sauvegarde qui fonctionne pas depuis 2 tps.
Oct 14 15:17:14 tp3 backup.sh[3663]: [Oct 14 15:17:14] [INFO] Starting backup.
Oct 14 15:17:14 tp3 backup.sh[3663]: [Oct 14 15:17:14] [INFO] Success. Backup tmp_201014_151714.tar.gz has been saved to /opt/backup/.
Hint: Some lines were ellipsized, use -l to show in full.
```

Donc, dans les premières lignes, on remarque que le service a failed et dans les deux dernières, on est infromé d'un probable `success` du script (???). La sauvegarde serait dans `/opt/backup/` eh bien let's go checker ça:

```bash
[claquettechaussette@tp3 /]$ ls /opt/backup/
tmp_201014_143947.tar.gz  tmp_201014_151245.tar.gz  tmp_201014_151714.tar.gz
```

...  
Je comprends plus rien...

Et maintenant le timer. Il se présente sous cette forme:

```sh
[Unit]
Description=Execute backup every day at midnight

[Timer]
OnCalendar=*-*-* *:00:00
Unit=backup.service

[Install]
WantedBy=multi-user.target
```

Mais attend c'est pas le plus fou dans tout ça, regarde: 

```bash
[claquettechaussette@tp3 system]$ sudo systemctl enable backup.timer
Created symlink from /etc/systemd/system/multi-user.target.wants/backup.timer to /usr/lib/systemd/system/backup.timer.
[claquettechaussette@tp3 system]$ systemctl start backup.timer*
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: claquettechaussette
Password:
==== AUTHENTICATION COMPLETE ===
[claquettechaussette@tp3 system]$ systemctl is-enabled backup.timer
enabled
[claquettechaussette@tp3 system]$ sudo systemctl start backup
[claquettechaussette@tp3 system]$ systemctl status backup
● backup.service - Le script de sauvegarde qui fonctionne pas depuis 2 tps
   Loaded: loaded (/etc/systemd/system/backup.service; disabled; vendor preset: disabled)
   Active: inactive (dead) since Wed 2020-10-14 15:53:48 UTC; 20s ago
  Process: 3767 ExecStart=/home/claquettechaussette/backup.sh /tmp (code=exited, status=0/SUCCESS)
 Main PID: 3767 (code=exited, status=0/SUCCESS)

Oct 14 15:53:48 tp3 systemd[1]: Started Le script de sauvegarde qui fonctionne pas depuis 2 tps.
Oct 14 15:53:48 tp3 backup.sh[3767]: [Oct 14 15:53:48] [INFO] Starting backup.
Oct 14 15:53:48 tp3 backup.sh[3767]: [Oct 14 15:53:48] [INFO] Success. Backup tmp_201014_155348.tar.gz has been saved to /opt/backup/.
Hint: Some lines were ellipsized, use -l to show in full.
```

Désormais, le backup se lance sans grogner ou retourner d'erreur. Comme quoi une fois qu'on lui a attribué une horloge il sait quoi faire. Tu la vois la métaphore ou pas ? Eh question bonus: sans temporalité, sans secondes, minutes, heures, jours, mois, année... On est quoi au final ?

## II - [Develloppers feat. Godefroy](https://www.youtube.com/watch?v=KMU0tzLwhbE)
### 1 - Gestion de boot

![](../images/mcgonagal.png)

Ensuite fais un `ctrl + f` et tape ".service" dans la barre de recherche pour trouver tout les services présent sur le diagramme. Puis à toi de chercher les plus long à démarrer.  
La légende raconte que les 3 grosses feignasses du groupe sont:
> - systemd-logind.service (4.494s)
> - chronyd.service (4.532s)
> - polkit.service (4.580s)

Voilà !

### 2 - Gestion de l'heure ? (genre... sérieusement ?)

C'est que t'as l'air sérieux en plus...
```bash
[vagrant@tp3 /]$ timedatectl
      Local time: Wed 2020-10-14 17:54:54 UTC
  Universal time: Wed 2020-10-14 17:54:54 UTC
        RTC time: Wed 2020-10-14 17:19:34
       Time zone: UTC (UTC, +0000)
     NTP enabled: yes
NTP synchronized: no
 RTC in local TZ: no
      DST active: n/a
```

Alors, sachant que Migwel possède trois pommes, que son ami Tuyo lui, a une armée de chenilles, et que le sergent Chesterfield et le caporal Blutch sont en plein voyage à Versailles, selon le théorème de puthagore, nous sommes parrallèlle à l'équateur lunaire lui même alignés avec les astres dimensionnels. Je rajoute 6, je décale le 7, je retiens 32, je te nique le 8, j'ajoute la TVA, les împots et le fisc. Je peux donc en conclure que mon fuseau horraire est le même que celui du méridien de Greenwich a svoir le point de cordonnées (0;0;0), soit: `(UTC, +0000)`.

![](../images/iq.jpg)

En reprenant le calcul d'avant, on y ajoute la clause des liaisons de Van der Waals afin de déduire que nous ne sommes pas synchronisé à un serveur NTP (sah quel dommage).

Donc après avoir fait un `timedatectl list-timezones`, j'ai chois de me poser sur le fuseau horraire de Nairobi, parce que why not.  
So let's do this: `sudo timedatectl set-timezone Africa/Nairobi`, et paf! ça fait des chocapics.

### 3 - Change pseudo but forgot password

Non mais là tu nous prends pour des grosses merde, là c'est non Léo, je peux pas travailler dasn des conditions pareilles, c'est négatif.

![](../images/mah.jpg)

```bash
[vagrant@tp3 /]$ hostname
hostname
tp3
```

Et si je veux le changer:

```bash
[vagrant@tp3 /]$ cat /etc/hostname
tp3
[vagrant@tp3 /]$ sudo nano /etc/hostname
[vagrant@tp3 /]$ cat /etc/hostname
WIN7 > Linux > it4
```