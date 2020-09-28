# TP1 : Déploiement classique

*HEEEEEEEY ! Que c'est cool de se retrouver franchement, ça faisait un bail ! Passé ces retrouvailles dignes de celles d'Avdol et Polnarreff, est ce que tu es prêt à reperdre ton temps sur des comptes rendus avec 666% d'humour ?*

## Prérequis

Bien bien bien... pour ce TP, on va commencer par partitionner un disque random. Alors, c'est marrant de dire tout ça mais oon va partitionner lequel ?

```bash
[vinny@localhost ~]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   10G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    9G  0 part
  ├─centos-root 253:0    0    8G  0 lvm  /
  └─centos-swap 253:1    0    1G  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
sr0              11:0    1 1024M  0 rom
[vinny@localhost ~]$
```

Oh bah tiens tiens tiens, y'a pile un disque de 5go comme tu las demandé sur l'énoncé. Si ça c'est pas un hasard. 

![](./images/niiice.gif)

(on l'a juste ajouté via virtualbox, rien d'affolant.)
Triffouillons donc ce disque.

```bash
[vinny@localhost ~]$ sudo pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
[vinny@localhost ~]$ sudo pvs
  PV         VG     Fmt  Attr PSize  PFree
  /dev/sda2  centos lvm2 a--  <9.00g    0
  /dev/sdb          lvm2 ---   5.00g 5.00g
[vinny@localhost ~]$ sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               <9.00 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              2303
  Free PE               0
  Allocated PE          2303
  PV UUID               2F2INx-Fcal-1YbL-ecjf-5MBD-Lbh2-pr2RAh

  "/dev/sdb" is a new physical volume of "5.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb
  VG Name
  PV Size               5.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               l2Nnb1-7UWe-FBhO-mgd7-RwIh-iayQ-YHRnBf
```

![](./images/transition.JPG)

```bash
[vinny@localhost ~]$ sudo vgcreate serv /dev/sdb
  Volume group "serv" successfully created
[vinny@localhost ~]$ sudo pvs
  PV         VG     Fmt  Attr PSize  PFree
  /dev/sda2  centos lvm2 a--  <9.00g     0
  /dev/sdb   serv   lvm2 a--  <5.00g <5.00g
```

![](./images/transition.JPG)

```bash
[vinny@localhost ~]$ sudo lvcreate -L 2G serv -n site1
  Logical volume "site1" created.
[vinny@localhost ~]$ sudo lvcreate -L 3G serv -n site2
  Volume group "serv" has insufficient free space (767 extents): 768 required.
[vinny@localhost ~]$ sudo lvcreate -L 100%FREE serv -n site2
  Cannnot parse size argument.
  Invalid argument for --size: 100%FREE
  Error during parsing of command line.
[vinny@localhost ~]$ sudo lvcreate -l 100%FREE serv -n site2
  Logical volume "site2" created.
  [vinny@localhost ~]$ sudo lvs
[sudo] password for vinny:
  LV    VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root  centos -wi-ao---- <8.00g
  swap  centos -wi-ao----  1.00g
  site1 serv   -wi-a-----  2.00g
  site2 serv   -wi-a----- <3.00g
```

Une fois que c'est fait on formate le disque avec `sudo mkfs -t ext4 /dev/srv/site1`

Et on mount les partitions avec `mount /dev/serv/site1 /srv/site1` + `mount /dev/serv/site2 /srv/site2`

Et on check:

```bash
[vinny@localhost /]$ mount
[...]
/dev/mapper/serv-site1 on /srv/site1 type ext4 (rw,relatime,seclabel,data=ordered)
/dev/mapper/serv-site2 on /srv/site2 type ext4 (rw,relatime,seclabel,data=ordered)
```

```bash
[vinny@localhost /]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 484M     0  484M   0% /dev
tmpfs                    496M     0  496M   0% /dev/shm
tmpfs                    496M   13M  483M   3% /run
tmpfs                    496M     0  496M   0% /sys/fs/cgroup
/dev/mapper/centos-root  8.0G  1.6G  6.5G  20% /
/dev/sda1               1014M  136M  879M  14% /boot
tmpfs                    100M     0  100M   0% /run/user/1000
/dev/mapper/serv-site1   2.0G  6.0M  1.8G   1% /srv/site1
/dev/mapper/serv-site2   2.9G  9.0M  2.8G   1% /srv/site2
```

Pour clôturer le tout, on rajoute deux lignes dans fstab `>/dev/serv/site1 /srv/site1 ext4 defaults 0 0 >/dev/serv/site2 /srv/site2 ext4 defaults 0 0`

Les prérequis c'est finis. Passons à la partie la plus simple.

## NGINX

Partie simple car ENORMES souvenirs de mon projet infra, tu te souviens celui où je te faisais passer pour un gros naze

![](./images/kuzco.png)

Bon, on installe nginx sur node1, je te retape "les" commandes nécessaires si ce n'est `sudo yum install -y epel-release` et `sudo yum –y install nginx`

Maintenant check ça:

```bash
[vinny@node1 nginx]$ cat nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  node1.tp1.b2;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
                return 301 /site1;
        }

        location /site1 {
                alias /srv/site1
        }

         location /site2 {
                alias /srv/site2
        }


        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}
```

