# BONSOIR AVENTURIER
## Ici tu trouveras plein d'explications qui finalement n'en sont pas vu que rien n'y est expliqué.
## Juste des lignes de commandes retracant tout le travail fait pour atteindre les plus grands sommets.

### P.S. :

![](../images/prank2.png)

## Partie serveur python

```bash
-- Unit pyserveur.service has begun starting up.
Oct 09 19:56:06 tp3 systemd[19202]: Failed at step USER spawning /usr/bin/sudo: No such process
-- Subject: Process /usr/bin/sudo could not be executed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- The process /usr/bin/sudo could not be executed and failed.
--
-- The error number returned by this process is 3.
Oct 09 19:56:06 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 19:56:06 tp3 systemd[19203]: Failed at step USER spawning /usr/bin/sudo: No such process
-- Subject: Process /usr/bin/sudo could not be executed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- The process /usr/bin/sudo could not be executed and failed.
--
-- The error number returned by this process is 3.
Oct 09 19:56:06 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 19:56:06 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 19:56:06 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 19:56:06 tp3 systemd[1]: pyserveur.service failed.
Oct 09 19:56:06 tp3 sudo[19194]: pam_unix(sudo:session): session closed for user root
Oct 09 19:56:06 tp3 polkitd[381]: Unregistered Authentication Agent for unix-process:19196:4516745 (system bus name :1.194, object patOct 09 19:56:18 tp3 sudo[19218]:  vagrant : TTY=pts/0 ; PWD=/etc/systemd/system ; USER=root ; COMMAND=/bin/journalctl -xe
Oct 09 19:56:18 tp3 sudo[19218]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
Oct 09 19:56:32 tp3 sudo[19218]: pam_unix(sudo:session): session closed for user root
Oct 09 19:56:49 tp3 sudo[19248]:  vagrant : TTY=pts/0 ; PWD=/etc/systemd/system ; USER=root ; COMMAND=/bin/yum install python3
Oct 09 19:56:49 tp3 sudo[19248]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
Oct 09 19:57:08 tp3 sudo[19248]: pam_unix(sudo:session): session closed for user root
Oct 09 19:57:11 tp3 sudo[19332]:  vagrant : TTY=pts/0 ; PWD=/etc/systemd/system ; USER=root ; COMMAND=/bin/journalctl -xe
Oct 09 19:57:11 tp3 sudo[19332]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
```

```bash
[claquettechaussette@tp3 ~]$ sudo systemctl start pyserveur

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for claquettechaussette:
Warning: pyserveur.service changed on disk. Run 'systemctl daemon-reload' to reload units.
Job for pyserveur.service failed because the control process exited with error code. See "systemctl status pyserveur.service" and "journalctl -xe" for details.
[claquettechaussette@tp3 ~]$ sudo systemctl daemonreload
Unknown operation 'daemonreload'.
[claquettechaussette@tp3 ~]$ sudo systemctl daemon-reload
[claquettechaussette@tp3 ~]$ sudo systemctl start pyserveur
Job for pyserveur.service failed because the control process exited with error code. See "systemctl status pyserveur.service" and "journalctl -xe" for details.
[claquettechaussette@tp3 ~]$ journalctl -xe
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:03:47 tp3 systemd[19755]: Failed at step USER spawning /usr/bin/sudo: No such process
-- Subject: Process /usr/bin/sudo could not be executed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- The process /usr/bin/sudo could not be executed and failed.
--
-- The error number returned by this process is 3.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[19756]: Failed at step USER spawning /usr/bin/sudo: No such process
-- Subject: Process /usr/bin/sudo could not be executed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- The process /usr/bin/sudo could not be executed and failed.
--
-- The error number returned by this process is 3.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:03:47 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:03:47 tp3 sudo[19743]: pam_unix(sudo:session): session closed for user root
Oct 09 20:03:47 tp3 polkitd[381]: Unregistered Authentication Agent for unix-process:19749:4562876 (system bus name :1.207, object patOct 09 20:04:00 tp3 sudo[19770]: claquettechaussette : TTY=pts/0 ; PWD=/home/claquettechaussette ; USER=root ; COMMAND=/bin/systemctl
Oct 09 20:04:00 tp3 sudo[19770]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
Oct 09 20:04:00 tp3 sudo[19770]: pam_unix(sudo:session): session closed for user root
Oct 09 20:04:05 tp3 sudo[19777]: claquettechaussette : TTY=pts/0 ; PWD=/home/claquettechaussette ; USER=root ; COMMAND=/bin/systemctl
Oct 09 20:04:05 tp3 sudo[19777]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
Oct 09 20:04:05 tp3 polkitd[381]: Registered Authentication Agent for unix-process:19780:4564637 (system bus name :1.210 [/usr/bin/pktOct 09 20:04:05 tp3 systemd[1]: Reloading.
Oct 09 20:04:05 tp3 sudo[19777]: pam_unix(sudo:session): session closed for user root
Oct 09 20:04:05 tp3 polkitd[381]: Unregistered Authentication Agent for unix-process:19780:4564637 (system bus name :1.210, object patOct 09 20:04:08 tp3 sudo[19799]: claquettechaussette : TTY=pts/0 ; PWD=/home/claquettechaussette ; USER=root ; COMMAND=/bin/systemctl
Oct 09 20:04:08 tp3 sudo[19799]: pam_unix(sudo:session): session opened for user root by vagrant(uid=0)
Oct 09 20:04:08 tp3 polkitd[381]: Registered Authentication Agent for unix-process:19801:4564947 (system bus name :1.212 [/usr/bin/pktOct 09 20:04:08 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:08 tp3 sudo[19807]: sudo: no tty present and no askpass program specified
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:09 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:09 tp3 sudo[19810]: sudo: no tty present and no askpass program specified
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:11 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:11 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
```

