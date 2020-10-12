# Tp3 yes de la commande bash

## 0 prérequis


-> go check vagrantfile (use utput.iso.repackage.lnk from tp2 because why not)

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

# I SS (services systemd)
## 1 intro

```bash
[vagrant@tp3 .ssh]$ systemctl list-unit-files
UNIT FILE                                     STATE
proc-sys-fs-binfmt_misc.automount             static
[...]
systemd-tmpfiles-clean.timer                  static

251 unit files listed.

```

there is 251 services systemd available in the machine

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

count the line where `running` appears and you got it. (btw it's 23)

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

one failed -> selinux because deactivated
16 exited, why ? because fuck'em that's why

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

32 enabled

## 2 analyse d'un service

nginx 

path
```bash
[vagrant@tp3 ~]$ which nginx
/usr/sbin/nginx
```

```bash
[vagrant@tp3 ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```

on voit que nginx est inactif, o mon saigneuhre que fèr ? pas de panique jeune double de moi que je joue avec des lignes pleine de fautes d'orthographe pour que tu puisses faire la différence. il suffit de lancer la commande `sudo systemctl start nginx`.

then,

```bash
[vagrant@tp3 ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-10-09 07:25:50 UTC; 10s ago
  Process: 2337 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 2335 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 2333 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 2339 (nginx)
   CGroup: /system.slice/nginx.service
           ├─2339 nginx: master process /usr/sbin/nginx
           └─2340 nginx: worker process
```

on remarque que les lignes `Process: ` donne aussi le path du service.

contenu

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