NGINX fonctionne, les bonnes location de server et les bons ports sont opérationnels, node2 peut se connecter au serveur node1.

Pour la permission des dossiers site1 et site2, je me suis permis de les mettres en 704 (700 pour l'admin qui possedera tout les droit sur les fichiers et le 4 pour l'user qui pourra uniquement lire le dossier (on aurait pu mettre 700 mais dans le doute on autorise la lecture du dossier pour le visiteur (messire ! un sarrasin !)))

![](./images/sarazin.gif)

![](./images/transition.JPG)

## Le scripting (...)

Voici une petite représentation de ce que j'ai tenté... en vain.

```sh
#!/bin/bash

backup_files="/srv/site1 /srv/site2"

dest="/home/vinny/backup"

day=$(date +%Y%m%d_%H%M)

filename="$(backup_files)

name="${target_dir}_${day}.tar.gz"

echo "backing up $backup_files to $dest/$name"
date
echo

tar czf $dest$name $backup_files

echo
echo "Backup finished
date

ls -lh $dest

```

Y'a de l'idée tu vas me dire, mais ca fonctionne pas parce que :

```bash
[vinny@node1 ~]$ sudo ./tp1_backup.sh
./tp1_backup.sh: line 13: backup_files: command not found
./tp1_backup.sh: line 13: up: command not found
```

`backupfiles` etant un juste un "nom", pourquoi il va me chercher une commande, j'ai pa d'idéé pour le coup.

Je fais donc l'appel à un ami pour avoir quleque chose de fonctionnel, Messire Godefroy de Montmirail.

![](./images/montmirail.png)


```sh
#!/bin/bash

target_path="${1}"
target_dir=$( echo "${target-path/}" | rev | cut -d'/' -f1 | rev)

date=$(date +\%Y%m%d_%H%M)
backup_name="${target_dir}/${date}.tar.gz"
backup_dir="/etc/home/vinny/backup"
backup_path="${backup_dir}/${backup-name}"

backup_user_name="Kupaité Simon"
backup_user_uid=1482

backups_quantity=7

if [[ ${EUID} -ne ${backup_user_uid} ]]; then
  echo "This script must be run as \"${backup_user_name}\" user, which d}. Exiting" >&2
  exit1
fi

if [[ ! -d "${target_path}" ]]; 
  echo "The target path ${target_path} does not exist. Exiting" >&2
  exit 1
fi

if [[ ! -d "${backup_dir}" ]]; 
  echo "The destination directory ${backup_dir} does not exist. Exiting" >&2
  exit 1
fi

archive_compress() {
  tar cvzf \
    "${backup_path}" \
    "${target_path}" \
    --ignore-failed-read =&> /dev/null

    if [[ $? -eq 0]]
    then
      echo "Backup has been successfully saved"
    else
      echo "Your backup failed... Bravo six, going dark."
    
    rm "${backup_path}"
  fi
}
```

(franchement cimer fraté)
, ok je t'ai piqué ton code mais bon je m'en serais jamais sortis avec un truc fonctionnel. j'ai saisi comment fonctionne tn script, je peux te l'expliquer oralement pour te le prouver mas jamais j'aurais pu l'écrire de moi même. srry)

pour éxécuter le script toutes les heures, on pop cette ligne dans la "user's crontab" : '@hourly /home/vinny/tp1_backup.sh >/var/log/backup.log 2>&1' (ou '0 * * * * /home/vinny/tp1_backup.sh >/var/log/backup.log 2>&1' c'est la même chose)

Et là c'est le drame... le script de Messire Godefroy de Montmirail ne marche pas!

![](./images/diablerie.jpg)


```bash
[vinny@node1 ~]$ sudo ./tp1_backup.sh
[sudo] password for vinny:
This script must be run as "Kupaité Simon" user, which d}. Exiting
./tp1_backup.sh: line 18: exit1: command not found
./tp1_backup.sh: line 24: syntax error near unexpected token `fi'
./tp1_backup.sh: line 24: `fi'
```

![](./images/200.gif)

## Monitoring, alerting

Pour le monitoring avec discord... je vois le délire mais avec un script non fonctionel ça foire un peu tout... j'ai quand même essayé de tester la question, sans grand succès.

Pour me rattraper, je vais tenter d'expliquer comment j'aurais fait si tout fonctionnait correctement.

Si j'ai bien compris le principe, avec netdata, on cherche à faire poper des notifications d'alerte sur un serveur discord. Jusque là, j'ai bon. Maintenant, une fois bien configuré, on lie la vm contenant le server ainsi que le script au serveur discord dédié dont on aura au préalable récupéré son adresse (IP ou http) ou son identifiant pour lui envoyer une notification qui nous informera d'un malfonctionnement de la vm en cas de problème d'allocution de RAM (par ex) et/ou de disque (par ex aussi) et/ou de même d'une mauvaise éxécution du script de backup ce qui nuiera formellement au but du TP seulement dans le cas où on ne serait pas informé du problème et donc pas capable de gérer le problème.

![](./images/mad.jpg)