```bash
[vagrant@tp3 system]$ sudo journalctl -xe -u pyserveur
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 19:56:06 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 19:56:06 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:03:47 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:03:47 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:04:08 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:08 tp3 sudo[19807]: sudo: no tty present and no askpass program specified
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:09 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:09 tp3 sudo[19810]: sudo: no tty present and no askpass program specified
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:11 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:11 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:04:11 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:04:11 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:12:23 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:12:24 tp3 sudo[20307]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd reload
Oct 09 20:12:25 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=2
Oct 09 20:12:25 tp3 sudo[20315]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd --remove-port=8Oct 09 20:12:26 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:12:26 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:12:26 tp3 systemd[1]: pyserveur.service failed.
 ESCOC

evel




ed state.




evel


exited, code=exited status=217
exited, code=exited status=217
.


evel




ed state.




evel


failed
s program specified
t identify password for [claquettechaussette]
exited, code=exited status=1
failed
s program specified
t identify password for [claquettechaussette]
exited, code=exited status=1
.


evel




ed state.




evel


; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd reload
exited, code=exited status=2
; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd --remove-port=8080/tcp
.


evel




ed state.

 ESCOD                                                                                                                                -- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 19:56:06 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 19:56:06 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:03:47 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=217
Oct 09 20:03:47 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:03:47 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:03:47 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:04:08 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:08 tp3 sudo[19807]: sudo: no tty present and no askpass program specified
Oct 09 20:04:08 tp3 sudo[19807]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:09 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): conversation failed
Oct 09 20:04:09 tp3 sudo[19810]: sudo: no tty present and no askpass program specified
Oct 09 20:04:09 tp3 sudo[19810]: pam_unix(sudo:auth): auth could not identify password for [claquettechaussette]
Oct 09 20:04:11 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=1
Oct 09 20:04:11 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:04:11 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:04:11 tp3 systemd[1]: pyserveur.service failed.
Oct 09 20:12:23 tp3 systemd[1]: Starting Un serveur python ?...
-- Subject: Unit pyserveur.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has begun starting up.
Oct 09 20:12:24 tp3 sudo[20307]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd reload
Oct 09 20:12:25 tp3 systemd[1]: pyserveur.service: control process exited, code=exited status=2
Oct 09 20:12:25 tp3 sudo[20315]: claquettechaussette : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/firewall-cmd --remove-port=8Oct 09 20:12:26 tp3 systemd[1]: Failed to start Un serveur python ?.
-- Subject: Unit pyserveur.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pyserveur.service has failed.
--
-- The result is failed.
Oct 09 20:12:26 tp3 systemd[1]: Unit pyserveur.service entered failed state.
Oct 09 20:12:26 tp3 systemd[1]: pyserveur.service failed.
```

`[vagrant@tp3 system]$ sudo systemctl start pyserveur`


