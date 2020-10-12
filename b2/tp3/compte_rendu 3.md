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

Bon... pour lister tout les services qui contiennent la ligne `WantedBy=multi-user.target`, rien de plus compliqué qu'utiliser un bon gros vieux `grep -r "WantedBy=multi-user.target"` à la racine du pc (sauf que non tu vas voir c'est pas si simple que ça).

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
grep: /proc/131/task/131/environ: Permission denied
grep: /proc/131/task/131/auxv: Permission denied
grep: /proc/131/task/131/personality: Operation not permitted
grep: /proc/131/task/131/syscall: Operation not permitted
grep: /proc/131/task/131/mem: Permission denied
grep: /proc/131/task/131/clear_refs: Permission denied
grep: /proc/131/task/131/stack: Permission denied
grep: /proc/131/task/131/io: Permission denied
grep: /proc/131/task/131/patch_state: Permission denied
grep: /proc/131/fd: Permission denied
grep: /proc/131/map_files: Permission denied
grep: /proc/131/fdinfo: Permission denied
grep: /proc/131/ns: Permission denied
grep: /proc/131/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/131/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/131/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/131/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/131/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/131/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/131/net/nf_conntrack: Permission denied
grep: /proc/131/net/ip_tables_names: Permission denied
grep: /proc/131/net/ip6_tables_names: Permission denied
grep: /proc/131/net/ip_tables_matches: Permission denied
grep: /proc/131/net/ip_tables_targets: Permission denied
grep: /proc/131/net/ip6_tables_matches: Permission denied
grep: /proc/131/net/ip6_tables_targets: Permission denied
grep: /proc/131/net/nf_conntrack_expect: Permission denied
grep: /proc/131/environ: Permission denied
grep: /proc/131/auxv: Permission denied
grep: /proc/131/personality: Operation not permitted
grep: /proc/131/syscall: Operation not permitted
grep: /proc/131/mem: Permission denied
grep: /proc/131/mountstats: Permission denied
grep: /proc/131/clear_refs: Permission denied
grep: /proc/131/stack: Permission denied
grep: /proc/131/io: Permission denied
grep: /proc/131/patch_state: Permission denied
grep: /proc/136/task/136/fd: Permission denied
grep: /proc/136/task/136/fdinfo: Permission denied
grep: /proc/136/task/136/ns: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/136/task/136/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/136/task/136/net/nf_conntrack: Permission denied
grep: /proc/136/task/136/net/ip_tables_names: Permission denied
grep: /proc/136/task/136/net/ip6_tables_names: Permission denied
grep: /proc/136/task/136/net/ip_tables_matches: Permission denied
grep: /proc/136/task/136/net/ip_tables_targets: Permission denied
grep: /proc/136/task/136/net/ip6_tables_matches: Permission denied
grep: /proc/136/task/136/net/ip6_tables_targets: Permission denied
grep: /proc/136/task/136/net/nf_conntrack_expect: Permission denied
grep: /proc/136/task/136/environ: Permission denied
grep: /proc/136/task/136/auxv: Permission denied
grep: /proc/136/task/136/personality: Operation not permitted
grep: /proc/136/task/136/syscall: Operation not permitted
grep: /proc/136/task/136/mem: Permission denied
grep: /proc/136/task/136/clear_refs: Permission denied
grep: /proc/136/task/136/stack: Permission denied
grep: /proc/136/task/136/io: Permission denied
grep: /proc/136/task/136/patch_state: Permission denied
grep: /proc/136/fd: Permission denied
grep: /proc/136/map_files: Permission denied
grep: /proc/136/fdinfo: Permission denied
grep: /proc/136/ns: Permission denied
grep: /proc/136/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/136/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/136/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/136/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/136/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/136/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/136/net/nf_conntrack: Permission denied
grep: /proc/136/net/ip_tables_names: Permission denied
grep: /proc/136/net/ip6_tables_names: Permission denied
grep: /proc/136/net/ip_tables_matches: Permission denied
grep: /proc/136/net/ip_tables_targets: Permission denied
grep: /proc/136/net/ip6_tables_matches: Permission denied
grep: /proc/136/net/ip6_tables_targets: Permission denied
grep: /proc/136/net/nf_conntrack_expect: Permission denied
grep: /proc/136/environ: Permission denied
grep: /proc/136/auxv: Permission denied
grep: /proc/136/personality: Operation not permitted
grep: /proc/136/syscall: Operation not permitted
grep: /proc/136/mem: Permission denied
grep: /proc/136/mountstats: Permission denied
grep: /proc/136/clear_refs: Permission denied
grep: /proc/136/stack: Permission denied
grep: /proc/136/io: Permission denied
grep: /proc/136/patch_state: Permission denied
grep: /proc/138/task/138/fd: Permission denied
grep: /proc/138/task/138/fdinfo: Permission denied
grep: /proc/138/task/138/ns: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/138/task/138/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/138/task/138/net/nf_conntrack: Permission denied
grep: /proc/138/task/138/net/ip_tables_names: Permission denied
grep: /proc/138/task/138/net/ip6_tables_names: Permission denied
grep: /proc/138/task/138/net/ip_tables_matches: Permission denied
grep: /proc/138/task/138/net/ip_tables_targets: Permission denied
grep: /proc/138/task/138/net/ip6_tables_matches: Permission denied
grep: /proc/138/task/138/net/ip6_tables_targets: Permission denied
grep: /proc/138/task/138/net/nf_conntrack_expect: Permission denied
grep: /proc/138/task/138/environ: Permission denied
grep: /proc/138/task/138/auxv: Permission denied
grep: /proc/138/task/138/personality: Operation not permitted
grep: /proc/138/task/138/syscall: Operation not permitted
grep: /proc/138/task/138/mem: Permission denied
grep: /proc/138/task/138/clear_refs: Permission denied
grep: /proc/138/task/138/stack: Permission denied
grep: /proc/138/task/138/io: Permission denied
grep: /proc/138/task/138/patch_state: Permission denied
grep: /proc/138/fd: Permission denied
grep: /proc/138/map_files: Permission denied
grep: /proc/138/fdinfo: Permission denied
grep: /proc/138/ns: Permission denied
grep: /proc/138/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/138/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/138/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/138/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/138/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/138/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/138/net/nf_conntrack: Permission denied
grep: /proc/138/net/ip_tables_names: Permission denied
grep: /proc/138/net/ip6_tables_names: Permission denied
grep: /proc/138/net/ip_tables_matches: Permission denied
grep: /proc/138/net/ip_tables_targets: Permission denied
grep: /proc/138/net/ip6_tables_matches: Permission denied
grep: /proc/138/net/ip6_tables_targets: Permission denied
grep: /proc/138/net/nf_conntrack_expect: Permission denied
grep: /proc/138/environ: Permission denied
grep: /proc/138/auxv: Permission denied
grep: /proc/138/personality: Operation not permitted
grep: /proc/138/syscall: Operation not permitted
grep: /proc/138/mem: Permission denied
grep: /proc/138/mountstats: Permission denied
grep: /proc/138/clear_refs: Permission denied
grep: /proc/138/stack: Permission denied
grep: /proc/138/io: Permission denied
grep: /proc/138/patch_state: Permission denied
grep: /proc/139/task/139/fd: Permission denied
grep: /proc/139/task/139/fdinfo: Permission denied
grep: /proc/139/task/139/ns: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/139/task/139/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/139/task/139/net/nf_conntrack: Permission denied
grep: /proc/139/task/139/net/ip_tables_names: Permission denied
grep: /proc/139/task/139/net/ip6_tables_names: Permission denied
grep: /proc/139/task/139/net/ip_tables_matches: Permission denied
grep: /proc/139/task/139/net/ip_tables_targets: Permission denied
grep: /proc/139/task/139/net/ip6_tables_matches: Permission denied
grep: /proc/139/task/139/net/ip6_tables_targets: Permission denied
grep: /proc/139/task/139/net/nf_conntrack_expect: Permission denied
grep: /proc/139/task/139/environ: Permission denied
grep: /proc/139/task/139/auxv: Permission denied
grep: /proc/139/task/139/personality: Operation not permitted
grep: /proc/139/task/139/syscall: Operation not permitted
grep: /proc/139/task/139/mem: Permission denied
grep: /proc/139/task/139/clear_refs: Permission denied
grep: /proc/139/task/139/stack: Permission denied
grep: /proc/139/task/139/io: Permission denied
grep: /proc/139/task/139/patch_state: Permission denied
grep: /proc/139/fd: Permission denied
grep: /proc/139/map_files: Permission denied
grep: /proc/139/fdinfo: Permission denied
grep: /proc/139/ns: Permission denied
grep: /proc/139/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/139/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/139/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/139/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/139/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/139/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/139/net/nf_conntrack: Permission denied
grep: /proc/139/net/ip_tables_names: Permission denied
grep: /proc/139/net/ip6_tables_names: Permission denied
grep: /proc/139/net/ip_tables_matches: Permission denied
grep: /proc/139/net/ip_tables_targets: Permission denied
grep: /proc/139/net/ip6_tables_matches: Permission denied
grep: /proc/139/net/ip6_tables_targets: Permission denied
grep: /proc/139/net/nf_conntrack_expect: Permission denied
grep: /proc/139/environ: Permission denied
grep: /proc/139/auxv: Permission denied
grep: /proc/139/personality: Operation not permitted
grep: /proc/139/syscall: Operation not permitted
grep: /proc/139/mem: Permission denied
grep: /proc/139/mountstats: Permission denied
grep: /proc/139/clear_refs: Permission denied
grep: /proc/139/stack: Permission denied
grep: /proc/139/io: Permission denied
grep: /proc/139/patch_state: Permission denied
grep: /proc/140/task/140/fd: Permission denied
grep: /proc/140/task/140/fdinfo: Permission denied
grep: /proc/140/task/140/ns: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/140/task/140/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/140/task/140/net/nf_conntrack: Permission denied
grep: /proc/140/task/140/net/ip_tables_names: Permission denied
grep: /proc/140/task/140/net/ip6_tables_names: Permission denied
grep: /proc/140/task/140/net/ip_tables_matches: Permission denied
grep: /proc/140/task/140/net/ip_tables_targets: Permission denied
grep: /proc/140/task/140/net/ip6_tables_matches: Permission denied
grep: /proc/140/task/140/net/ip6_tables_targets: Permission denied
grep: /proc/140/task/140/net/nf_conntrack_expect: Permission denied
grep: /proc/140/task/140/environ: Permission denied
grep: /proc/140/task/140/auxv: Permission denied
grep: /proc/140/task/140/personality: Operation not permitted
grep: /proc/140/task/140/syscall: Operation not permitted
grep: /proc/140/task/140/mem: Permission denied
grep: /proc/140/task/140/clear_refs: Permission denied
grep: /proc/140/task/140/stack: Permission denied
grep: /proc/140/task/140/io: Permission denied
grep: /proc/140/task/140/patch_state: Permission denied
grep: /proc/140/fd: Permission denied
grep: /proc/140/map_files: Permission denied
grep: /proc/140/fdinfo: Permission denied
grep: /proc/140/ns: Permission denied
grep: /proc/140/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/140/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/140/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/140/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/140/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/140/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/140/net/nf_conntrack: Permission denied
grep: /proc/140/net/ip_tables_names: Permission denied
grep: /proc/140/net/ip6_tables_names: Permission denied
grep: /proc/140/net/ip_tables_matches: Permission denied
grep: /proc/140/net/ip_tables_targets: Permission denied
grep: /proc/140/net/ip6_tables_matches: Permission denied
grep: /proc/140/net/ip6_tables_targets: Permission denied
grep: /proc/140/net/nf_conntrack_expect: Permission denied
grep: /proc/140/environ: Permission denied
grep: /proc/140/auxv: Permission denied
grep: /proc/140/personality: Operation not permitted
grep: /proc/140/syscall: Operation not permitted
grep: /proc/140/mem: Permission denied
grep: /proc/140/mountstats: Permission denied
grep: /proc/140/clear_refs: Permission denied
grep: /proc/140/stack: Permission denied
grep: /proc/140/io: Permission denied
grep: /proc/140/patch_state: Permission denied
grep: /proc/161/task/161/fd: Permission denied
grep: /proc/161/task/161/fdinfo: Permission denied
grep: /proc/161/task/161/ns: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/161/task/161/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/161/task/161/net/nf_conntrack: Permission denied
grep: /proc/161/task/161/net/ip_tables_names: Permission denied
grep: /proc/161/task/161/net/ip6_tables_names: Permission denied
grep: /proc/161/task/161/net/ip_tables_matches: Permission denied
grep: /proc/161/task/161/net/ip_tables_targets: Permission denied
grep: /proc/161/task/161/net/ip6_tables_matches: Permission denied
grep: /proc/161/task/161/net/ip6_tables_targets: Permission denied
grep: /proc/161/task/161/net/nf_conntrack_expect: Permission denied
grep: /proc/161/task/161/environ: Permission denied
grep: /proc/161/task/161/auxv: Permission denied
grep: /proc/161/task/161/personality: Operation not permitted
grep: /proc/161/task/161/syscall: Operation not permitted
grep: /proc/161/task/161/mem: Permission denied
grep: /proc/161/task/161/clear_refs: Permission denied
grep: /proc/161/task/161/stack: Permission denied
grep: /proc/161/task/161/io: Permission denied
grep: /proc/161/task/161/patch_state: Permission denied
grep: /proc/161/fd: Permission denied
grep: /proc/161/map_files: Permission denied
grep: /proc/161/fdinfo: Permission denied
grep: /proc/161/ns: Permission denied
grep: /proc/161/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/161/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/161/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/161/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/161/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/161/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/161/net/nf_conntrack: Permission denied
grep: /proc/161/net/ip_tables_names: Permission denied
grep: /proc/161/net/ip6_tables_names: Permission denied
grep: /proc/161/net/ip_tables_matches: Permission denied
grep: /proc/161/net/ip_tables_targets: Permission denied
grep: /proc/161/net/ip6_tables_matches: Permission denied
grep: /proc/161/net/ip6_tables_targets: Permission denied
grep: /proc/161/net/nf_conntrack_expect: Permission denied
grep: /proc/161/environ: Permission denied
grep: /proc/161/auxv: Permission denied
grep: /proc/161/personality: Operation not permitted
grep: /proc/161/syscall: Operation not permitted
grep: /proc/161/mem: Permission denied
grep: /proc/161/mountstats: Permission denied
grep: /proc/161/clear_refs: Permission denied
grep: /proc/161/stack: Permission denied
grep: /proc/161/io: Permission denied
grep: /proc/161/patch_state: Permission denied
grep: /proc/162/task/162/fd: Permission denied
grep: /proc/162/task/162/fdinfo: Permission denied
grep: /proc/162/task/162/ns: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/162/task/162/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/162/task/162/net/nf_conntrack: Permission denied
grep: /proc/162/task/162/net/ip_tables_names: Permission denied
grep: /proc/162/task/162/net/ip6_tables_names: Permission denied
grep: /proc/162/task/162/net/ip_tables_matches: Permission denied
grep: /proc/162/task/162/net/ip_tables_targets: Permission denied
grep: /proc/162/task/162/net/ip6_tables_matches: Permission denied
grep: /proc/162/task/162/net/ip6_tables_targets: Permission denied
grep: /proc/162/task/162/net/nf_conntrack_expect: Permission denied
grep: /proc/162/task/162/environ: Permission denied
grep: /proc/162/task/162/auxv: Permission denied
grep: /proc/162/task/162/personality: Operation not permitted
grep: /proc/162/task/162/syscall: Operation not permitted
grep: /proc/162/task/162/mem: Permission denied
grep: /proc/162/task/162/clear_refs: Permission denied
grep: /proc/162/task/162/stack: Permission denied
grep: /proc/162/task/162/io: Permission denied
grep: /proc/162/task/162/patch_state: Permission denied
grep: /proc/162/fd: Permission denied
grep: /proc/162/map_files: Permission denied
grep: /proc/162/fdinfo: Permission denied
grep: /proc/162/ns: Permission denied
grep: /proc/162/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/162/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/162/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/162/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/162/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/162/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/162/net/nf_conntrack: Permission denied
grep: /proc/162/net/ip_tables_names: Permission denied
grep: /proc/162/net/ip6_tables_names: Permission denied
grep: /proc/162/net/ip_tables_matches: Permission denied
grep: /proc/162/net/ip_tables_targets: Permission denied
grep: /proc/162/net/ip6_tables_matches: Permission denied
grep: /proc/162/net/ip6_tables_targets: Permission denied
grep: /proc/162/net/nf_conntrack_expect: Permission denied
grep: /proc/162/environ: Permission denied
grep: /proc/162/auxv: Permission denied
grep: /proc/162/personality: Operation not permitted
grep: /proc/162/syscall: Operation not permitted
grep: /proc/162/mem: Permission denied
grep: /proc/162/mountstats: Permission denied
grep: /proc/162/clear_refs: Permission denied
grep: /proc/162/stack: Permission denied
grep: /proc/162/io: Permission denied
grep: /proc/162/patch_state: Permission denied
grep: /proc/163/task/163/fd: Permission denied
grep: /proc/163/task/163/fdinfo: Permission denied
grep: /proc/163/task/163/ns: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/163/task/163/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/163/task/163/net/nf_conntrack: Permission denied
grep: /proc/163/task/163/net/ip_tables_names: Permission denied
grep: /proc/163/task/163/net/ip6_tables_names: Permission denied
grep: /proc/163/task/163/net/ip_tables_matches: Permission denied
grep: /proc/163/task/163/net/ip_tables_targets: Permission denied
grep: /proc/163/task/163/net/ip6_tables_matches: Permission denied
grep: /proc/163/task/163/net/ip6_tables_targets: Permission denied
grep: /proc/163/task/163/net/nf_conntrack_expect: Permission denied
grep: /proc/163/task/163/environ: Permission denied
grep: /proc/163/task/163/auxv: Permission denied
grep: /proc/163/task/163/personality: Operation not permitted
grep: /proc/163/task/163/syscall: Operation not permitted
grep: /proc/163/task/163/mem: Permission denied
grep: /proc/163/task/163/clear_refs: Permission denied
grep: /proc/163/task/163/stack: Permission denied
grep: /proc/163/task/163/io: Permission denied
grep: /proc/163/task/163/patch_state: Permission denied
grep: /proc/163/fd: Permission denied
grep: /proc/163/map_files: Permission denied
grep: /proc/163/fdinfo: Permission denied
grep: /proc/163/ns: Permission denied
grep: /proc/163/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/163/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/163/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/163/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/163/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/163/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/163/net/nf_conntrack: Permission denied
grep: /proc/163/net/ip_tables_names: Permission denied
grep: /proc/163/net/ip6_tables_names: Permission denied
grep: /proc/163/net/ip_tables_matches: Permission denied
grep: /proc/163/net/ip_tables_targets: Permission denied
grep: /proc/163/net/ip6_tables_matches: Permission denied
grep: /proc/163/net/ip6_tables_targets: Permission denied
grep: /proc/163/net/nf_conntrack_expect: Permission denied
grep: /proc/163/environ: Permission denied
grep: /proc/163/auxv: Permission denied
grep: /proc/163/personality: Operation not permitted
grep: /proc/163/syscall: Operation not permitted
grep: /proc/163/mem: Permission denied
grep: /proc/163/mountstats: Permission denied
grep: /proc/163/clear_refs: Permission denied
grep: /proc/163/stack: Permission denied
grep: /proc/163/io: Permission denied
grep: /proc/163/patch_state: Permission denied
grep: /proc/164/task/164/fd: Permission denied
grep: /proc/164/task/164/fdinfo: Permission denied
grep: /proc/164/task/164/ns: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/164/task/164/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/164/task/164/net/nf_conntrack: Permission denied
grep: /proc/164/task/164/net/ip_tables_names: Permission denied
grep: /proc/164/task/164/net/ip6_tables_names: Permission denied
grep: /proc/164/task/164/net/ip_tables_matches: Permission denied
grep: /proc/164/task/164/net/ip_tables_targets: Permission denied
grep: /proc/164/task/164/net/ip6_tables_matches: Permission denied
grep: /proc/164/task/164/net/ip6_tables_targets: Permission denied
grep: /proc/164/task/164/net/nf_conntrack_expect: Permission denied
grep: /proc/164/task/164/environ: Permission denied
grep: /proc/164/task/164/auxv: Permission denied
grep: /proc/164/task/164/personality: Operation not permitted
grep: /proc/164/task/164/syscall: Operation not permitted
grep: /proc/164/task/164/mem: Permission denied
grep: /proc/164/task/164/clear_refs: Permission denied
grep: /proc/164/task/164/stack: Permission denied
grep: /proc/164/task/164/io: Permission denied
grep: /proc/164/task/164/patch_state: Permission denied
grep: /proc/164/fd: Permission denied
grep: /proc/164/map_files: Permission denied
grep: /proc/164/fdinfo: Permission denied
grep: /proc/164/ns: Permission denied
grep: /proc/164/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/164/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/164/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/164/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/164/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/164/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/164/net/nf_conntrack: Permission denied
grep: /proc/164/net/ip_tables_names: Permission denied
grep: /proc/164/net/ip6_tables_names: Permission denied
grep: /proc/164/net/ip_tables_matches: Permission denied
grep: /proc/164/net/ip_tables_targets: Permission denied
grep: /proc/164/net/ip6_tables_matches: Permission denied
grep: /proc/164/net/ip6_tables_targets: Permission denied
grep: /proc/164/net/nf_conntrack_expect: Permission denied
grep: /proc/164/environ: Permission denied
grep: /proc/164/auxv: Permission denied
grep: /proc/164/personality: Operation not permitted
grep: /proc/164/syscall: Operation not permitted
grep: /proc/164/mem: Permission denied
grep: /proc/164/mountstats: Permission denied
grep: /proc/164/clear_refs: Permission denied
grep: /proc/164/stack: Permission denied
grep: /proc/164/io: Permission denied
grep: /proc/164/patch_state: Permission denied
grep: /proc/165/task/165/fd: Permission denied
grep: /proc/165/task/165/fdinfo: Permission denied
grep: /proc/165/task/165/ns: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/165/task/165/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/165/task/165/net/nf_conntrack: Permission denied
grep: /proc/165/task/165/net/ip_tables_names: Permission denied
grep: /proc/165/task/165/net/ip6_tables_names: Permission denied
grep: /proc/165/task/165/net/ip_tables_matches: Permission denied
grep: /proc/165/task/165/net/ip_tables_targets: Permission denied
grep: /proc/165/task/165/net/ip6_tables_matches: Permission denied
grep: /proc/165/task/165/net/ip6_tables_targets: Permission denied
grep: /proc/165/task/165/net/nf_conntrack_expect: Permission denied
grep: /proc/165/task/165/environ: Permission denied
grep: /proc/165/task/165/auxv: Permission denied
grep: /proc/165/task/165/personality: Operation not permitted
grep: /proc/165/task/165/syscall: Operation not permitted
grep: /proc/165/task/165/mem: Permission denied
grep: /proc/165/task/165/clear_refs: Permission denied
grep: /proc/165/task/165/stack: Permission denied
grep: /proc/165/task/165/io: Permission denied
grep: /proc/165/task/165/patch_state: Permission denied
grep: /proc/165/fd: Permission denied
grep: /proc/165/map_files: Permission denied
grep: /proc/165/fdinfo: Permission denied
grep: /proc/165/ns: Permission denied
grep: /proc/165/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/165/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/165/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/165/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/165/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/165/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/165/net/nf_conntrack: Permission denied
grep: /proc/165/net/ip_tables_names: Permission denied
grep: /proc/165/net/ip6_tables_names: Permission denied
grep: /proc/165/net/ip_tables_matches: Permission denied
grep: /proc/165/net/ip_tables_targets: Permission denied
grep: /proc/165/net/ip6_tables_matches: Permission denied
grep: /proc/165/net/ip6_tables_targets: Permission denied
grep: /proc/165/net/nf_conntrack_expect: Permission denied
grep: /proc/165/environ: Permission denied
grep: /proc/165/auxv: Permission denied
grep: /proc/165/personality: Operation not permitted
grep: /proc/165/syscall: Operation not permitted
grep: /proc/165/mem: Permission denied
grep: /proc/165/mountstats: Permission denied
grep: /proc/165/clear_refs: Permission denied
grep: /proc/165/stack: Permission denied
grep: /proc/165/io: Permission denied
grep: /proc/165/patch_state: Permission denied
grep: /proc/166/task/166/fd: Permission denied
grep: /proc/166/task/166/fdinfo: Permission denied
grep: /proc/166/task/166/ns: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/166/task/166/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/166/task/166/net/nf_conntrack: Permission denied
grep: /proc/166/task/166/net/ip_tables_names: Permission denied
grep: /proc/166/task/166/net/ip6_tables_names: Permission denied
grep: /proc/166/task/166/net/ip_tables_matches: Permission denied
grep: /proc/166/task/166/net/ip_tables_targets: Permission denied
grep: /proc/166/task/166/net/ip6_tables_matches: Permission denied
grep: /proc/166/task/166/net/ip6_tables_targets: Permission denied
grep: /proc/166/task/166/net/nf_conntrack_expect: Permission denied
grep: /proc/166/task/166/environ: Permission denied
grep: /proc/166/task/166/auxv: Permission denied
grep: /proc/166/task/166/personality: Operation not permitted
grep: /proc/166/task/166/syscall: Operation not permitted
grep: /proc/166/task/166/mem: Permission denied
grep: /proc/166/task/166/clear_refs: Permission denied
grep: /proc/166/task/166/stack: Permission denied
grep: /proc/166/task/166/io: Permission denied
grep: /proc/166/task/166/patch_state: Permission denied
grep: /proc/166/fd: Permission denied
grep: /proc/166/map_files: Permission denied
grep: /proc/166/fdinfo: Permission denied
grep: /proc/166/ns: Permission denied
grep: /proc/166/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/166/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/166/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/166/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/166/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/166/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/166/net/nf_conntrack: Permission denied
grep: /proc/166/net/ip_tables_names: Permission denied
grep: /proc/166/net/ip6_tables_names: Permission denied
grep: /proc/166/net/ip_tables_matches: Permission denied
grep: /proc/166/net/ip_tables_targets: Permission denied
grep: /proc/166/net/ip6_tables_matches: Permission denied
grep: /proc/166/net/ip6_tables_targets: Permission denied
grep: /proc/166/net/nf_conntrack_expect: Permission denied
grep: /proc/166/environ: Permission denied
grep: /proc/166/auxv: Permission denied
grep: /proc/166/personality: Operation not permitted
grep: /proc/166/syscall: Operation not permitted
grep: /proc/166/mem: Permission denied
grep: /proc/166/mountstats: Permission denied
grep: /proc/166/clear_refs: Permission denied
grep: /proc/166/stack: Permission denied
grep: /proc/166/io: Permission denied
grep: /proc/166/patch_state: Permission denied
grep: /proc/167/task/167/fd: Permission denied
grep: /proc/167/task/167/fdinfo: Permission denied
grep: /proc/167/task/167/ns: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/167/task/167/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/167/task/167/net/nf_conntrack: Permission denied
grep: /proc/167/task/167/net/ip_tables_names: Permission denied
grep: /proc/167/task/167/net/ip6_tables_names: Permission denied
grep: /proc/167/task/167/net/ip_tables_matches: Permission denied
grep: /proc/167/task/167/net/ip_tables_targets: Permission denied
grep: /proc/167/task/167/net/ip6_tables_matches: Permission denied
grep: /proc/167/task/167/net/ip6_tables_targets: Permission denied
grep: /proc/167/task/167/net/nf_conntrack_expect: Permission denied
grep: /proc/167/task/167/environ: Permission denied
grep: /proc/167/task/167/auxv: Permission denied
grep: /proc/167/task/167/personality: Operation not permitted
grep: /proc/167/task/167/syscall: Operation not permitted
grep: /proc/167/task/167/mem: Permission denied
grep: /proc/167/task/167/clear_refs: Permission denied
grep: /proc/167/task/167/stack: Permission denied
grep: /proc/167/task/167/io: Permission denied
grep: /proc/167/task/167/patch_state: Permission denied
grep: /proc/167/fd: Permission denied
grep: /proc/167/map_files: Permission denied
grep: /proc/167/fdinfo: Permission denied
grep: /proc/167/ns: Permission denied
grep: /proc/167/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/167/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/167/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/167/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/167/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/167/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/167/net/nf_conntrack: Permission denied
grep: /proc/167/net/ip_tables_names: Permission denied
grep: /proc/167/net/ip6_tables_names: Permission denied
grep: /proc/167/net/ip_tables_matches: Permission denied
grep: /proc/167/net/ip_tables_targets: Permission denied
grep: /proc/167/net/ip6_tables_matches: Permission denied
grep: /proc/167/net/ip6_tables_targets: Permission denied
grep: /proc/167/net/nf_conntrack_expect: Permission denied
grep: /proc/167/environ: Permission denied
grep: /proc/167/auxv: Permission denied
grep: /proc/167/personality: Operation not permitted
grep: /proc/167/syscall: Operation not permitted
grep: /proc/167/mem: Permission denied
grep: /proc/167/mountstats: Permission denied
grep: /proc/167/clear_refs: Permission denied
grep: /proc/167/stack: Permission denied
grep: /proc/167/io: Permission denied
grep: /proc/167/patch_state: Permission denied
grep: /proc/168/task/168/fd: Permission denied
grep: /proc/168/task/168/fdinfo: Permission denied
grep: /proc/168/task/168/ns: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/168/task/168/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/168/task/168/net/nf_conntrack: Permission denied
grep: /proc/168/task/168/net/ip_tables_names: Permission denied
grep: /proc/168/task/168/net/ip6_tables_names: Permission denied
grep: /proc/168/task/168/net/ip_tables_matches: Permission denied
grep: /proc/168/task/168/net/ip_tables_targets: Permission denied
grep: /proc/168/task/168/net/ip6_tables_matches: Permission denied
grep: /proc/168/task/168/net/ip6_tables_targets: Permission denied
grep: /proc/168/task/168/net/nf_conntrack_expect: Permission denied
grep: /proc/168/task/168/environ: Permission denied
grep: /proc/168/task/168/auxv: Permission denied
grep: /proc/168/task/168/personality: Operation not permitted
grep: /proc/168/task/168/syscall: Operation not permitted
grep: /proc/168/task/168/mem: Permission denied
grep: /proc/168/task/168/clear_refs: Permission denied
grep: /proc/168/task/168/stack: Permission denied
grep: /proc/168/task/168/io: Permission denied
grep: /proc/168/task/168/patch_state: Permission denied
grep: /proc/168/fd: Permission denied
grep: /proc/168/map_files: Permission denied
grep: /proc/168/fdinfo: Permission denied
grep: /proc/168/ns: Permission denied
grep: /proc/168/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/168/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/168/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/168/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/168/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/168/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/168/net/nf_conntrack: Permission denied
grep: /proc/168/net/ip_tables_names: Permission denied
grep: /proc/168/net/ip6_tables_names: Permission denied
grep: /proc/168/net/ip_tables_matches: Permission denied
grep: /proc/168/net/ip_tables_targets: Permission denied
grep: /proc/168/net/ip6_tables_matches: Permission denied
grep: /proc/168/net/ip6_tables_targets: Permission denied
grep: /proc/168/net/nf_conntrack_expect: Permission denied
grep: /proc/168/environ: Permission denied
grep: /proc/168/auxv: Permission denied
grep: /proc/168/personality: Operation not permitted
grep: /proc/168/syscall: Operation not permitted
grep: /proc/168/mem: Permission denied
grep: /proc/168/mountstats: Permission denied
grep: /proc/168/clear_refs: Permission denied
grep: /proc/168/stack: Permission denied
grep: /proc/168/io: Permission denied
grep: /proc/168/patch_state: Permission denied
grep: /proc/169/task/169/fd: Permission denied
grep: /proc/169/task/169/fdinfo: Permission denied
grep: /proc/169/task/169/ns: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/169/task/169/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/169/task/169/net/nf_conntrack: Permission denied
grep: /proc/169/task/169/net/ip_tables_names: Permission denied
grep: /proc/169/task/169/net/ip6_tables_names: Permission denied
grep: /proc/169/task/169/net/ip_tables_matches: Permission denied
grep: /proc/169/task/169/net/ip_tables_targets: Permission denied
grep: /proc/169/task/169/net/ip6_tables_matches: Permission denied
grep: /proc/169/task/169/net/ip6_tables_targets: Permission denied
grep: /proc/169/task/169/net/nf_conntrack_expect: Permission denied
grep: /proc/169/task/169/environ: Permission denied
grep: /proc/169/task/169/auxv: Permission denied
grep: /proc/169/task/169/personality: Operation not permitted
grep: /proc/169/task/169/syscall: Operation not permitted
grep: /proc/169/task/169/mem: Permission denied
grep: /proc/169/task/169/clear_refs: Permission denied
grep: /proc/169/task/169/stack: Permission denied
grep: /proc/169/task/169/io: Permission denied
grep: /proc/169/task/169/patch_state: Permission denied
grep: /proc/169/fd: Permission denied
grep: /proc/169/map_files: Permission denied
grep: /proc/169/fdinfo: Permission denied
grep: /proc/169/ns: Permission denied
grep: /proc/169/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/169/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/169/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/169/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/169/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/169/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/169/net/nf_conntrack: Permission denied
grep: /proc/169/net/ip_tables_names: Permission denied
grep: /proc/169/net/ip6_tables_names: Permission denied
grep: /proc/169/net/ip_tables_matches: Permission denied
grep: /proc/169/net/ip_tables_targets: Permission denied
grep: /proc/169/net/ip6_tables_matches: Permission denied
grep: /proc/169/net/ip6_tables_targets: Permission denied
grep: /proc/169/net/nf_conntrack_expect: Permission denied
grep: /proc/169/environ: Permission denied
grep: /proc/169/auxv: Permission denied
grep: /proc/169/personality: Operation not permitted
grep: /proc/169/syscall: Operation not permitted
grep: /proc/169/mem: Permission denied
grep: /proc/169/mountstats: Permission denied
grep: /proc/169/clear_refs: Permission denied
grep: /proc/169/stack: Permission denied
grep: /proc/169/io: Permission denied
grep: /proc/169/patch_state: Permission denied
grep: /proc/170/task/170/fd: Permission denied
grep: /proc/170/task/170/fdinfo: Permission denied
grep: /proc/170/task/170/ns: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/170/task/170/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/170/task/170/net/nf_conntrack: Permission denied
grep: /proc/170/task/170/net/ip_tables_names: Permission denied
grep: /proc/170/task/170/net/ip6_tables_names: Permission denied
grep: /proc/170/task/170/net/ip_tables_matches: Permission denied
grep: /proc/170/task/170/net/ip_tables_targets: Permission denied
grep: /proc/170/task/170/net/ip6_tables_matches: Permission denied
grep: /proc/170/task/170/net/ip6_tables_targets: Permission denied
grep: /proc/170/task/170/net/nf_conntrack_expect: Permission denied
grep: /proc/170/task/170/environ: Permission denied
grep: /proc/170/task/170/auxv: Permission denied
grep: /proc/170/task/170/personality: Operation not permitted
grep: /proc/170/task/170/syscall: Operation not permitted
grep: /proc/170/task/170/mem: Permission denied
grep: /proc/170/task/170/clear_refs: Permission denied
grep: /proc/170/task/170/stack: Permission denied
grep: /proc/170/task/170/io: Permission denied
grep: /proc/170/task/170/patch_state: Permission denied
grep: /proc/170/fd: Permission denied
grep: /proc/170/map_files: Permission denied
grep: /proc/170/fdinfo: Permission denied
grep: /proc/170/ns: Permission denied
grep: /proc/170/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/170/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/170/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/170/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/170/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/170/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/170/net/nf_conntrack: Permission denied
grep: /proc/170/net/ip_tables_names: Permission denied
grep: /proc/170/net/ip6_tables_names: Permission denied
grep: /proc/170/net/ip_tables_matches: Permission denied
grep: /proc/170/net/ip_tables_targets: Permission denied
grep: /proc/170/net/ip6_tables_matches: Permission denied
grep: /proc/170/net/ip6_tables_targets: Permission denied
grep: /proc/170/net/nf_conntrack_expect: Permission denied
grep: /proc/170/environ: Permission denied
grep: /proc/170/auxv: Permission denied
grep: /proc/170/personality: Operation not permitted
grep: /proc/170/syscall: Operation not permitted
grep: /proc/170/mem: Permission denied
grep: /proc/170/mountstats: Permission denied
grep: /proc/170/clear_refs: Permission denied
grep: /proc/170/stack: Permission denied
grep: /proc/170/io: Permission denied
grep: /proc/170/patch_state: Permission denied
grep: /proc/171/task/171/fd: Permission denied
grep: /proc/171/task/171/fdinfo: Permission denied
grep: /proc/171/task/171/ns: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/171/task/171/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/171/task/171/net/nf_conntrack: Permission denied
grep: /proc/171/task/171/net/ip_tables_names: Permission denied
grep: /proc/171/task/171/net/ip6_tables_names: Permission denied
grep: /proc/171/task/171/net/ip_tables_matches: Permission denied
grep: /proc/171/task/171/net/ip_tables_targets: Permission denied
grep: /proc/171/task/171/net/ip6_tables_matches: Permission denied
grep: /proc/171/task/171/net/ip6_tables_targets: Permission denied
grep: /proc/171/task/171/net/nf_conntrack_expect: Permission denied
grep: /proc/171/task/171/environ: Permission denied
grep: /proc/171/task/171/auxv: Permission denied
grep: /proc/171/task/171/personality: Operation not permitted
grep: /proc/171/task/171/syscall: Operation not permitted
grep: /proc/171/task/171/mem: Permission denied
grep: /proc/171/task/171/clear_refs: Permission denied
grep: /proc/171/task/171/stack: Permission denied
grep: /proc/171/task/171/io: Permission denied
grep: /proc/171/task/171/patch_state: Permission denied
grep: /proc/171/fd: Permission denied
grep: /proc/171/map_files: Permission denied
grep: /proc/171/fdinfo: Permission denied
grep: /proc/171/ns: Permission denied
grep: /proc/171/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/171/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/171/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/171/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/171/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/171/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/171/net/nf_conntrack: Permission denied
grep: /proc/171/net/ip_tables_names: Permission denied
grep: /proc/171/net/ip6_tables_names: Permission denied
grep: /proc/171/net/ip_tables_matches: Permission denied
grep: /proc/171/net/ip_tables_targets: Permission denied
grep: /proc/171/net/ip6_tables_matches: Permission denied
grep: /proc/171/net/ip6_tables_targets: Permission denied
grep: /proc/171/net/nf_conntrack_expect: Permission denied
grep: /proc/171/environ: Permission denied
grep: /proc/171/auxv: Permission denied
grep: /proc/171/personality: Operation not permitted
grep: /proc/171/syscall: Operation not permitted
grep: /proc/171/mem: Permission denied
grep: /proc/171/mountstats: Permission denied
grep: /proc/171/clear_refs: Permission denied
grep: /proc/171/stack: Permission denied
grep: /proc/171/io: Permission denied
grep: /proc/171/patch_state: Permission denied
grep: /proc/172/task/172/fd: Permission denied
grep: /proc/172/task/172/fdinfo: Permission denied
grep: /proc/172/task/172/ns: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/172/task/172/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/172/task/172/net/nf_conntrack: Permission denied
grep: /proc/172/task/172/net/ip_tables_names: Permission denied
grep: /proc/172/task/172/net/ip6_tables_names: Permission denied
grep: /proc/172/task/172/net/ip_tables_matches: Permission denied
grep: /proc/172/task/172/net/ip_tables_targets: Permission denied
grep: /proc/172/task/172/net/ip6_tables_matches: Permission denied
grep: /proc/172/task/172/net/ip6_tables_targets: Permission denied
grep: /proc/172/task/172/net/nf_conntrack_expect: Permission denied
grep: /proc/172/task/172/environ: Permission denied
grep: /proc/172/task/172/auxv: Permission denied
grep: /proc/172/task/172/personality: Operation not permitted
grep: /proc/172/task/172/syscall: Operation not permitted
grep: /proc/172/task/172/mem: Permission denied
grep: /proc/172/task/172/clear_refs: Permission denied
grep: /proc/172/task/172/stack: Permission denied
grep: /proc/172/task/172/io: Permission denied
grep: /proc/172/task/172/patch_state: Permission denied
grep: /proc/172/fd: Permission denied
grep: /proc/172/map_files: Permission denied
grep: /proc/172/fdinfo: Permission denied
grep: /proc/172/ns: Permission denied
grep: /proc/172/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/172/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/172/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/172/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/172/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/172/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/172/net/nf_conntrack: Permission denied
grep: /proc/172/net/ip_tables_names: Permission denied
grep: /proc/172/net/ip6_tables_names: Permission denied
grep: /proc/172/net/ip_tables_matches: Permission denied
grep: /proc/172/net/ip_tables_targets: Permission denied
grep: /proc/172/net/ip6_tables_matches: Permission denied
grep: /proc/172/net/ip6_tables_targets: Permission denied
grep: /proc/172/net/nf_conntrack_expect: Permission denied
grep: /proc/172/environ: Permission denied
grep: /proc/172/auxv: Permission denied
grep: /proc/172/personality: Operation not permitted
grep: /proc/172/syscall: Operation not permitted
grep: /proc/172/mem: Permission denied
grep: /proc/172/mountstats: Permission denied
grep: /proc/172/clear_refs: Permission denied
grep: /proc/172/stack: Permission denied
grep: /proc/172/io: Permission denied
grep: /proc/172/patch_state: Permission denied
grep: /proc/234/task/234/fd: Permission denied
grep: /proc/234/task/234/fdinfo: Permission denied
grep: /proc/234/task/234/ns: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/234/task/234/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/234/task/234/net/nf_conntrack: Permission denied
grep: /proc/234/task/234/net/ip_tables_names: Permission denied
grep: /proc/234/task/234/net/ip6_tables_names: Permission denied
grep: /proc/234/task/234/net/ip_tables_matches: Permission denied
grep: /proc/234/task/234/net/ip_tables_targets: Permission denied
grep: /proc/234/task/234/net/ip6_tables_matches: Permission denied
grep: /proc/234/task/234/net/ip6_tables_targets: Permission denied
grep: /proc/234/task/234/net/nf_conntrack_expect: Permission denied
grep: /proc/234/task/234/environ: Permission denied
grep: /proc/234/task/234/auxv: Permission denied
grep: /proc/234/task/234/personality: Operation not permitted
grep: /proc/234/task/234/syscall: Operation not permitted
grep: /proc/234/task/234/maps: Permission denied
grep: /proc/234/task/234/numa_maps: Permission denied
grep: /proc/234/task/234/mem: Permission denied
grep: /proc/234/task/234/clear_refs: Permission denied
grep: /proc/234/task/234/smaps: Permission denied
grep: /proc/234/task/234/pagemap: Permission denied
grep: /proc/234/task/234/stack: Permission denied
grep: /proc/234/task/234/io: Permission denied
grep: /proc/234/task/234/patch_state: Permission denied
grep: /proc/234/fd: Permission denied
grep: /proc/234/map_files: Permission denied
grep: /proc/234/fdinfo: Permission denied
grep: /proc/234/ns: Permission denied
grep: /proc/234/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/234/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/234/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/234/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/234/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/234/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/234/net/nf_conntrack: Permission denied
grep: /proc/234/net/ip_tables_names: Permission denied
grep: /proc/234/net/ip6_tables_names: Permission denied
grep: /proc/234/net/ip_tables_matches: Permission denied
grep: /proc/234/net/ip_tables_targets: Permission denied
grep: /proc/234/net/ip6_tables_matches: Permission denied
grep: /proc/234/net/ip6_tables_targets: Permission denied
grep: /proc/234/net/nf_conntrack_expect: Permission denied
grep: /proc/234/environ: Permission denied
grep: /proc/234/auxv: Permission denied
grep: /proc/234/personality: Operation not permitted
grep: /proc/234/syscall: Operation not permitted
grep: /proc/234/maps: Permission denied
grep: /proc/234/numa_maps: Permission denied
grep: /proc/234/mem: Permission denied
grep: /proc/234/mountstats: Permission denied
grep: /proc/234/clear_refs: Permission denied
grep: /proc/234/smaps: Permission denied
grep: /proc/234/pagemap: Permission denied
grep: /proc/234/stack: Permission denied
grep: /proc/234/io: Permission denied
grep: /proc/234/patch_state: Permission denied
grep: /proc/269/task/269/fd: Permission denied
grep: /proc/269/task/269/fdinfo: Permission denied
grep: /proc/269/task/269/ns: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/269/task/269/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/269/task/269/net/nf_conntrack: Permission denied
grep: /proc/269/task/269/net/ip_tables_names: Permission denied
grep: /proc/269/task/269/net/ip6_tables_names: Permission denied
grep: /proc/269/task/269/net/ip_tables_matches: Permission denied
grep: /proc/269/task/269/net/ip_tables_targets: Permission denied
grep: /proc/269/task/269/net/ip6_tables_matches: Permission denied
grep: /proc/269/task/269/net/ip6_tables_targets: Permission denied
grep: /proc/269/task/269/net/nf_conntrack_expect: Permission denied
grep: /proc/269/task/269/environ: Permission denied
grep: /proc/269/task/269/auxv: Permission denied
grep: /proc/269/task/269/personality: Operation not permitted
grep: /proc/269/task/269/syscall: Operation not permitted
grep: /proc/269/task/269/maps: Permission denied
grep: /proc/269/task/269/numa_maps: Permission denied
grep: /proc/269/task/269/mem: Permission denied
grep: /proc/269/task/269/clear_refs: Permission denied
grep: /proc/269/task/269/smaps: Permission denied
grep: /proc/269/task/269/pagemap: Permission denied
grep: /proc/269/task/269/stack: Permission denied
grep: /proc/269/task/269/io: Permission denied
grep: /proc/269/task/269/patch_state: Permission denied
grep: /proc/269/fd: Permission denied
grep: /proc/269/map_files: Permission denied
grep: /proc/269/fdinfo: Permission denied
grep: /proc/269/ns: Permission denied
grep: /proc/269/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/269/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/269/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/269/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/269/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/269/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/269/net/nf_conntrack: Permission denied
grep: /proc/269/net/ip_tables_names: Permission denied
grep: /proc/269/net/ip6_tables_names: Permission denied
grep: /proc/269/net/ip_tables_matches: Permission denied
grep: /proc/269/net/ip_tables_targets: Permission denied
grep: /proc/269/net/ip6_tables_matches: Permission denied
grep: /proc/269/net/ip6_tables_targets: Permission denied
grep: /proc/269/net/nf_conntrack_expect: Permission denied
grep: /proc/269/environ: Permission denied
grep: /proc/269/auxv: Permission denied
grep: /proc/269/personality: Operation not permitted
grep: /proc/269/syscall: Operation not permitted
grep: /proc/269/maps: Permission denied
grep: /proc/269/numa_maps: Permission denied
grep: /proc/269/mem: Permission denied
grep: /proc/269/mountstats: Permission denied
grep: /proc/269/clear_refs: Permission denied
grep: /proc/269/smaps: Permission denied
grep: /proc/269/pagemap: Permission denied
grep: /proc/269/stack: Permission denied
grep: /proc/269/io: Permission denied
grep: /proc/269/patch_state: Permission denied
grep: /proc/286/task/286/fd: Permission denied
grep: /proc/286/task/286/fdinfo: Permission denied
grep: /proc/286/task/286/ns: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/286/task/286/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/286/task/286/net/nf_conntrack: Permission denied
grep: /proc/286/task/286/net/ip_tables_names: Permission denied
grep: /proc/286/task/286/net/ip6_tables_names: Permission denied
grep: /proc/286/task/286/net/ip_tables_matches: Permission denied
grep: /proc/286/task/286/net/ip_tables_targets: Permission denied
grep: /proc/286/task/286/net/ip6_tables_matches: Permission denied
grep: /proc/286/task/286/net/ip6_tables_targets: Permission denied
grep: /proc/286/task/286/net/nf_conntrack_expect: Permission denied
grep: /proc/286/task/286/environ: Permission denied
grep: /proc/286/task/286/auxv: Permission denied
grep: /proc/286/task/286/personality: Operation not permitted
grep: /proc/286/task/286/syscall: Operation not permitted
grep: /proc/286/task/286/mem: Permission denied
grep: /proc/286/task/286/clear_refs: Permission denied
grep: /proc/286/task/286/stack: Permission denied
grep: /proc/286/task/286/io: Permission denied
grep: /proc/286/task/286/patch_state: Permission denied
grep: /proc/286/fd: Permission denied
grep: /proc/286/map_files: Permission denied
grep: /proc/286/fdinfo: Permission denied
grep: /proc/286/ns: Permission denied
grep: /proc/286/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/286/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/286/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/286/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/286/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/286/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/286/net/nf_conntrack: Permission denied
grep: /proc/286/net/ip_tables_names: Permission denied
grep: /proc/286/net/ip6_tables_names: Permission denied
grep: /proc/286/net/ip_tables_matches: Permission denied
grep: /proc/286/net/ip_tables_targets: Permission denied
grep: /proc/286/net/ip6_tables_matches: Permission denied
grep: /proc/286/net/ip6_tables_targets: Permission denied
grep: /proc/286/net/nf_conntrack_expect: Permission denied
grep: /proc/286/environ: Permission denied
grep: /proc/286/auxv: Permission denied
grep: /proc/286/personality: Operation not permitted
grep: /proc/286/syscall: Operation not permitted
grep: /proc/286/mem: Permission denied
grep: /proc/286/mountstats: Permission denied
grep: /proc/286/clear_refs: Permission denied
grep: /proc/286/stack: Permission denied
grep: /proc/286/io: Permission denied
grep: /proc/286/patch_state: Permission denied
grep: /proc/287/task/287/fd: Permission denied
grep: /proc/287/task/287/fdinfo: Permission denied
grep: /proc/287/task/287/ns: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/287/task/287/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/287/task/287/net/nf_conntrack: Permission denied
grep: /proc/287/task/287/net/ip_tables_names: Permission denied
grep: /proc/287/task/287/net/ip6_tables_names: Permission denied
grep: /proc/287/task/287/net/ip_tables_matches: Permission denied
grep: /proc/287/task/287/net/ip_tables_targets: Permission denied
grep: /proc/287/task/287/net/ip6_tables_matches: Permission denied
grep: /proc/287/task/287/net/ip6_tables_targets: Permission denied
grep: /proc/287/task/287/net/nf_conntrack_expect: Permission denied
grep: /proc/287/task/287/environ: Permission denied
grep: /proc/287/task/287/auxv: Permission denied
grep: /proc/287/task/287/personality: Operation not permitted
grep: /proc/287/task/287/syscall: Operation not permitted
grep: /proc/287/task/287/mem: Permission denied
grep: /proc/287/task/287/clear_refs: Permission denied
grep: /proc/287/task/287/stack: Permission denied
grep: /proc/287/task/287/io: Permission denied
grep: /proc/287/task/287/patch_state: Permission denied
grep: /proc/287/fd: Permission denied
grep: /proc/287/map_files: Permission denied
grep: /proc/287/fdinfo: Permission denied
grep: /proc/287/ns: Permission denied
grep: /proc/287/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/287/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/287/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/287/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/287/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/287/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/287/net/nf_conntrack: Permission denied
grep: /proc/287/net/ip_tables_names: Permission denied
grep: /proc/287/net/ip6_tables_names: Permission denied
grep: /proc/287/net/ip_tables_matches: Permission denied
grep: /proc/287/net/ip_tables_targets: Permission denied
grep: /proc/287/net/ip6_tables_matches: Permission denied
grep: /proc/287/net/ip6_tables_targets: Permission denied
grep: /proc/287/net/nf_conntrack_expect: Permission denied
grep: /proc/287/environ: Permission denied
grep: /proc/287/auxv: Permission denied
grep: /proc/287/personality: Operation not permitted
grep: /proc/287/syscall: Operation not permitted
grep: /proc/287/mem: Permission denied
grep: /proc/287/mountstats: Permission denied
grep: /proc/287/clear_refs: Permission denied
grep: /proc/287/stack: Permission denied
grep: /proc/287/io: Permission denied
grep: /proc/287/patch_state: Permission denied
grep: /proc/289/task/289/fd: Permission denied
grep: /proc/289/task/289/fdinfo: Permission denied
grep: /proc/289/task/289/ns: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/289/task/289/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/289/task/289/net/nf_conntrack: Permission denied
grep: /proc/289/task/289/net/ip_tables_names: Permission denied
grep: /proc/289/task/289/net/ip6_tables_names: Permission denied
grep: /proc/289/task/289/net/ip_tables_matches: Permission denied
grep: /proc/289/task/289/net/ip_tables_targets: Permission denied
grep: /proc/289/task/289/net/ip6_tables_matches: Permission denied
grep: /proc/289/task/289/net/ip6_tables_targets: Permission denied
grep: /proc/289/task/289/net/nf_conntrack_expect: Permission denied
grep: /proc/289/task/289/environ: Permission denied
grep: /proc/289/task/289/auxv: Permission denied
grep: /proc/289/task/289/personality: Operation not permitted
grep: /proc/289/task/289/syscall: Operation not permitted
grep: /proc/289/task/289/maps: Permission denied
grep: /proc/289/task/289/numa_maps: Permission denied
grep: /proc/289/task/289/mem: Permission denied
grep: /proc/289/task/289/clear_refs: Permission denied
grep: /proc/289/task/289/smaps: Permission denied
grep: /proc/289/task/289/pagemap: Permission denied
grep: /proc/289/task/289/stack: Permission denied
grep: /proc/289/task/289/io: Permission denied
grep: /proc/289/task/289/patch_state: Permission denied
grep: /proc/289/task/290/fd: Permission denied
grep: /proc/289/task/290/fdinfo: Permission denied
grep: /proc/289/task/290/ns: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/289/task/290/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/289/task/290/net/nf_conntrack: Permission denied
grep: /proc/289/task/290/net/ip_tables_names: Permission denied
grep: /proc/289/task/290/net/ip6_tables_names: Permission denied
grep: /proc/289/task/290/net/ip_tables_matches: Permission denied
grep: /proc/289/task/290/net/ip_tables_targets: Permission denied
grep: /proc/289/task/290/net/ip6_tables_matches: Permission denied
grep: /proc/289/task/290/net/ip6_tables_targets: Permission denied
grep: /proc/289/task/290/net/nf_conntrack_expect: Permission denied
grep: /proc/289/task/290/environ: Permission denied
grep: /proc/289/task/290/auxv: Permission denied
grep: /proc/289/task/290/personality: Operation not permitted
grep: /proc/289/task/290/syscall: Operation not permitted
grep: /proc/289/task/290/maps: Permission denied
grep: /proc/289/task/290/numa_maps: Permission denied
grep: /proc/289/task/290/mem: Permission denied
grep: /proc/289/task/290/clear_refs: Permission denied
grep: /proc/289/task/290/smaps: Permission denied
grep: /proc/289/task/290/pagemap: Permission denied
grep: /proc/289/task/290/stack: Permission denied
grep: /proc/289/task/290/io: Permission denied
grep: /proc/289/task/290/patch_state: Permission denied
grep: /proc/289/fd: Permission denied
grep: /proc/289/map_files: Permission denied
grep: /proc/289/fdinfo: Permission denied
grep: /proc/289/ns: Permission denied
grep: /proc/289/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/289/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/289/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/289/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/289/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/289/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/289/net/nf_conntrack: Permission denied
grep: /proc/289/net/ip_tables_names: Permission denied
grep: /proc/289/net/ip6_tables_names: Permission denied
grep: /proc/289/net/ip_tables_matches: Permission denied
grep: /proc/289/net/ip_tables_targets: Permission denied
grep: /proc/289/net/ip6_tables_matches: Permission denied
grep: /proc/289/net/ip6_tables_targets: Permission denied
grep: /proc/289/net/nf_conntrack_expect: Permission denied
grep: /proc/289/environ: Permission denied
grep: /proc/289/auxv: Permission denied
grep: /proc/289/personality: Operation not permitted
grep: /proc/289/syscall: Operation not permitted
grep: /proc/289/maps: Permission denied
grep: /proc/289/numa_maps: Permission denied
grep: /proc/289/mem: Permission denied
grep: /proc/289/mountstats: Permission denied
grep: /proc/289/clear_refs: Permission denied
grep: /proc/289/smaps: Permission denied
grep: /proc/289/pagemap: Permission denied
grep: /proc/289/stack: Permission denied
grep: /proc/289/io: Permission denied
grep: /proc/289/patch_state: Permission denied
grep: /proc/323/task/323/fd: Permission denied
grep: /proc/323/task/323/fdinfo: Permission denied
grep: /proc/323/task/323/ns: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/323/task/323/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/323/task/323/net/nf_conntrack: Permission denied
grep: /proc/323/task/323/net/ip_tables_names: Permission denied
grep: /proc/323/task/323/net/ip6_tables_names: Permission denied
grep: /proc/323/task/323/net/ip_tables_matches: Permission denied
grep: /proc/323/task/323/net/ip_tables_targets: Permission denied
grep: /proc/323/task/323/net/ip6_tables_matches: Permission denied
grep: /proc/323/task/323/net/ip6_tables_targets: Permission denied
grep: /proc/323/task/323/net/nf_conntrack_expect: Permission denied
grep: /proc/323/task/323/environ: Permission denied
grep: /proc/323/task/323/auxv: Permission denied
grep: /proc/323/task/323/personality: Operation not permitted
grep: /proc/323/task/323/syscall: Operation not permitted
grep: /proc/323/task/323/maps: Permission denied
grep: /proc/323/task/323/numa_maps: Permission denied
grep: /proc/323/task/323/mem: Permission denied
grep: /proc/323/task/323/clear_refs: Permission denied
grep: /proc/323/task/323/smaps: Permission denied
grep: /proc/323/task/323/pagemap: Permission denied
grep: /proc/323/task/323/stack: Permission denied
grep: /proc/323/task/323/io: Permission denied
grep: /proc/323/task/323/patch_state: Permission denied
grep: /proc/323/fd: Permission denied
grep: /proc/323/map_files: Permission denied
grep: /proc/323/fdinfo: Permission denied
grep: /proc/323/ns: Permission denied
grep: /proc/323/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/323/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/323/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/323/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/323/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/323/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/323/net/nf_conntrack: Permission denied
grep: /proc/323/net/ip_tables_names: Permission denied
grep: /proc/323/net/ip6_tables_names: Permission denied
grep: /proc/323/net/ip_tables_matches: Permission denied
grep: /proc/323/net/ip_tables_targets: Permission denied
grep: /proc/323/net/ip6_tables_matches: Permission denied
grep: /proc/323/net/ip6_tables_targets: Permission denied
grep: /proc/323/net/nf_conntrack_expect: Permission denied
grep: /proc/323/environ: Permission denied
grep: /proc/323/auxv: Permission denied
grep: /proc/323/personality: Operation not permitted
grep: /proc/323/syscall: Operation not permitted
grep: /proc/323/maps: Permission denied
grep: /proc/323/numa_maps: Permission denied
grep: /proc/323/mem: Permission denied
grep: /proc/323/mountstats: Permission denied
grep: /proc/323/clear_refs: Permission denied
grep: /proc/323/smaps: Permission denied
grep: /proc/323/pagemap: Permission denied
grep: /proc/323/stack: Permission denied
grep: /proc/323/io: Permission denied
grep: /proc/323/patch_state: Permission denied
grep: /proc/324/task/324/fd: Permission denied
grep: /proc/324/task/324/fdinfo: Permission denied
grep: /proc/324/task/324/ns: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/324/task/324/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/324/task/324/net/nf_conntrack: Permission denied
grep: /proc/324/task/324/net/ip_tables_names: Permission denied
grep: /proc/324/task/324/net/ip6_tables_names: Permission denied
grep: /proc/324/task/324/net/ip_tables_matches: Permission denied
grep: /proc/324/task/324/net/ip_tables_targets: Permission denied
grep: /proc/324/task/324/net/ip6_tables_matches: Permission denied
grep: /proc/324/task/324/net/ip6_tables_targets: Permission denied
grep: /proc/324/task/324/net/nf_conntrack_expect: Permission denied
grep: /proc/324/task/324/environ: Permission denied
grep: /proc/324/task/324/auxv: Permission denied
grep: /proc/324/task/324/personality: Operation not permitted
grep: /proc/324/task/324/syscall: Operation not permitted
grep: /proc/324/task/324/maps: Permission denied
grep: /proc/324/task/324/numa_maps: Permission denied
grep: /proc/324/task/324/mem: Permission denied
grep: /proc/324/task/324/clear_refs: Permission denied
grep: /proc/324/task/324/smaps: Permission denied
grep: /proc/324/task/324/pagemap: Permission denied
grep: /proc/324/task/324/stack: Permission denied
grep: /proc/324/task/324/io: Permission denied
grep: /proc/324/task/324/patch_state: Permission denied
grep: /proc/324/task/375/fd: Permission denied
grep: /proc/324/task/375/fdinfo: Permission denied
grep: /proc/324/task/375/ns: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/324/task/375/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/324/task/375/net/nf_conntrack: Permission denied
grep: /proc/324/task/375/net/ip_tables_names: Permission denied
grep: /proc/324/task/375/net/ip6_tables_names: Permission denied
grep: /proc/324/task/375/net/ip_tables_matches: Permission denied
grep: /proc/324/task/375/net/ip_tables_targets: Permission denied
grep: /proc/324/task/375/net/ip6_tables_matches: Permission denied
grep: /proc/324/task/375/net/ip6_tables_targets: Permission denied
grep: /proc/324/task/375/net/nf_conntrack_expect: Permission denied
grep: /proc/324/task/375/environ: Permission denied
grep: /proc/324/task/375/auxv: Permission denied
grep: /proc/324/task/375/personality: Operation not permitted
grep: /proc/324/task/375/syscall: Operation not permitted
grep: /proc/324/task/375/maps: Permission denied
grep: /proc/324/task/375/numa_maps: Permission denied
grep: /proc/324/task/375/mem: Permission denied
grep: /proc/324/task/375/clear_refs: Permission denied
grep: /proc/324/task/375/smaps: Permission denied
grep: /proc/324/task/375/pagemap: Permission denied
grep: /proc/324/task/375/stack: Permission denied
grep: /proc/324/task/375/io: Permission denied
grep: /proc/324/task/375/patch_state: Permission denied
grep: /proc/324/fd: Permission denied
grep: /proc/324/map_files: Permission denied
grep: /proc/324/fdinfo: Permission denied
grep: /proc/324/ns: Permission denied
grep: /proc/324/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/324/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/324/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/324/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/324/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/324/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/324/net/nf_conntrack: Permission denied
grep: /proc/324/net/ip_tables_names: Permission denied
grep: /proc/324/net/ip6_tables_names: Permission denied
grep: /proc/324/net/ip_tables_matches: Permission denied
grep: /proc/324/net/ip_tables_targets: Permission denied
grep: /proc/324/net/ip6_tables_matches: Permission denied
grep: /proc/324/net/ip6_tables_targets: Permission denied
grep: /proc/324/net/nf_conntrack_expect: Permission denied
grep: /proc/324/environ: Permission denied
grep: /proc/324/auxv: Permission denied
grep: /proc/324/personality: Operation not permitted
grep: /proc/324/syscall: Operation not permitted
grep: /proc/324/maps: Permission denied
grep: /proc/324/numa_maps: Permission denied
grep: /proc/324/mem: Permission denied
grep: /proc/324/mountstats: Permission denied
grep: /proc/324/clear_refs: Permission denied
grep: /proc/324/smaps: Permission denied
grep: /proc/324/pagemap: Permission denied
grep: /proc/324/stack: Permission denied
grep: /proc/324/io: Permission denied
grep: /proc/324/patch_state: Permission denied
grep: /proc/341/task/341/fd: Permission denied
grep: /proc/341/task/341/fdinfo: Permission denied
grep: /proc/341/task/341/ns: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/341/task/341/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/341/task/341/net/nf_conntrack: Permission denied
grep: /proc/341/task/341/net/ip_tables_names: Permission denied
grep: /proc/341/task/341/net/ip6_tables_names: Permission denied
grep: /proc/341/task/341/net/ip_tables_matches: Permission denied
grep: /proc/341/task/341/net/ip_tables_targets: Permission denied
grep: /proc/341/task/341/net/ip6_tables_matches: Permission denied
grep: /proc/341/task/341/net/ip6_tables_targets: Permission denied
grep: /proc/341/task/341/net/nf_conntrack_expect: Permission denied
grep: /proc/341/task/341/environ: Permission denied
grep: /proc/341/task/341/auxv: Permission denied
grep: /proc/341/task/341/personality: Operation not permitted
grep: /proc/341/task/341/syscall: Operation not permitted
grep: /proc/341/task/341/maps: Permission denied
grep: /proc/341/task/341/numa_maps: Permission denied
grep: /proc/341/task/341/mem: Permission denied
grep: /proc/341/task/341/clear_refs: Permission denied
grep: /proc/341/task/341/smaps: Permission denied
grep: /proc/341/task/341/pagemap: Permission denied
grep: /proc/341/task/341/stack: Permission denied
grep: /proc/341/task/341/io: Permission denied
grep: /proc/341/task/341/patch_state: Permission denied
grep: /proc/341/fd: Permission denied
grep: /proc/341/map_files: Permission denied
grep: /proc/341/fdinfo: Permission denied
grep: /proc/341/ns: Permission denied
grep: /proc/341/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/341/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/341/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/341/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/341/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/341/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/341/net/nf_conntrack: Permission denied
grep: /proc/341/net/ip_tables_names: Permission denied
grep: /proc/341/net/ip6_tables_names: Permission denied
grep: /proc/341/net/ip_tables_matches: Permission denied
grep: /proc/341/net/ip_tables_targets: Permission denied
grep: /proc/341/net/ip6_tables_matches: Permission denied
grep: /proc/341/net/ip6_tables_targets: Permission denied
grep: /proc/341/net/nf_conntrack_expect: Permission denied
grep: /proc/341/environ: Permission denied
grep: /proc/341/auxv: Permission denied
grep: /proc/341/personality: Operation not permitted
grep: /proc/341/syscall: Operation not permitted
grep: /proc/341/maps: Permission denied
grep: /proc/341/numa_maps: Permission denied
grep: /proc/341/mem: Permission denied
grep: /proc/341/mountstats: Permission denied
grep: /proc/341/clear_refs: Permission denied
grep: /proc/341/smaps: Permission denied
grep: /proc/341/pagemap: Permission denied
grep: /proc/341/stack: Permission denied
grep: /proc/341/io: Permission denied
grep: /proc/341/patch_state: Permission denied
grep: /proc/381/task/381/fd: Permission denied
grep: /proc/381/task/381/fdinfo: Permission denied
grep: /proc/381/task/381/ns: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/381/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/381/net/nf_conntrack: Permission denied
grep: /proc/381/task/381/net/ip_tables_names: Permission denied
grep: /proc/381/task/381/net/ip6_tables_names: Permission denied
grep: /proc/381/task/381/net/ip_tables_matches: Permission denied
grep: /proc/381/task/381/net/ip_tables_targets: Permission denied
grep: /proc/381/task/381/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/381/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/381/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/381/environ: Permission denied
grep: /proc/381/task/381/auxv: Permission denied
grep: /proc/381/task/381/personality: Operation not permitted
grep: /proc/381/task/381/syscall: Operation not permitted
grep: /proc/381/task/381/maps: Permission denied
grep: /proc/381/task/381/numa_maps: Permission denied
grep: /proc/381/task/381/mem: Permission denied
grep: /proc/381/task/381/clear_refs: Permission denied
grep: /proc/381/task/381/smaps: Permission denied
grep: /proc/381/task/381/pagemap: Permission denied
grep: /proc/381/task/381/stack: Permission denied
grep: /proc/381/task/381/io: Permission denied
grep: /proc/381/task/381/patch_state: Permission denied
grep: /proc/381/task/403/fd: Permission denied
grep: /proc/381/task/403/fdinfo: Permission denied
grep: /proc/381/task/403/ns: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/403/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/403/net/nf_conntrack: Permission denied
grep: /proc/381/task/403/net/ip_tables_names: Permission denied
grep: /proc/381/task/403/net/ip6_tables_names: Permission denied
grep: /proc/381/task/403/net/ip_tables_matches: Permission denied
grep: /proc/381/task/403/net/ip_tables_targets: Permission denied
grep: /proc/381/task/403/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/403/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/403/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/403/environ: Permission denied
grep: /proc/381/task/403/auxv: Permission denied
grep: /proc/381/task/403/personality: Operation not permitted
grep: /proc/381/task/403/syscall: Operation not permitted
grep: /proc/381/task/403/maps: Permission denied
grep: /proc/381/task/403/numa_maps: Permission denied
grep: /proc/381/task/403/mem: Permission denied
grep: /proc/381/task/403/clear_refs: Permission denied
grep: /proc/381/task/403/smaps: Permission denied
grep: /proc/381/task/403/pagemap: Permission denied
grep: /proc/381/task/403/stack: Permission denied
grep: /proc/381/task/403/io: Permission denied
grep: /proc/381/task/403/patch_state: Permission denied
grep: /proc/381/task/404/fd: Permission denied
grep: /proc/381/task/404/fdinfo: Permission denied
grep: /proc/381/task/404/ns: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/404/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/404/net/nf_conntrack: Permission denied
grep: /proc/381/task/404/net/ip_tables_names: Permission denied
grep: /proc/381/task/404/net/ip6_tables_names: Permission denied
grep: /proc/381/task/404/net/ip_tables_matches: Permission denied
grep: /proc/381/task/404/net/ip_tables_targets: Permission denied
grep: /proc/381/task/404/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/404/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/404/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/404/environ: Permission denied
grep: /proc/381/task/404/auxv: Permission denied
grep: /proc/381/task/404/personality: Operation not permitted
grep: /proc/381/task/404/syscall: Operation not permitted
grep: /proc/381/task/404/maps: Permission denied
grep: /proc/381/task/404/numa_maps: Permission denied
grep: /proc/381/task/404/mem: Permission denied
grep: /proc/381/task/404/clear_refs: Permission denied
grep: /proc/381/task/404/smaps: Permission denied
grep: /proc/381/task/404/pagemap: Permission denied
grep: /proc/381/task/404/stack: Permission denied
grep: /proc/381/task/404/io: Permission denied
grep: /proc/381/task/404/patch_state: Permission denied
grep: /proc/381/task/405/fd: Permission denied
grep: /proc/381/task/405/fdinfo: Permission denied
grep: /proc/381/task/405/ns: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/405/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/405/net/nf_conntrack: Permission denied
grep: /proc/381/task/405/net/ip_tables_names: Permission denied
grep: /proc/381/task/405/net/ip6_tables_names: Permission denied
grep: /proc/381/task/405/net/ip_tables_matches: Permission denied
grep: /proc/381/task/405/net/ip_tables_targets: Permission denied
grep: /proc/381/task/405/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/405/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/405/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/405/environ: Permission denied
grep: /proc/381/task/405/auxv: Permission denied
grep: /proc/381/task/405/personality: Operation not permitted
grep: /proc/381/task/405/syscall: Operation not permitted
grep: /proc/381/task/405/maps: Permission denied
grep: /proc/381/task/405/numa_maps: Permission denied
grep: /proc/381/task/405/mem: Permission denied
grep: /proc/381/task/405/clear_refs: Permission denied
grep: /proc/381/task/405/smaps: Permission denied
grep: /proc/381/task/405/pagemap: Permission denied
grep: /proc/381/task/405/stack: Permission denied
grep: /proc/381/task/405/io: Permission denied
grep: /proc/381/task/405/patch_state: Permission denied
grep: /proc/381/task/406/fd: Permission denied
grep: /proc/381/task/406/fdinfo: Permission denied
grep: /proc/381/task/406/ns: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/406/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/406/net/nf_conntrack: Permission denied
grep: /proc/381/task/406/net/ip_tables_names: Permission denied
grep: /proc/381/task/406/net/ip6_tables_names: Permission denied
grep: /proc/381/task/406/net/ip_tables_matches: Permission denied
grep: /proc/381/task/406/net/ip_tables_targets: Permission denied
grep: /proc/381/task/406/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/406/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/406/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/406/environ: Permission denied
grep: /proc/381/task/406/auxv: Permission denied
grep: /proc/381/task/406/personality: Operation not permitted
grep: /proc/381/task/406/syscall: Operation not permitted
grep: /proc/381/task/406/maps: Permission denied
grep: /proc/381/task/406/numa_maps: Permission denied
grep: /proc/381/task/406/mem: Permission denied
grep: /proc/381/task/406/clear_refs: Permission denied
grep: /proc/381/task/406/smaps: Permission denied
grep: /proc/381/task/406/pagemap: Permission denied
grep: /proc/381/task/406/stack: Permission denied
grep: /proc/381/task/406/io: Permission denied
grep: /proc/381/task/406/patch_state: Permission denied
grep: /proc/381/task/407/fd: Permission denied
grep: /proc/381/task/407/fdinfo: Permission denied
grep: /proc/381/task/407/ns: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/407/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/407/net/nf_conntrack: Permission denied
grep: /proc/381/task/407/net/ip_tables_names: Permission denied
grep: /proc/381/task/407/net/ip6_tables_names: Permission denied
grep: /proc/381/task/407/net/ip_tables_matches: Permission denied
grep: /proc/381/task/407/net/ip_tables_targets: Permission denied
grep: /proc/381/task/407/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/407/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/407/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/407/environ: Permission denied
grep: /proc/381/task/407/auxv: Permission denied
grep: /proc/381/task/407/personality: Operation not permitted
grep: /proc/381/task/407/syscall: Operation not permitted
grep: /proc/381/task/407/maps: Permission denied
grep: /proc/381/task/407/numa_maps: Permission denied
grep: /proc/381/task/407/mem: Permission denied
grep: /proc/381/task/407/clear_refs: Permission denied
grep: /proc/381/task/407/smaps: Permission denied
grep: /proc/381/task/407/pagemap: Permission denied
grep: /proc/381/task/407/stack: Permission denied
grep: /proc/381/task/407/io: Permission denied
grep: /proc/381/task/407/patch_state: Permission denied
grep: /proc/381/task/408/fd: Permission denied
grep: /proc/381/task/408/fdinfo: Permission denied
grep: /proc/381/task/408/ns: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/task/408/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/task/408/net/nf_conntrack: Permission denied
grep: /proc/381/task/408/net/ip_tables_names: Permission denied
grep: /proc/381/task/408/net/ip6_tables_names: Permission denied
grep: /proc/381/task/408/net/ip_tables_matches: Permission denied
grep: /proc/381/task/408/net/ip_tables_targets: Permission denied
grep: /proc/381/task/408/net/ip6_tables_matches: Permission denied
grep: /proc/381/task/408/net/ip6_tables_targets: Permission denied
grep: /proc/381/task/408/net/nf_conntrack_expect: Permission denied
grep: /proc/381/task/408/environ: Permission denied
grep: /proc/381/task/408/auxv: Permission denied
grep: /proc/381/task/408/personality: Operation not permitted
grep: /proc/381/task/408/syscall: Operation not permitted
grep: /proc/381/task/408/maps: Permission denied
grep: /proc/381/task/408/numa_maps: Permission denied
grep: /proc/381/task/408/mem: Permission denied
grep: /proc/381/task/408/clear_refs: Permission denied
grep: /proc/381/task/408/smaps: Permission denied
grep: /proc/381/task/408/pagemap: Permission denied
grep: /proc/381/task/408/stack: Permission denied
grep: /proc/381/task/408/io: Permission denied
grep: /proc/381/task/408/patch_state: Permission denied
grep: /proc/381/fd: Permission denied
grep: /proc/381/map_files: Permission denied
grep: /proc/381/fdinfo: Permission denied
grep: /proc/381/ns: Permission denied
grep: /proc/381/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/381/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/381/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/381/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/381/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/381/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/381/net/nf_conntrack: Permission denied
grep: /proc/381/net/ip_tables_names: Permission denied
grep: /proc/381/net/ip6_tables_names: Permission denied
grep: /proc/381/net/ip_tables_matches: Permission denied
grep: /proc/381/net/ip_tables_targets: Permission denied
grep: /proc/381/net/ip6_tables_matches: Permission denied
grep: /proc/381/net/ip6_tables_targets: Permission denied
grep: /proc/381/net/nf_conntrack_expect: Permission denied
grep: /proc/381/environ: Permission denied
grep: /proc/381/auxv: Permission denied
grep: /proc/381/personality: Operation not permitted
grep: /proc/381/syscall: Operation not permitted
grep: /proc/381/maps: Permission denied
grep: /proc/381/numa_maps: Permission denied
grep: /proc/381/mem: Permission denied
grep: /proc/381/mountstats: Permission denied
grep: /proc/381/clear_refs: Permission denied
grep: /proc/381/smaps: Permission denied
grep: /proc/381/pagemap: Permission denied
grep: /proc/381/stack: Permission denied
grep: /proc/381/io: Permission denied
grep: /proc/381/patch_state: Permission denied
grep: /proc/385/task/385/fd: Permission denied
grep: /proc/385/task/385/fdinfo: Permission denied
grep: /proc/385/task/385/ns: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/385/task/385/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/385/task/385/net/nf_conntrack: Permission denied
grep: /proc/385/task/385/net/ip_tables_names: Permission denied
grep: /proc/385/task/385/net/ip6_tables_names: Permission denied
grep: /proc/385/task/385/net/ip_tables_matches: Permission denied
grep: /proc/385/task/385/net/ip_tables_targets: Permission denied
grep: /proc/385/task/385/net/ip6_tables_matches: Permission denied
grep: /proc/385/task/385/net/ip6_tables_targets: Permission denied
grep: /proc/385/task/385/net/nf_conntrack_expect: Permission denied
grep: /proc/385/task/385/environ: Permission denied
grep: /proc/385/task/385/auxv: Permission denied
grep: /proc/385/task/385/personality: Operation not permitted
grep: /proc/385/task/385/syscall: Operation not permitted
grep: /proc/385/task/385/maps: Permission denied
grep: /proc/385/task/385/numa_maps: Permission denied
grep: /proc/385/task/385/mem: Permission denied
grep: /proc/385/task/385/clear_refs: Permission denied
grep: /proc/385/task/385/smaps: Permission denied
grep: /proc/385/task/385/pagemap: Permission denied
grep: /proc/385/task/385/stack: Permission denied
grep: /proc/385/task/385/io: Permission denied
grep: /proc/385/task/385/patch_state: Permission denied
grep: /proc/385/fd: Permission denied
grep: /proc/385/map_files: Permission denied
grep: /proc/385/fdinfo: Permission denied
grep: /proc/385/ns: Permission denied
grep: /proc/385/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/385/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/385/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/385/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/385/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/385/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/385/net/nf_conntrack: Permission denied
grep: /proc/385/net/ip_tables_names: Permission denied
grep: /proc/385/net/ip6_tables_names: Permission denied
grep: /proc/385/net/ip_tables_matches: Permission denied
grep: /proc/385/net/ip_tables_targets: Permission denied
grep: /proc/385/net/ip6_tables_matches: Permission denied
grep: /proc/385/net/ip6_tables_targets: Permission denied
grep: /proc/385/net/nf_conntrack_expect: Permission denied
grep: /proc/385/environ: Permission denied
grep: /proc/385/auxv: Permission denied
grep: /proc/385/personality: Operation not permitted
grep: /proc/385/syscall: Operation not permitted
grep: /proc/385/maps: Permission denied
grep: /proc/385/numa_maps: Permission denied
grep: /proc/385/mem: Permission denied
grep: /proc/385/mountstats: Permission denied
grep: /proc/385/clear_refs: Permission denied
grep: /proc/385/smaps: Permission denied
grep: /proc/385/pagemap: Permission denied
grep: /proc/385/stack: Permission denied
grep: /proc/385/io: Permission denied
grep: /proc/385/patch_state: Permission denied
grep: /proc/393/task/393/fd: Permission denied
grep: /proc/393/task/393/fdinfo: Permission denied
grep: /proc/393/task/393/ns: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/393/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/393/net/nf_conntrack: Permission denied
grep: /proc/393/task/393/net/ip_tables_names: Permission denied
grep: /proc/393/task/393/net/ip6_tables_names: Permission denied
grep: /proc/393/task/393/net/ip_tables_matches: Permission denied
grep: /proc/393/task/393/net/ip_tables_targets: Permission denied
grep: /proc/393/task/393/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/393/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/393/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/393/environ: Permission denied
grep: /proc/393/task/393/auxv: Permission denied
grep: /proc/393/task/393/personality: Operation not permitted
grep: /proc/393/task/393/syscall: Operation not permitted
grep: /proc/393/task/393/maps: Permission denied
grep: /proc/393/task/393/numa_maps: Permission denied
grep: /proc/393/task/393/mem: Permission denied
grep: /proc/393/task/393/clear_refs: Permission denied
grep: /proc/393/task/393/smaps: Permission denied
grep: /proc/393/task/393/pagemap: Permission denied
grep: /proc/393/task/393/stack: Permission denied
grep: /proc/393/task/393/io: Permission denied
grep: /proc/393/task/393/patch_state: Permission denied
grep: /proc/393/task/394/fd: Permission denied
grep: /proc/393/task/394/fdinfo: Permission denied
grep: /proc/393/task/394/ns: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/394/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/394/net/nf_conntrack: Permission denied
grep: /proc/393/task/394/net/ip_tables_names: Permission denied
grep: /proc/393/task/394/net/ip6_tables_names: Permission denied
grep: /proc/393/task/394/net/ip_tables_matches: Permission denied
grep: /proc/393/task/394/net/ip_tables_targets: Permission denied
grep: /proc/393/task/394/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/394/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/394/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/394/environ: Permission denied
grep: /proc/393/task/394/auxv: Permission denied
grep: /proc/393/task/394/personality: Operation not permitted
grep: /proc/393/task/394/syscall: Operation not permitted
grep: /proc/393/task/394/maps: Permission denied
grep: /proc/393/task/394/numa_maps: Permission denied
grep: /proc/393/task/394/mem: Permission denied
grep: /proc/393/task/394/clear_refs: Permission denied
grep: /proc/393/task/394/smaps: Permission denied
grep: /proc/393/task/394/pagemap: Permission denied
grep: /proc/393/task/394/stack: Permission denied
grep: /proc/393/task/394/io: Permission denied
grep: /proc/393/task/394/patch_state: Permission denied
grep: /proc/393/task/395/fd: Permission denied
grep: /proc/393/task/395/fdinfo: Permission denied
grep: /proc/393/task/395/ns: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/395/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/395/net/nf_conntrack: Permission denied
grep: /proc/393/task/395/net/ip_tables_names: Permission denied
grep: /proc/393/task/395/net/ip6_tables_names: Permission denied
grep: /proc/393/task/395/net/ip_tables_matches: Permission denied
grep: /proc/393/task/395/net/ip_tables_targets: Permission denied
grep: /proc/393/task/395/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/395/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/395/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/395/environ: Permission denied
grep: /proc/393/task/395/auxv: Permission denied
grep: /proc/393/task/395/personality: Operation not permitted
grep: /proc/393/task/395/syscall: Operation not permitted
grep: /proc/393/task/395/maps: Permission denied
grep: /proc/393/task/395/numa_maps: Permission denied
grep: /proc/393/task/395/mem: Permission denied
grep: /proc/393/task/395/clear_refs: Permission denied
grep: /proc/393/task/395/smaps: Permission denied
grep: /proc/393/task/395/pagemap: Permission denied
grep: /proc/393/task/395/stack: Permission denied
grep: /proc/393/task/395/io: Permission denied
grep: /proc/393/task/395/patch_state: Permission denied
grep: /proc/393/task/396/fd: Permission denied
grep: /proc/393/task/396/fdinfo: Permission denied
grep: /proc/393/task/396/ns: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/396/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/396/net/nf_conntrack: Permission denied
grep: /proc/393/task/396/net/ip_tables_names: Permission denied
grep: /proc/393/task/396/net/ip6_tables_names: Permission denied
grep: /proc/393/task/396/net/ip_tables_matches: Permission denied
grep: /proc/393/task/396/net/ip_tables_targets: Permission denied
grep: /proc/393/task/396/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/396/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/396/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/396/environ: Permission denied
grep: /proc/393/task/396/auxv: Permission denied
grep: /proc/393/task/396/personality: Operation not permitted
grep: /proc/393/task/396/syscall: Operation not permitted
grep: /proc/393/task/396/maps: Permission denied
grep: /proc/393/task/396/numa_maps: Permission denied
grep: /proc/393/task/396/mem: Permission denied
grep: /proc/393/task/396/clear_refs: Permission denied
grep: /proc/393/task/396/smaps: Permission denied
grep: /proc/393/task/396/pagemap: Permission denied
grep: /proc/393/task/396/stack: Permission denied
grep: /proc/393/task/396/io: Permission denied
grep: /proc/393/task/396/patch_state: Permission denied
grep: /proc/393/task/397/fd: Permission denied
grep: /proc/393/task/397/fdinfo: Permission denied
grep: /proc/393/task/397/ns: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/397/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/397/net/nf_conntrack: Permission denied
grep: /proc/393/task/397/net/ip_tables_names: Permission denied
grep: /proc/393/task/397/net/ip6_tables_names: Permission denied
grep: /proc/393/task/397/net/ip_tables_matches: Permission denied
grep: /proc/393/task/397/net/ip_tables_targets: Permission denied
grep: /proc/393/task/397/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/397/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/397/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/397/environ: Permission denied
grep: /proc/393/task/397/auxv: Permission denied
grep: /proc/393/task/397/personality: Operation not permitted
grep: /proc/393/task/397/syscall: Operation not permitted
grep: /proc/393/task/397/maps: Permission denied
grep: /proc/393/task/397/numa_maps: Permission denied
grep: /proc/393/task/397/mem: Permission denied
grep: /proc/393/task/397/clear_refs: Permission denied
grep: /proc/393/task/397/smaps: Permission denied
grep: /proc/393/task/397/pagemap: Permission denied
grep: /proc/393/task/397/stack: Permission denied
grep: /proc/393/task/397/io: Permission denied
grep: /proc/393/task/397/patch_state: Permission denied
grep: /proc/393/task/398/fd: Permission denied
grep: /proc/393/task/398/fdinfo: Permission denied
grep: /proc/393/task/398/ns: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/task/398/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/task/398/net/nf_conntrack: Permission denied
grep: /proc/393/task/398/net/ip_tables_names: Permission denied
grep: /proc/393/task/398/net/ip6_tables_names: Permission denied
grep: /proc/393/task/398/net/ip_tables_matches: Permission denied
grep: /proc/393/task/398/net/ip_tables_targets: Permission denied
grep: /proc/393/task/398/net/ip6_tables_matches: Permission denied
grep: /proc/393/task/398/net/ip6_tables_targets: Permission denied
grep: /proc/393/task/398/net/nf_conntrack_expect: Permission denied
grep: /proc/393/task/398/environ: Permission denied
grep: /proc/393/task/398/auxv: Permission denied
grep: /proc/393/task/398/personality: Operation not permitted
grep: /proc/393/task/398/syscall: Operation not permitted
grep: /proc/393/task/398/maps: Permission denied
grep: /proc/393/task/398/numa_maps: Permission denied
grep: /proc/393/task/398/mem: Permission denied
grep: /proc/393/task/398/clear_refs: Permission denied
grep: /proc/393/task/398/smaps: Permission denied
grep: /proc/393/task/398/pagemap: Permission denied
grep: /proc/393/task/398/stack: Permission denied
grep: /proc/393/task/398/io: Permission denied
grep: /proc/393/task/398/patch_state: Permission denied
grep: /proc/393/fd: Permission denied
grep: /proc/393/map_files: Permission denied
grep: /proc/393/fdinfo: Permission denied
grep: /proc/393/ns: Permission denied
grep: /proc/393/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/393/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/393/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/393/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/393/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/393/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/393/net/nf_conntrack: Permission denied
grep: /proc/393/net/ip_tables_names: Permission denied
grep: /proc/393/net/ip6_tables_names: Permission denied
grep: /proc/393/net/ip_tables_matches: Permission denied
grep: /proc/393/net/ip_tables_targets: Permission denied
grep: /proc/393/net/ip6_tables_matches: Permission denied
grep: /proc/393/net/ip6_tables_targets: Permission denied
grep: /proc/393/net/nf_conntrack_expect: Permission denied
grep: /proc/393/environ: Permission denied
grep: /proc/393/auxv: Permission denied
grep: /proc/393/personality: Operation not permitted
grep: /proc/393/syscall: Operation not permitted
grep: /proc/393/maps: Permission denied
grep: /proc/393/numa_maps: Permission denied
grep: /proc/393/mem: Permission denied
grep: /proc/393/mountstats: Permission denied
grep: /proc/393/clear_refs: Permission denied
grep: /proc/393/smaps: Permission denied
grep: /proc/393/pagemap: Permission denied
grep: /proc/393/stack: Permission denied
grep: /proc/393/io: Permission denied
grep: /proc/393/patch_state: Permission denied
grep: /proc/401/task/401/fd: Permission denied
grep: /proc/401/task/401/fdinfo: Permission denied
grep: /proc/401/task/401/ns: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/401/task/401/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/401/task/401/net/nf_conntrack: Permission denied
grep: /proc/401/task/401/net/ip_tables_names: Permission denied
grep: /proc/401/task/401/net/ip6_tables_names: Permission denied
grep: /proc/401/task/401/net/ip_tables_matches: Permission denied
grep: /proc/401/task/401/net/ip_tables_targets: Permission denied
grep: /proc/401/task/401/net/ip6_tables_matches: Permission denied
grep: /proc/401/task/401/net/ip6_tables_targets: Permission denied
grep: /proc/401/task/401/net/nf_conntrack_expect: Permission denied
grep: /proc/401/task/401/environ: Permission denied
grep: /proc/401/task/401/auxv: Permission denied
grep: /proc/401/task/401/personality: Operation not permitted
grep: /proc/401/task/401/syscall: Operation not permitted
grep: /proc/401/task/401/maps: Permission denied
grep: /proc/401/task/401/numa_maps: Permission denied
grep: /proc/401/task/401/mem: Permission denied
grep: /proc/401/task/401/clear_refs: Permission denied
grep: /proc/401/task/401/smaps: Permission denied
grep: /proc/401/task/401/pagemap: Permission denied
grep: /proc/401/task/401/stack: Permission denied
grep: /proc/401/task/401/io: Permission denied
grep: /proc/401/task/401/patch_state: Permission denied
grep: /proc/401/fd: Permission denied
grep: /proc/401/map_files: Permission denied
grep: /proc/401/fdinfo: Permission denied
grep: /proc/401/ns: Permission denied
grep: /proc/401/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/401/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/401/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/401/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/401/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/401/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/401/net/nf_conntrack: Permission denied
grep: /proc/401/net/ip_tables_names: Permission denied
grep: /proc/401/net/ip6_tables_names: Permission denied
grep: /proc/401/net/ip_tables_matches: Permission denied
grep: /proc/401/net/ip_tables_targets: Permission denied
grep: /proc/401/net/ip6_tables_matches: Permission denied
grep: /proc/401/net/ip6_tables_targets: Permission denied
grep: /proc/401/net/nf_conntrack_expect: Permission denied
grep: /proc/401/environ: Permission denied
grep: /proc/401/auxv: Permission denied
grep: /proc/401/personality: Operation not permitted
grep: /proc/401/syscall: Operation not permitted
grep: /proc/401/maps: Permission denied
grep: /proc/401/numa_maps: Permission denied
grep: /proc/401/mem: Permission denied
grep: /proc/401/mountstats: Permission denied
grep: /proc/401/clear_refs: Permission denied
grep: /proc/401/smaps: Permission denied
grep: /proc/401/pagemap: Permission denied
grep: /proc/401/stack: Permission denied
grep: /proc/401/io: Permission denied
grep: /proc/401/patch_state: Permission denied
grep: /proc/402/task/402/fd: Permission denied
grep: /proc/402/task/402/fdinfo: Permission denied
grep: /proc/402/task/402/ns: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/402/task/402/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/402/task/402/net/nf_conntrack: Permission denied
grep: /proc/402/task/402/net/ip_tables_names: Permission denied
grep: /proc/402/task/402/net/ip6_tables_names: Permission denied
grep: /proc/402/task/402/net/ip_tables_matches: Permission denied
grep: /proc/402/task/402/net/ip_tables_targets: Permission denied
grep: /proc/402/task/402/net/ip6_tables_matches: Permission denied
grep: /proc/402/task/402/net/ip6_tables_targets: Permission denied
grep: /proc/402/task/402/net/nf_conntrack_expect: Permission denied
grep: /proc/402/task/402/environ: Permission denied
grep: /proc/402/task/402/auxv: Permission denied
grep: /proc/402/task/402/personality: Operation not permitted
grep: /proc/402/task/402/syscall: Operation not permitted
grep: /proc/402/task/402/maps: Permission denied
grep: /proc/402/task/402/numa_maps: Permission denied
grep: /proc/402/task/402/mem: Permission denied
grep: /proc/402/task/402/clear_refs: Permission denied
grep: /proc/402/task/402/smaps: Permission denied
grep: /proc/402/task/402/pagemap: Permission denied
grep: /proc/402/task/402/stack: Permission denied
grep: /proc/402/task/402/io: Permission denied
grep: /proc/402/task/402/patch_state: Permission denied
grep: /proc/402/fd: Permission denied
grep: /proc/402/map_files: Permission denied
grep: /proc/402/fdinfo: Permission denied
grep: /proc/402/ns: Permission denied
grep: /proc/402/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/402/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/402/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/402/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/402/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/402/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/402/net/nf_conntrack: Permission denied
grep: /proc/402/net/ip_tables_names: Permission denied
grep: /proc/402/net/ip6_tables_names: Permission denied
grep: /proc/402/net/ip_tables_matches: Permission denied
grep: /proc/402/net/ip_tables_targets: Permission denied
grep: /proc/402/net/ip6_tables_matches: Permission denied
grep: /proc/402/net/ip6_tables_targets: Permission denied
grep: /proc/402/net/nf_conntrack_expect: Permission denied
grep: /proc/402/environ: Permission denied
grep: /proc/402/auxv: Permission denied
grep: /proc/402/personality: Operation not permitted
grep: /proc/402/syscall: Operation not permitted
grep: /proc/402/maps: Permission denied
grep: /proc/402/numa_maps: Permission denied
grep: /proc/402/mem: Permission denied
grep: /proc/402/mountstats: Permission denied
grep: /proc/402/clear_refs: Permission denied
grep: /proc/402/smaps: Permission denied
grep: /proc/402/pagemap: Permission denied
grep: /proc/402/stack: Permission denied
grep: /proc/402/io: Permission denied
grep: /proc/402/patch_state: Permission denied
grep: /proc/410/task/410/fd: Permission denied
grep: /proc/410/task/410/fdinfo: Permission denied
grep: /proc/410/task/410/ns: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/410/task/410/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/410/task/410/net/nf_conntrack: Permission denied
grep: /proc/410/task/410/net/ip_tables_names: Permission denied
grep: /proc/410/task/410/net/ip6_tables_names: Permission denied
grep: /proc/410/task/410/net/ip_tables_matches: Permission denied
grep: /proc/410/task/410/net/ip_tables_targets: Permission denied
grep: /proc/410/task/410/net/ip6_tables_matches: Permission denied
grep: /proc/410/task/410/net/ip6_tables_targets: Permission denied
grep: /proc/410/task/410/net/nf_conntrack_expect: Permission denied
grep: /proc/410/task/410/environ: Permission denied
grep: /proc/410/task/410/auxv: Permission denied
grep: /proc/410/task/410/personality: Operation not permitted
grep: /proc/410/task/410/syscall: Operation not permitted
grep: /proc/410/task/410/maps: Permission denied
grep: /proc/410/task/410/numa_maps: Permission denied
grep: /proc/410/task/410/mem: Permission denied
grep: /proc/410/task/410/clear_refs: Permission denied
grep: /proc/410/task/410/smaps: Permission denied
grep: /proc/410/task/410/pagemap: Permission denied
grep: /proc/410/task/410/stack: Permission denied
grep: /proc/410/task/410/io: Permission denied
grep: /proc/410/task/410/patch_state: Permission denied
grep: /proc/410/task/539/fd: Permission denied
grep: /proc/410/task/539/fdinfo: Permission denied
grep: /proc/410/task/539/ns: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/410/task/539/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/410/task/539/net/nf_conntrack: Permission denied
grep: /proc/410/task/539/net/ip_tables_names: Permission denied
grep: /proc/410/task/539/net/ip6_tables_names: Permission denied
grep: /proc/410/task/539/net/ip_tables_matches: Permission denied
grep: /proc/410/task/539/net/ip_tables_targets: Permission denied
grep: /proc/410/task/539/net/ip6_tables_matches: Permission denied
grep: /proc/410/task/539/net/ip6_tables_targets: Permission denied
grep: /proc/410/task/539/net/nf_conntrack_expect: Permission denied
grep: /proc/410/task/539/environ: Permission denied
grep: /proc/410/task/539/auxv: Permission denied
grep: /proc/410/task/539/personality: Operation not permitted
grep: /proc/410/task/539/syscall: Operation not permitted
grep: /proc/410/task/539/maps: Permission denied
grep: /proc/410/task/539/numa_maps: Permission denied
grep: /proc/410/task/539/mem: Permission denied
grep: /proc/410/task/539/clear_refs: Permission denied
grep: /proc/410/task/539/smaps: Permission denied
grep: /proc/410/task/539/pagemap: Permission denied
grep: /proc/410/task/539/stack: Permission denied
grep: /proc/410/task/539/io: Permission denied
grep: /proc/410/task/539/patch_state: Permission denied
grep: /proc/410/fd: Permission denied
grep: /proc/410/map_files: Permission denied
grep: /proc/410/fdinfo: Permission denied
grep: /proc/410/ns: Permission denied
grep: /proc/410/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/410/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/410/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/410/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/410/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/410/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/410/net/nf_conntrack: Permission denied
grep: /proc/410/net/ip_tables_names: Permission denied
grep: /proc/410/net/ip6_tables_names: Permission denied
grep: /proc/410/net/ip_tables_matches: Permission denied
grep: /proc/410/net/ip_tables_targets: Permission denied
grep: /proc/410/net/ip6_tables_matches: Permission denied
grep: /proc/410/net/ip6_tables_targets: Permission denied
grep: /proc/410/net/nf_conntrack_expect: Permission denied
grep: /proc/410/environ: Permission denied
grep: /proc/410/auxv: Permission denied
grep: /proc/410/personality: Operation not permitted
grep: /proc/410/syscall: Operation not permitted
grep: /proc/410/maps: Permission denied
grep: /proc/410/numa_maps: Permission denied
grep: /proc/410/mem: Permission denied
grep: /proc/410/mountstats: Permission denied
grep: /proc/410/clear_refs: Permission denied
grep: /proc/410/smaps: Permission denied
grep: /proc/410/pagemap: Permission denied
grep: /proc/410/stack: Permission denied
grep: /proc/410/io: Permission denied
grep: /proc/410/patch_state: Permission denied
grep: /proc/785/task/785/fd: Permission denied
grep: /proc/785/task/785/fdinfo: Permission denied
grep: /proc/785/task/785/ns: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/785/task/785/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/785/task/785/net/nf_conntrack: Permission denied
grep: /proc/785/task/785/net/ip_tables_names: Permission denied
grep: /proc/785/task/785/net/ip6_tables_names: Permission denied
grep: /proc/785/task/785/net/ip_tables_matches: Permission denied
grep: /proc/785/task/785/net/ip_tables_targets: Permission denied
grep: /proc/785/task/785/net/ip6_tables_matches: Permission denied
grep: /proc/785/task/785/net/ip6_tables_targets: Permission denied
grep: /proc/785/task/785/net/nf_conntrack_expect: Permission denied
grep: /proc/785/task/785/environ: Permission denied
grep: /proc/785/task/785/auxv: Permission denied
grep: /proc/785/task/785/personality: Operation not permitted
grep: /proc/785/task/785/syscall: Operation not permitted
grep: /proc/785/task/785/maps: Permission denied
grep: /proc/785/task/785/numa_maps: Permission denied
grep: /proc/785/task/785/mem: Permission denied
grep: /proc/785/task/785/clear_refs: Permission denied
grep: /proc/785/task/785/smaps: Permission denied
grep: /proc/785/task/785/pagemap: Permission denied
grep: /proc/785/task/785/stack: Permission denied
grep: /proc/785/task/785/io: Permission denied
grep: /proc/785/task/785/patch_state: Permission denied
grep: /proc/785/task/793/fd: Permission denied
grep: /proc/785/task/793/fdinfo: Permission denied
grep: /proc/785/task/793/ns: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/785/task/793/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/785/task/793/net/nf_conntrack: Permission denied
grep: /proc/785/task/793/net/ip_tables_names: Permission denied
grep: /proc/785/task/793/net/ip6_tables_names: Permission denied
grep: /proc/785/task/793/net/ip_tables_matches: Permission denied
grep: /proc/785/task/793/net/ip_tables_targets: Permission denied
grep: /proc/785/task/793/net/ip6_tables_matches: Permission denied
grep: /proc/785/task/793/net/ip6_tables_targets: Permission denied
grep: /proc/785/task/793/net/nf_conntrack_expect: Permission denied
grep: /proc/785/task/793/environ: Permission denied
grep: /proc/785/task/793/auxv: Permission denied
grep: /proc/785/task/793/personality: Operation not permitted
grep: /proc/785/task/793/syscall: Operation not permitted
grep: /proc/785/task/793/maps: Permission denied
grep: /proc/785/task/793/numa_maps: Permission denied
grep: /proc/785/task/793/mem: Permission denied
grep: /proc/785/task/793/clear_refs: Permission denied
grep: /proc/785/task/793/smaps: Permission denied
grep: /proc/785/task/793/pagemap: Permission denied
grep: /proc/785/task/793/stack: Permission denied
grep: /proc/785/task/793/io: Permission denied
grep: /proc/785/task/793/patch_state: Permission denied
grep: /proc/785/task/794/fd: Permission denied
grep: /proc/785/task/794/fdinfo: Permission denied
grep: /proc/785/task/794/ns: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/785/task/794/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/785/task/794/net/nf_conntrack: Permission denied
grep: /proc/785/task/794/net/ip_tables_names: Permission denied
grep: /proc/785/task/794/net/ip6_tables_names: Permission denied
grep: /proc/785/task/794/net/ip_tables_matches: Permission denied
grep: /proc/785/task/794/net/ip_tables_targets: Permission denied
grep: /proc/785/task/794/net/ip6_tables_matches: Permission denied
grep: /proc/785/task/794/net/ip6_tables_targets: Permission denied
grep: /proc/785/task/794/net/nf_conntrack_expect: Permission denied
grep: /proc/785/task/794/environ: Permission denied
grep: /proc/785/task/794/auxv: Permission denied
grep: /proc/785/task/794/personality: Operation not permitted
grep: /proc/785/task/794/syscall: Operation not permitted
grep: /proc/785/task/794/maps: Permission denied
grep: /proc/785/task/794/numa_maps: Permission denied
grep: /proc/785/task/794/mem: Permission denied
grep: /proc/785/task/794/clear_refs: Permission denied
grep: /proc/785/task/794/smaps: Permission denied
grep: /proc/785/task/794/pagemap: Permission denied
grep: /proc/785/task/794/stack: Permission denied
grep: /proc/785/task/794/io: Permission denied
grep: /proc/785/task/794/patch_state: Permission denied
grep: /proc/785/fd: Permission denied
grep: /proc/785/map_files: Permission denied
grep: /proc/785/fdinfo: Permission denied
grep: /proc/785/ns: Permission denied
grep: /proc/785/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/785/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/785/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/785/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/785/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/785/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/785/net/nf_conntrack: Permission denied
grep: /proc/785/net/ip_tables_names: Permission denied
grep: /proc/785/net/ip6_tables_names: Permission denied
grep: /proc/785/net/ip_tables_matches: Permission denied
grep: /proc/785/net/ip_tables_targets: Permission denied
grep: /proc/785/net/ip6_tables_matches: Permission denied
grep: /proc/785/net/ip6_tables_targets: Permission denied
grep: /proc/785/net/nf_conntrack_expect: Permission denied
grep: /proc/785/environ: Permission denied
grep: /proc/785/auxv: Permission denied
grep: /proc/785/personality: Operation not permitted
grep: /proc/785/syscall: Operation not permitted
grep: /proc/785/maps: Permission denied
grep: /proc/785/numa_maps: Permission denied
grep: /proc/785/mem: Permission denied
grep: /proc/785/mountstats: Permission denied
grep: /proc/785/clear_refs: Permission denied
grep: /proc/785/smaps: Permission denied
grep: /proc/785/pagemap: Permission denied
grep: /proc/785/stack: Permission denied
grep: /proc/785/io: Permission denied
grep: /proc/785/patch_state: Permission denied
grep: /proc/787/task/787/fd: Permission denied
grep: /proc/787/task/787/fdinfo: Permission denied
grep: /proc/787/task/787/ns: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/task/787/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/task/787/net/nf_conntrack: Permission denied
grep: /proc/787/task/787/net/ip_tables_names: Permission denied
grep: /proc/787/task/787/net/ip6_tables_names: Permission denied
grep: /proc/787/task/787/net/ip_tables_matches: Permission denied
grep: /proc/787/task/787/net/ip_tables_targets: Permission denied
grep: /proc/787/task/787/net/ip6_tables_matches: Permission denied
grep: /proc/787/task/787/net/ip6_tables_targets: Permission denied
grep: /proc/787/task/787/net/nf_conntrack_expect: Permission denied
grep: /proc/787/task/787/environ: Permission denied
grep: /proc/787/task/787/auxv: Permission denied
grep: /proc/787/task/787/personality: Operation not permitted
grep: /proc/787/task/787/syscall: Operation not permitted
grep: /proc/787/task/787/maps: Permission denied
grep: /proc/787/task/787/numa_maps: Permission denied
grep: /proc/787/task/787/mem: Permission denied
grep: /proc/787/task/787/clear_refs: Permission denied
grep: /proc/787/task/787/smaps: Permission denied
grep: /proc/787/task/787/pagemap: Permission denied
grep: /proc/787/task/787/stack: Permission denied
grep: /proc/787/task/787/io: Permission denied
grep: /proc/787/task/787/patch_state: Permission denied
grep: /proc/787/task/978/fd: Permission denied
grep: /proc/787/task/978/fdinfo: Permission denied
grep: /proc/787/task/978/ns: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/task/978/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/task/978/net/nf_conntrack: Permission denied
grep: /proc/787/task/978/net/ip_tables_names: Permission denied
grep: /proc/787/task/978/net/ip6_tables_names: Permission denied
grep: /proc/787/task/978/net/ip_tables_matches: Permission denied
grep: /proc/787/task/978/net/ip_tables_targets: Permission denied
grep: /proc/787/task/978/net/ip6_tables_matches: Permission denied
grep: /proc/787/task/978/net/ip6_tables_targets: Permission denied
grep: /proc/787/task/978/net/nf_conntrack_expect: Permission denied
grep: /proc/787/task/978/environ: Permission denied
grep: /proc/787/task/978/auxv: Permission denied
grep: /proc/787/task/978/personality: Operation not permitted
grep: /proc/787/task/978/syscall: Operation not permitted
grep: /proc/787/task/978/maps: Permission denied
grep: /proc/787/task/978/numa_maps: Permission denied
grep: /proc/787/task/978/mem: Permission denied
grep: /proc/787/task/978/clear_refs: Permission denied
grep: /proc/787/task/978/smaps: Permission denied
grep: /proc/787/task/978/pagemap: Permission denied
grep: /proc/787/task/978/stack: Permission denied
grep: /proc/787/task/978/io: Permission denied
grep: /proc/787/task/978/patch_state: Permission denied
grep: /proc/787/task/979/fd: Permission denied
grep: /proc/787/task/979/fdinfo: Permission denied
grep: /proc/787/task/979/ns: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/task/979/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/task/979/net/nf_conntrack: Permission denied
grep: /proc/787/task/979/net/ip_tables_names: Permission denied
grep: /proc/787/task/979/net/ip6_tables_names: Permission denied
grep: /proc/787/task/979/net/ip_tables_matches: Permission denied
grep: /proc/787/task/979/net/ip_tables_targets: Permission denied
grep: /proc/787/task/979/net/ip6_tables_matches: Permission denied
grep: /proc/787/task/979/net/ip6_tables_targets: Permission denied
grep: /proc/787/task/979/net/nf_conntrack_expect: Permission denied
grep: /proc/787/task/979/environ: Permission denied
grep: /proc/787/task/979/auxv: Permission denied
grep: /proc/787/task/979/personality: Operation not permitted
grep: /proc/787/task/979/syscall: Operation not permitted
grep: /proc/787/task/979/maps: Permission denied
grep: /proc/787/task/979/numa_maps: Permission denied
grep: /proc/787/task/979/mem: Permission denied
grep: /proc/787/task/979/clear_refs: Permission denied
grep: /proc/787/task/979/smaps: Permission denied
grep: /proc/787/task/979/pagemap: Permission denied
grep: /proc/787/task/979/stack: Permission denied
grep: /proc/787/task/979/io: Permission denied
grep: /proc/787/task/979/patch_state: Permission denied
grep: /proc/787/task/982/fd: Permission denied
grep: /proc/787/task/982/fdinfo: Permission denied
grep: /proc/787/task/982/ns: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/task/982/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/task/982/net/nf_conntrack: Permission denied
grep: /proc/787/task/982/net/ip_tables_names: Permission denied
grep: /proc/787/task/982/net/ip6_tables_names: Permission denied
grep: /proc/787/task/982/net/ip_tables_matches: Permission denied
grep: /proc/787/task/982/net/ip_tables_targets: Permission denied
grep: /proc/787/task/982/net/ip6_tables_matches: Permission denied
grep: /proc/787/task/982/net/ip6_tables_targets: Permission denied
grep: /proc/787/task/982/net/nf_conntrack_expect: Permission denied
grep: /proc/787/task/982/environ: Permission denied
grep: /proc/787/task/982/auxv: Permission denied
grep: /proc/787/task/982/personality: Operation not permitted
grep: /proc/787/task/982/syscall: Operation not permitted
grep: /proc/787/task/982/maps: Permission denied
grep: /proc/787/task/982/numa_maps: Permission denied
grep: /proc/787/task/982/mem: Permission denied
grep: /proc/787/task/982/clear_refs: Permission denied
grep: /proc/787/task/982/smaps: Permission denied
grep: /proc/787/task/982/pagemap: Permission denied
grep: /proc/787/task/982/stack: Permission denied
grep: /proc/787/task/982/io: Permission denied
grep: /proc/787/task/982/patch_state: Permission denied
grep: /proc/787/task/985/fd: Permission denied
grep: /proc/787/task/985/fdinfo: Permission denied
grep: /proc/787/task/985/ns: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/task/985/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/task/985/net/nf_conntrack: Permission denied
grep: /proc/787/task/985/net/ip_tables_names: Permission denied
grep: /proc/787/task/985/net/ip6_tables_names: Permission denied
grep: /proc/787/task/985/net/ip_tables_matches: Permission denied
grep: /proc/787/task/985/net/ip_tables_targets: Permission denied
grep: /proc/787/task/985/net/ip6_tables_matches: Permission denied
grep: /proc/787/task/985/net/ip6_tables_targets: Permission denied
grep: /proc/787/task/985/net/nf_conntrack_expect: Permission denied
grep: /proc/787/task/985/environ: Permission denied
grep: /proc/787/task/985/auxv: Permission denied
grep: /proc/787/task/985/personality: Operation not permitted
grep: /proc/787/task/985/syscall: Operation not permitted
grep: /proc/787/task/985/maps: Permission denied
grep: /proc/787/task/985/numa_maps: Permission denied
grep: /proc/787/task/985/mem: Permission denied
grep: /proc/787/task/985/clear_refs: Permission denied
grep: /proc/787/task/985/smaps: Permission denied
grep: /proc/787/task/985/pagemap: Permission denied
grep: /proc/787/task/985/stack: Permission denied
grep: /proc/787/task/985/io: Permission denied
grep: /proc/787/task/985/patch_state: Permission denied
grep: /proc/787/fd: Permission denied
grep: /proc/787/map_files: Permission denied
grep: /proc/787/fdinfo: Permission denied
grep: /proc/787/ns: Permission denied
grep: /proc/787/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/787/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/787/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/787/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/787/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/787/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/787/net/nf_conntrack: Permission denied
grep: /proc/787/net/ip_tables_names: Permission denied
grep: /proc/787/net/ip6_tables_names: Permission denied
grep: /proc/787/net/ip_tables_matches: Permission denied
grep: /proc/787/net/ip_tables_targets: Permission denied
grep: /proc/787/net/ip6_tables_matches: Permission denied
grep: /proc/787/net/ip6_tables_targets: Permission denied
grep: /proc/787/net/nf_conntrack_expect: Permission denied
grep: /proc/787/environ: Permission denied
grep: /proc/787/auxv: Permission denied
grep: /proc/787/personality: Operation not permitted
grep: /proc/787/syscall: Operation not permitted
grep: /proc/787/maps: Permission denied
grep: /proc/787/numa_maps: Permission denied
grep: /proc/787/mem: Permission denied
grep: /proc/787/mountstats: Permission denied
grep: /proc/787/clear_refs: Permission denied
grep: /proc/787/smaps: Permission denied
grep: /proc/787/pagemap: Permission denied
grep: /proc/787/stack: Permission denied
grep: /proc/787/io: Permission denied
grep: /proc/787/patch_state: Permission denied
grep: /proc/788/task/788/fd: Permission denied
grep: /proc/788/task/788/fdinfo: Permission denied
grep: /proc/788/task/788/ns: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/788/task/788/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/788/task/788/net/nf_conntrack: Permission denied
grep: /proc/788/task/788/net/ip_tables_names: Permission denied
grep: /proc/788/task/788/net/ip6_tables_names: Permission denied
grep: /proc/788/task/788/net/ip_tables_matches: Permission denied
grep: /proc/788/task/788/net/ip_tables_targets: Permission denied
grep: /proc/788/task/788/net/ip6_tables_matches: Permission denied
grep: /proc/788/task/788/net/ip6_tables_targets: Permission denied
grep: /proc/788/task/788/net/nf_conntrack_expect: Permission denied
grep: /proc/788/task/788/environ: Permission denied
grep: /proc/788/task/788/auxv: Permission denied
grep: /proc/788/task/788/personality: Operation not permitted
grep: /proc/788/task/788/syscall: Operation not permitted
grep: /proc/788/task/788/maps: Permission denied
grep: /proc/788/task/788/numa_maps: Permission denied
grep: /proc/788/task/788/mem: Permission denied
grep: /proc/788/task/788/clear_refs: Permission denied
grep: /proc/788/task/788/smaps: Permission denied
grep: /proc/788/task/788/pagemap: Permission denied
grep: /proc/788/task/788/stack: Permission denied
grep: /proc/788/task/788/io: Permission denied
grep: /proc/788/task/788/patch_state: Permission denied
grep: /proc/788/fd: Permission denied
grep: /proc/788/map_files: Permission denied
grep: /proc/788/fdinfo: Permission denied
grep: /proc/788/ns: Permission denied
grep: /proc/788/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/788/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/788/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/788/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/788/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/788/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/788/net/nf_conntrack: Permission denied
grep: /proc/788/net/ip_tables_names: Permission denied
grep: /proc/788/net/ip6_tables_names: Permission denied
grep: /proc/788/net/ip_tables_matches: Permission denied
grep: /proc/788/net/ip_tables_targets: Permission denied
grep: /proc/788/net/ip6_tables_matches: Permission denied
grep: /proc/788/net/ip6_tables_targets: Permission denied
grep: /proc/788/net/nf_conntrack_expect: Permission denied
grep: /proc/788/environ: Permission denied
grep: /proc/788/auxv: Permission denied
grep: /proc/788/personality: Operation not permitted
grep: /proc/788/syscall: Operation not permitted
grep: /proc/788/maps: Permission denied
grep: /proc/788/numa_maps: Permission denied
grep: /proc/788/mem: Permission denied
grep: /proc/788/mountstats: Permission denied
grep: /proc/788/clear_refs: Permission denied
grep: /proc/788/smaps: Permission denied
grep: /proc/788/pagemap: Permission denied
grep: /proc/788/stack: Permission denied
grep: /proc/788/io: Permission denied
grep: /proc/788/patch_state: Permission denied
grep: /proc/1025/task/1025/fd: Permission denied
grep: /proc/1025/task/1025/fdinfo: Permission denied
grep: /proc/1025/task/1025/ns: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1025/task/1025/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1025/task/1025/net/nf_conntrack: Permission denied
grep: /proc/1025/task/1025/net/ip_tables_names: Permission denied
grep: /proc/1025/task/1025/net/ip6_tables_names: Permission denied
grep: /proc/1025/task/1025/net/ip_tables_matches: Permission denied
grep: /proc/1025/task/1025/net/ip_tables_targets: Permission denied
grep: /proc/1025/task/1025/net/ip6_tables_matches: Permission denied
grep: /proc/1025/task/1025/net/ip6_tables_targets: Permission denied
grep: /proc/1025/task/1025/net/nf_conntrack_expect: Permission denied
grep: /proc/1025/task/1025/environ: Permission denied
grep: /proc/1025/task/1025/auxv: Permission denied
grep: /proc/1025/task/1025/personality: Operation not permitted
grep: /proc/1025/task/1025/syscall: Operation not permitted
grep: /proc/1025/task/1025/maps: Permission denied
grep: /proc/1025/task/1025/numa_maps: Permission denied
grep: /proc/1025/task/1025/mem: Permission denied
grep: /proc/1025/task/1025/clear_refs: Permission denied
grep: /proc/1025/task/1025/smaps: Permission denied
grep: /proc/1025/task/1025/pagemap: Permission denied
grep: /proc/1025/task/1025/stack: Permission denied
grep: /proc/1025/task/1025/io: Permission denied
grep: /proc/1025/task/1025/patch_state: Permission denied
grep: /proc/1025/fd: Permission denied
grep: /proc/1025/map_files: Permission denied
grep: /proc/1025/fdinfo: Permission denied
grep: /proc/1025/ns: Permission denied
grep: /proc/1025/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1025/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1025/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1025/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1025/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1025/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1025/net/nf_conntrack: Permission denied
grep: /proc/1025/net/ip_tables_names: Permission denied
grep: /proc/1025/net/ip6_tables_names: Permission denied
grep: /proc/1025/net/ip_tables_matches: Permission denied
grep: /proc/1025/net/ip_tables_targets: Permission denied
grep: /proc/1025/net/ip6_tables_matches: Permission denied
grep: /proc/1025/net/ip6_tables_targets: Permission denied
grep: /proc/1025/net/nf_conntrack_expect: Permission denied
grep: /proc/1025/environ: Permission denied
grep: /proc/1025/auxv: Permission denied
grep: /proc/1025/personality: Operation not permitted
grep: /proc/1025/syscall: Operation not permitted
grep: /proc/1025/maps: Permission denied
grep: /proc/1025/numa_maps: Permission denied
grep: /proc/1025/mem: Permission denied
grep: /proc/1025/mountstats: Permission denied
grep: /proc/1025/clear_refs: Permission denied
grep: /proc/1025/smaps: Permission denied
grep: /proc/1025/pagemap: Permission denied
grep: /proc/1025/stack: Permission denied
grep: /proc/1025/io: Permission denied
grep: /proc/1025/patch_state: Permission denied
grep: /proc/1027/task/1027/fd: Permission denied
grep: /proc/1027/task/1027/fdinfo: Permission denied
grep: /proc/1027/task/1027/ns: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1027/task/1027/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1027/task/1027/net/nf_conntrack: Permission denied
grep: /proc/1027/task/1027/net/ip_tables_names: Permission denied
grep: /proc/1027/task/1027/net/ip6_tables_names: Permission denied
grep: /proc/1027/task/1027/net/ip_tables_matches: Permission denied
grep: /proc/1027/task/1027/net/ip_tables_targets: Permission denied
grep: /proc/1027/task/1027/net/ip6_tables_matches: Permission denied
grep: /proc/1027/task/1027/net/ip6_tables_targets: Permission denied
grep: /proc/1027/task/1027/net/nf_conntrack_expect: Permission denied
grep: /proc/1027/task/1027/environ: Permission denied
grep: /proc/1027/task/1027/auxv: Permission denied
grep: /proc/1027/task/1027/personality: Operation not permitted
grep: /proc/1027/task/1027/syscall: Operation not permitted
grep: /proc/1027/task/1027/maps: Permission denied
grep: /proc/1027/task/1027/numa_maps: Permission denied
grep: /proc/1027/task/1027/mem: Permission denied
grep: /proc/1027/task/1027/clear_refs: Permission denied
grep: /proc/1027/task/1027/smaps: Permission denied
grep: /proc/1027/task/1027/pagemap: Permission denied
grep: /proc/1027/task/1027/stack: Permission denied
grep: /proc/1027/task/1027/io: Permission denied
grep: /proc/1027/task/1027/patch_state: Permission denied
grep: /proc/1027/fd: Permission denied
grep: /proc/1027/map_files: Permission denied
grep: /proc/1027/fdinfo: Permission denied
grep: /proc/1027/ns: Permission denied
grep: /proc/1027/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1027/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1027/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1027/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1027/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1027/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1027/net/nf_conntrack: Permission denied
grep: /proc/1027/net/ip_tables_names: Permission denied
grep: /proc/1027/net/ip6_tables_names: Permission denied
grep: /proc/1027/net/ip_tables_matches: Permission denied
grep: /proc/1027/net/ip_tables_targets: Permission denied
grep: /proc/1027/net/ip6_tables_matches: Permission denied
grep: /proc/1027/net/ip6_tables_targets: Permission denied
grep: /proc/1027/net/nf_conntrack_expect: Permission denied
grep: /proc/1027/environ: Permission denied
grep: /proc/1027/auxv: Permission denied
grep: /proc/1027/personality: Operation not permitted
grep: /proc/1027/syscall: Operation not permitted
grep: /proc/1027/maps: Permission denied
grep: /proc/1027/numa_maps: Permission denied
grep: /proc/1027/mem: Permission denied
grep: /proc/1027/mountstats: Permission denied
grep: /proc/1027/clear_refs: Permission denied
grep: /proc/1027/smaps: Permission denied
grep: /proc/1027/pagemap: Permission denied
grep: /proc/1027/stack: Permission denied
grep: /proc/1027/io: Permission denied
grep: /proc/1027/patch_state: Permission denied
grep: /proc/1935/task/1935/fd: Permission denied
grep: /proc/1935/task/1935/fdinfo: Permission denied
grep: /proc/1935/task/1935/ns: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1935/task/1935/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1935/task/1935/net/nf_conntrack: Permission denied
grep: /proc/1935/task/1935/net/ip_tables_names: Permission denied
grep: /proc/1935/task/1935/net/ip6_tables_names: Permission denied
grep: /proc/1935/task/1935/net/ip_tables_matches: Permission denied
grep: /proc/1935/task/1935/net/ip_tables_targets: Permission denied
grep: /proc/1935/task/1935/net/ip6_tables_matches: Permission denied
grep: /proc/1935/task/1935/net/ip6_tables_targets: Permission denied
grep: /proc/1935/task/1935/net/nf_conntrack_expect: Permission denied
grep: /proc/1935/task/1935/environ: Permission denied
grep: /proc/1935/task/1935/auxv: Permission denied
grep: /proc/1935/task/1935/personality: Operation not permitted
grep: /proc/1935/task/1935/syscall: Operation not permitted
grep: /proc/1935/task/1935/maps: Permission denied
grep: /proc/1935/task/1935/numa_maps: Permission denied
grep: /proc/1935/task/1935/mem: Permission denied
grep: /proc/1935/task/1935/clear_refs: Permission denied
grep: /proc/1935/task/1935/smaps: Permission denied
grep: /proc/1935/task/1935/pagemap: Permission denied
grep: /proc/1935/task/1935/stack: Permission denied
grep: /proc/1935/task/1935/io: Permission denied
grep: /proc/1935/task/1935/patch_state: Permission denied
grep: /proc/1935/task/1937/fd: Permission denied
grep: /proc/1935/task/1937/fdinfo: Permission denied
grep: /proc/1935/task/1937/ns: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1935/task/1937/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1935/task/1937/net/nf_conntrack: Permission denied
grep: /proc/1935/task/1937/net/ip_tables_names: Permission denied
grep: /proc/1935/task/1937/net/ip6_tables_names: Permission denied
grep: /proc/1935/task/1937/net/ip_tables_matches: Permission denied
grep: /proc/1935/task/1937/net/ip_tables_targets: Permission denied
grep: /proc/1935/task/1937/net/ip6_tables_matches: Permission denied
grep: /proc/1935/task/1937/net/ip6_tables_targets: Permission denied
grep: /proc/1935/task/1937/net/nf_conntrack_expect: Permission denied
grep: /proc/1935/task/1937/environ: Permission denied
grep: /proc/1935/task/1937/auxv: Permission denied
grep: /proc/1935/task/1937/personality: Operation not permitted
grep: /proc/1935/task/1937/syscall: Operation not permitted
grep: /proc/1935/task/1937/maps: Permission denied
grep: /proc/1935/task/1937/numa_maps: Permission denied
grep: /proc/1935/task/1937/mem: Permission denied
grep: /proc/1935/task/1937/clear_refs: Permission denied
grep: /proc/1935/task/1937/smaps: Permission denied
grep: /proc/1935/task/1937/pagemap: Permission denied
grep: /proc/1935/task/1937/stack: Permission denied
grep: /proc/1935/task/1937/io: Permission denied
grep: /proc/1935/task/1937/patch_state: Permission denied
grep: /proc/1935/task/1941/fd: Permission denied
grep: /proc/1935/task/1941/fdinfo: Permission denied
grep: /proc/1935/task/1941/ns: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1935/task/1941/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1935/task/1941/net/nf_conntrack: Permission denied
grep: /proc/1935/task/1941/net/ip_tables_names: Permission denied
grep: /proc/1935/task/1941/net/ip6_tables_names: Permission denied
grep: /proc/1935/task/1941/net/ip_tables_matches: Permission denied
grep: /proc/1935/task/1941/net/ip_tables_targets: Permission denied
grep: /proc/1935/task/1941/net/ip6_tables_matches: Permission denied
grep: /proc/1935/task/1941/net/ip6_tables_targets: Permission denied
grep: /proc/1935/task/1941/net/nf_conntrack_expect: Permission denied
grep: /proc/1935/task/1941/environ: Permission denied
grep: /proc/1935/task/1941/auxv: Permission denied
grep: /proc/1935/task/1941/personality: Operation not permitted
grep: /proc/1935/task/1941/syscall: Operation not permitted
grep: /proc/1935/task/1941/maps: Permission denied
grep: /proc/1935/task/1941/numa_maps: Permission denied
grep: /proc/1935/task/1941/mem: Permission denied
grep: /proc/1935/task/1941/clear_refs: Permission denied
grep: /proc/1935/task/1941/smaps: Permission denied
grep: /proc/1935/task/1941/pagemap: Permission denied
grep: /proc/1935/task/1941/stack: Permission denied
grep: /proc/1935/task/1941/io: Permission denied
grep: /proc/1935/task/1941/patch_state: Permission denied
grep: /proc/1935/fd: Permission denied
grep: /proc/1935/map_files: Permission denied
grep: /proc/1935/fdinfo: Permission denied
grep: /proc/1935/ns: Permission denied
grep: /proc/1935/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1935/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1935/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1935/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1935/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1935/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1935/net/nf_conntrack: Permission denied
grep: /proc/1935/net/ip_tables_names: Permission denied
grep: /proc/1935/net/ip6_tables_names: Permission denied
grep: /proc/1935/net/ip_tables_matches: Permission denied
grep: /proc/1935/net/ip_tables_targets: Permission denied
grep: /proc/1935/net/ip6_tables_matches: Permission denied
grep: /proc/1935/net/ip6_tables_targets: Permission denied
grep: /proc/1935/net/nf_conntrack_expect: Permission denied
grep: /proc/1935/environ: Permission denied
grep: /proc/1935/auxv: Permission denied
grep: /proc/1935/personality: Operation not permitted
grep: /proc/1935/syscall: Operation not permitted
grep: /proc/1935/maps: Permission denied
grep: /proc/1935/numa_maps: Permission denied
grep: /proc/1935/mem: Permission denied
grep: /proc/1935/mountstats: Permission denied
grep: /proc/1935/clear_refs: Permission denied
grep: /proc/1935/smaps: Permission denied
grep: /proc/1935/pagemap: Permission denied
grep: /proc/1935/stack: Permission denied
grep: /proc/1935/io: Permission denied
grep: /proc/1935/patch_state: Permission denied
grep: /proc/1954/task/1954/fd: Permission denied
grep: /proc/1954/task/1954/fdinfo: Permission denied
grep: /proc/1954/task/1954/ns: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1954/task/1954/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1954/task/1954/net/nf_conntrack: Permission denied
grep: /proc/1954/task/1954/net/ip_tables_names: Permission denied
grep: /proc/1954/task/1954/net/ip6_tables_names: Permission denied
grep: /proc/1954/task/1954/net/ip_tables_matches: Permission denied
grep: /proc/1954/task/1954/net/ip_tables_targets: Permission denied
grep: /proc/1954/task/1954/net/ip6_tables_matches: Permission denied
grep: /proc/1954/task/1954/net/ip6_tables_targets: Permission denied
grep: /proc/1954/task/1954/net/nf_conntrack_expect: Permission denied
grep: /proc/1954/task/1954/environ: Permission denied
grep: /proc/1954/task/1954/auxv: Permission denied
grep: /proc/1954/task/1954/personality: Operation not permitted
grep: /proc/1954/task/1954/syscall: Operation not permitted
grep: /proc/1954/task/1954/maps: Permission denied
grep: /proc/1954/task/1954/numa_maps: Permission denied
grep: /proc/1954/task/1954/mem: Permission denied
grep: /proc/1954/task/1954/clear_refs: Permission denied
grep: /proc/1954/task/1954/smaps: Permission denied
grep: /proc/1954/task/1954/pagemap: Permission denied
grep: /proc/1954/task/1954/stack: Permission denied
grep: /proc/1954/task/1954/io: Permission denied
grep: /proc/1954/task/1954/patch_state: Permission denied
grep: /proc/1954/fd: Permission denied
grep: /proc/1954/map_files: Permission denied
grep: /proc/1954/fdinfo: Permission denied
grep: /proc/1954/ns: Permission denied
grep: /proc/1954/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/1954/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/1954/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/1954/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/1954/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/1954/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/1954/net/nf_conntrack: Permission denied
grep: /proc/1954/net/ip_tables_names: Permission denied
grep: /proc/1954/net/ip6_tables_names: Permission denied
grep: /proc/1954/net/ip_tables_matches: Permission denied
grep: /proc/1954/net/ip_tables_targets: Permission denied
grep: /proc/1954/net/ip6_tables_matches: Permission denied
grep: /proc/1954/net/ip6_tables_targets: Permission denied
grep: /proc/1954/net/nf_conntrack_expect: Permission denied
grep: /proc/1954/environ: Permission denied
grep: /proc/1954/auxv: Permission denied
grep: /proc/1954/personality: Operation not permitted
grep: /proc/1954/syscall: Operation not permitted
grep: /proc/1954/maps: Permission denied
grep: /proc/1954/numa_maps: Permission denied
grep: /proc/1954/mem: Permission denied
grep: /proc/1954/mountstats: Permission denied
grep: /proc/1954/clear_refs: Permission denied
grep: /proc/1954/smaps: Permission denied
grep: /proc/1954/pagemap: Permission denied
grep: /proc/1954/stack: Permission denied
grep: /proc/1954/io: Permission denied
grep: /proc/1954/patch_state: Permission denied
grep: /proc/2278/task/2278/fd: Permission denied
grep: /proc/2278/task/2278/fdinfo: Permission denied
grep: /proc/2278/task/2278/ns: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/2278/task/2278/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/2278/task/2278/net/nf_conntrack: Permission denied
grep: /proc/2278/task/2278/net/ip_tables_names: Permission denied
grep: /proc/2278/task/2278/net/ip6_tables_names: Permission denied
grep: /proc/2278/task/2278/net/ip_tables_matches: Permission denied
grep: /proc/2278/task/2278/net/ip_tables_targets: Permission denied
grep: /proc/2278/task/2278/net/ip6_tables_matches: Permission denied
grep: /proc/2278/task/2278/net/ip6_tables_targets: Permission denied
grep: /proc/2278/task/2278/net/nf_conntrack_expect: Permission denied
grep: /proc/2278/task/2278/environ: Permission denied
grep: /proc/2278/task/2278/auxv: Permission denied
grep: /proc/2278/task/2278/personality: Operation not permitted
grep: /proc/2278/task/2278/syscall: Operation not permitted
grep: /proc/2278/task/2278/maps: Permission denied
grep: /proc/2278/task/2278/numa_maps: Permission denied
grep: /proc/2278/task/2278/mem: Permission denied
grep: /proc/2278/task/2278/clear_refs: Permission denied
grep: /proc/2278/task/2278/smaps: Permission denied
grep: /proc/2278/task/2278/pagemap: Permission denied
grep: /proc/2278/task/2278/stack: Permission denied
grep: /proc/2278/task/2278/io: Permission denied
grep: /proc/2278/task/2278/patch_state: Permission denied
grep: /proc/2278/fd: Permission denied
grep: /proc/2278/map_files: Permission denied
grep: /proc/2278/fdinfo: Permission denied
grep: /proc/2278/ns: Permission denied
grep: /proc/2278/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/2278/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/2278/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/2278/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/2278/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/2278/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/2278/net/nf_conntrack: Permission denied
grep: /proc/2278/net/ip_tables_names: Permission denied
grep: /proc/2278/net/ip6_tables_names: Permission denied
grep: /proc/2278/net/ip_tables_matches: Permission denied
grep: /proc/2278/net/ip_tables_targets: Permission denied
grep: /proc/2278/net/ip6_tables_matches: Permission denied
grep: /proc/2278/net/ip6_tables_targets: Permission denied
grep: /proc/2278/net/nf_conntrack_expect: Permission denied
grep: /proc/2278/environ: Permission denied
grep: /proc/2278/auxv: Permission denied
grep: /proc/2278/personality: Operation not permitted
grep: /proc/2278/syscall: Operation not permitted
grep: /proc/2278/maps: Permission denied
grep: /proc/2278/numa_maps: Permission denied
grep: /proc/2278/mem: Permission denied
grep: /proc/2278/mountstats: Permission denied
grep: /proc/2278/clear_refs: Permission denied
grep: /proc/2278/smaps: Permission denied
grep: /proc/2278/pagemap: Permission denied
grep: /proc/2278/stack: Permission denied
grep: /proc/2278/io: Permission denied
grep: /proc/2278/patch_state: Permission denied
grep: /proc/2281/task/2281/fd: Permission denied
grep: /proc/2281/task/2281/fdinfo: Permission denied
grep: /proc/2281/task/2281/ns: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/2281/task/2281/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/2281/task/2281/net/nf_conntrack: Permission denied
grep: /proc/2281/task/2281/net/ip_tables_names: Permission denied
grep: /proc/2281/task/2281/net/ip6_tables_names: Permission denied
grep: /proc/2281/task/2281/net/ip_tables_matches: Permission denied
grep: /proc/2281/task/2281/net/ip_tables_targets: Permission denied
grep: /proc/2281/task/2281/net/ip6_tables_matches: Permission denied
grep: /proc/2281/task/2281/net/ip6_tables_targets: Permission denied
grep: /proc/2281/task/2281/net/nf_conntrack_expect: Permission denied
grep: /proc/2281/task/2281/environ: Permission denied
grep: /proc/2281/task/2281/auxv: Permission denied
grep: /proc/2281/task/2281/personality: Operation not permitted
grep: /proc/2281/task/2281/syscall: Operation not permitted
grep: /proc/2281/task/2281/maps: Permission denied
grep: /proc/2281/task/2281/numa_maps: Permission denied
grep: /proc/2281/task/2281/mem: Permission denied
grep: /proc/2281/task/2281/clear_refs: Permission denied
grep: /proc/2281/task/2281/smaps: Permission denied
grep: /proc/2281/task/2281/pagemap: Permission denied
grep: /proc/2281/task/2281/stack: Permission denied
grep: /proc/2281/task/2281/io: Permission denied
grep: /proc/2281/task/2281/patch_state: Permission denied
grep: /proc/2281/fd: Permission denied
grep: /proc/2281/map_files: Permission denied
grep: /proc/2281/fdinfo: Permission denied
grep: /proc/2281/ns: Permission denied
grep: /proc/2281/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/2281/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/2281/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/2281/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/2281/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/2281/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/2281/net/nf_conntrack: Permission denied
grep: /proc/2281/net/ip_tables_names: Permission denied
grep: /proc/2281/net/ip6_tables_names: Permission denied
grep: /proc/2281/net/ip_tables_matches: Permission denied
grep: /proc/2281/net/ip_tables_targets: Permission denied
grep: /proc/2281/net/ip6_tables_matches: Permission denied
grep: /proc/2281/net/ip6_tables_targets: Permission denied
grep: /proc/2281/net/nf_conntrack_expect: Permission denied
grep: /proc/2281/environ: Permission denied
grep: /proc/2281/auxv: Permission denied
grep: /proc/2281/personality: Operation not permitted
grep: /proc/2281/syscall: Operation not permitted
grep: /proc/2281/maps: Permission denied
grep: /proc/2281/numa_maps: Permission denied
grep: /proc/2281/mem: Permission denied
grep: /proc/2281/mountstats: Permission denied
grep: /proc/2281/clear_refs: Permission denied
grep: /proc/2281/smaps: Permission denied
grep: /proc/2281/pagemap: Permission denied
grep: /proc/2281/stack: Permission denied
grep: /proc/2281/io: Permission denied
grep: /proc/2281/patch_state: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.ip/flush: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.ip/channel: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.ip/content: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.gid/flush: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.gid/channel: Permission denied
grep: /proc/2282/task/2282/net/rpc/auth.unix.gid/content: Permission denied
grep: /proc/2282/task/2282/net/nf_conntrack: Permission denied
grep: /proc/2282/task/2282/net/ip_tables_names: Permission denied
grep: /proc/2282/task/2282/net/ip6_tables_names: Permission denied
grep: /proc/2282/task/2282/net/ip_tables_matches: Permission denied
grep: /proc/2282/task/2282/net/ip_tables_targets: Permission denied
grep: /proc/2282/task/2282/net/ip6_tables_matches: Permission denied
grep: /proc/2282/task/2282/net/ip6_tables_targets: Permission denied
grep: /proc/2282/task/2282/net/nf_conntrack_expect: Permission denied
grep: /proc/2282/task/2282/mem: Input/output error
grep: /proc/2282/task/2282/clear_refs: Permission denied
^C
```