```bash
# nouveau terminal
[vagrant@tp3 ~]$ sudo ss -alnpt
State      Recv-Q Send-Q               Local Address:Port                              Peer Address:Port
LISTEN     0      128                              *:111                                          *:*                   users:(("rpcbind",pid=341,fd=8))
LISTEN     0      5                                *:8080                                         *:*                   users:(("python2",pid=20474,fd=3))
LISTEN     0      128                              *:80                                           *:*                   users:(("nginx",pid=2340,fd=6),("nginx",pid=2339,fd=6))
LISTEN     0      128                              *:22                                           *:*                   users:(("sshd",pid=788,fd=3))
LISTEN     0      100                      127.0.0.1:25                                           *:*                   users:(("master",pid=1025,fd=13))
LISTEN     0      128                           [::]:111                                       [::]:*                   users:(("rpcbind",pid=341,fd=11))
LISTEN     0      128                           [::]:80                                        [::]:*                   users:(("nginx",pid=2340,fd=7),("nginx",pid=2339,fd=7))
LISTEN     0      128                           [::]:22                                        [::]:*                   users:(("sshd",pid=788,fd=4))
LISTEN     0      100                          [::1]:25                                        [::]:*                   users:(("master",pid=1025,fd=14))
[vagrant@tp3 ~]$ curl localhost:8080
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
    <title>Directory listing for /</title>
    <body>
        <h2>Directory listing for /</h2>
        <hr>
            <ul>
                <li><a href="bin/">bin@</a>
                <li><a href="boot/">boot/</a>
                <li><a href="dev/">dev/</a>
                <li><a href="etc/">etc/</a>
                <li><a href="home/">home/</a>
                <li><a href="lib/">lib@</a>
                <li><a href="lib64/">lib64@</a>
                <li><a href="media/">media/</a>
                <li><a href="mnt/">mnt/</a>
                <li><a href="opt/">opt/</a>
                <li><a href="proc/">proc/</a>
                <li><a href="root/">root/</a>
                <li><a href="run/">run/</a>
                <li><a href="sbin/">sbin@</a>
                <li><a href="srv/">srv/</a>
                <li><a href="swapfile">swapfile</a>
                <li><a href="sys/">sys/</a>
                <li><a href="tmp/">tmp/</a>
                <li><a href="usr/">usr/</a>
                <li><a href="var/">var/</a>
            </ul>
        <hr>
    </body>
</html>
```

```bash
[vagrant@tp3 system]$ systemctl status pyserveur
● pyserveur.service - Un serveur python ?
   Loaded: loaded (/etc/systemd/system/pyserveur.service; disabled; vendor preset: disabled)
   Active: activating (start) since Fri 2020-10-09 20:13:37 UTC; 3min 29s ago
  Control: 20469 (sudo)
   CGroup: /system.slice/pyserveur.service
           ‣ 20469 /usr/bin/sudo /usr/bin/python2 -m SimpleHTTPServer 8080
[vagrant@tp3 system]$ sudo vim pyserveur.service
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

## Partie Sauvegarde

```bash
[claquettechaussette@tp3 /]$ sudo systemctl cat backup
# /etc/systemd/system/backup.service
[Unit]
Description=Le script de sauvegarde qui fonctionne pas depuis 2 tps

[Service]
ExecStart=/home/claquettechaussette/backup.sh
User=claquettechaussette
Group=wheel
PIDFile=/var/run/backup/pid
Environment=PATH=/usr/bin:/usr/local/bin

[Install]
WantedBy=multi-user.target
[claquettechaussette@tp3 /]$ sudo -u claquettechaussette /home/claquettechaussette/backup.sh
[Oct 14 14:38:17] [ERROR] The target path  does not exist. Exiting.
[claquettechaussette@tp3 /]$ sudo -u claquettechaussette /home/claquettechaussette/backup.sh /srv/site1
[Oct 14 14:38:31] [ERROR] The target path /srv/site1 does not exist. Exiting.
[claquettechaussette@tp3 /]$ sudo -u claquettechaussette /home/claquettechaussette/backup.sh /tmp
[Oct 14 14:38:39] [ERROR] The destination dir /opt/backup/ does not exist. Exiting.
[claquettechaussette@tp3 /]$ mkdir /opt/backup
mkdir: cannot create directory ‘/opt/backup’: Permission denied
[claquettechaussette@tp3 /]$
[claquettechaussette@tp3 /]$ sudo !!
sudo mkdir /opt/backup
[claquettechaussette@tp3 /]$
[claquettechaussette@tp3 /]$ sudo -u claquettechaussette /home/claquettechaussette/backup.sh /tmp
[Oct 14 14:39:31] [ERROR] The destination dir /opt/backup/ is not writable. Exiting.
[claquettechaussette@tp3 /]$ sudo chown claquettechaussette /opt/backup/
[claquettechaussette@tp3 /]$ sudo -u claquettechaussette /home/claquettechaussette/backup.sh /tmp
[Oct 14 14:39:47] [INFO] Starting backup.
[Oct 14 14:39:47] [INFO] Success. Backup tmp_201014_143947.tar.gz has been saved to /opt/backup/.
```