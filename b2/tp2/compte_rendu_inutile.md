# TP2 DE SES DEFUNTS

*Quelque chose me dit que tu vas vite te faire chier en lisant ce compte rendu, d'une part parce que les allers retours sur les différents fichiers ç'est marrant deux secondes mais x65 non merci j'ai plus faim. Alors je te propose de t'ambiancier sur cette [musique](https://www.youtube.com/watch?v=UFcJmOs8DRQ), ou celle [là](https://www.youtube.com/watch?v=A8Uij5N3etU) ou celle [ci](https://www.youtube.com/watch?v=LRVMUOYTc3Y) (si t'es plus headbanger taré) héhé.*

## 1 J'ai oublié le titre mais menfou

Pour commencer, je voulais spécifier la taille du disque mais il connaissait pas l'attribut `disk`. Alors je lui ai fait gober le plugin avec `vagrant plugin install vagrant-disksize` (mais ca a jamais marché la vm reste bloqué a 40gb )

![](./images/anyway.jpg)

Donc vagrant..., je te laisse aller check le vagrantfile + script pour cette première partie avant de regarder la suite (comme pour toutes les autres partie en fait).

```powershell
PS P:\Logiciels\HashiCorp\vagrant_tp> vagrant up
Bringing machine 'hello' up with 'virtualbox' provider...
==> hello: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> hello: flag to force provisioning. Provisioners marked to run always will still run.
==> hello: Running provisioner: shell...
    hello: Running: C:/Users/marsl/AppData/Local/Temp/vagrant-shell20201005-18620-545s5v.sh
    hello: Loaded plugins: fastestmirror
    hello: Determining fastest mirrors
    hello:  * base: mir01.syntis.net
    hello:  * extras: mir01.syntis.net
    hello:  * updates: mir01.syntis.net
    hello: Resolving Dependencies
    hello: --> Running transaction check
    hello: ---> Package vim-enhanced.x86_64 2:7.4.629-6.el7 will be installed
    hello: --> Processing Dependency: vim-common = 2:7.4.629-6.el7 for package: 2:vim-enhanced-7.4.629-6.el7.x86_64
    hello: --> Processing Dependency: perl(:MODULE_COMPAT_5.16.3) for package: 2:vim-enhanced-7.4.629-6.el7.x86_64
    hello: --> Processing Dependency: libperl.so()(64bit) for package: 2:vim-enhanced-7.4.629-6.el7.x86_64
    hello: --> Processing Dependency: libgpm.so.2()(64bit) for package: 2:vim-enhanced-7.4.629-6.el7.x86_64
    hello: --> Running transaction check
    hello: ---> Package gpm-libs.x86_64 0:1.20.7-6.el7 will be installed
    hello: ---> Package perl.x86_64 4:5.16.3-295.el7 will be installed
    hello: --> Processing Dependency: perl(Socket) >= 1.3 for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Scalar::Util) >= 1.10 for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl-macros for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(threads::shared) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(threads) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(constant) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Time::Local) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Time::HiRes) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Storable) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Socket) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Scalar::Util) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Pod::Simple::XHTML) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Pod::Simple::Search) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Getopt::Long) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Filter::Util::Call) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(File::Temp) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(File::Spec::Unix) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(File::Spec::Functions) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(File::Spec) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(File::Path) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Exporter) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Cwd) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: --> Processing Dependency: perl(Carp) for package: 4:perl-5.16.3-295.el7.x86_64
    hello: ---> Package perl-libs.x86_64 4:5.16.3-295.el7 will be installed
    hello: ---> Package vim-common.x86_64 2:7.4.629-6.el7 will be installed
    hello: --> Processing Dependency: vim-filesystem for package: 2:vim-common-7.4.629-6.el7.x86_64
    hello: --> Running transaction check
    hello: ---> Package perl-Carp.noarch 0:1.26-244.el7 will be installed
    hello: ---> Package perl-Exporter.noarch 0:5.68-3.el7 will be installed
    hello: ---> Package perl-File-Path.noarch 0:2.09-2.el7 will be installed
    hello: ---> Package perl-File-Temp.noarch 0:0.23.01-3.el7 will be installed
    hello: ---> Package perl-Filter.x86_64 0:1.49-3.el7 will be installed
    hello: ---> Package perl-Getopt-Long.noarch 0:2.40-3.el7 will be installed
    hello: --> Processing Dependency: perl(Pod::Usage) >= 1.14 for package: perl-Getopt-Long-2.40-3.el7.noarch
    hello: --> Processing Dependency: perl(Text::ParseWords) for package: perl-Getopt-Long-2.40-3.el7.noarch
    hello: ---> Package perl-PathTools.x86_64 0:3.40-5.el7 will be installed
    hello: ---> Package perl-Pod-Simple.noarch 1:3.28-4.el7 will be installed
    hello: --> Processing Dependency: perl(Pod::Escapes) >= 1.04 for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
    hello: --> Processing Dependency: perl(Encode) for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
    hello: ---> Package perl-Scalar-List-Utils.x86_64 0:1.27-248.el7 will be installed
    hello: ---> Package perl-Socket.x86_64 0:2.010-5.el7 will be installed
    hello: ---> Package perl-Storable.x86_64 0:2.45-3.el7 will be installed
    hello: ---> Package perl-Time-HiRes.x86_64 4:1.9725-3.el7 will be installed
    hello: ---> Package perl-Time-Local.noarch 0:1.2300-2.el7 will be installed
    hello: ---> Package perl-constant.noarch 0:1.27-2.el7 will be installed
    hello: ---> Package perl-macros.x86_64 4:5.16.3-295.el7 will be installed
    hello: ---> Package perl-threads.x86_64 0:1.87-4.el7 will be installed
    hello: ---> Package perl-threads-shared.x86_64 0:1.43-6.el7 will be installed
    hello: ---> Package vim-filesystem.x86_64 2:7.4.629-6.el7 will be installed
    hello: --> Running transaction check
    hello: ---> Package perl-Encode.x86_64 0:2.51-7.el7 will be installed
    hello: ---> Package perl-Pod-Escapes.noarch 1:1.04-295.el7 will be installed
    hello: ---> Package perl-Pod-Usage.noarch 0:1.63-3.el7 will be installed
    hello: --> Processing Dependency: perl(Pod::Text) >= 3.15 for package: perl-Pod-Usage-1.63-3.el7.noarch
    hello: --> Processing Dependency: perl-Pod-Perldoc for package: perl-Pod-Usage-1.63-3.el7.noarch
    hello: ---> Package perl-Text-ParseWords.noarch 0:3.29-4.el7 will be installed
    hello: --> Running transaction check
    hello: ---> Package perl-Pod-Perldoc.noarch 0:3.20-4.el7 will be installed
    hello: --> Processing Dependency: perl(parent) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
    hello: --> Processing Dependency: perl(HTTP::Tiny) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
    hello: ---> Package perl-podlators.noarch 0:2.5.1-3.el7 will be installed
    hello: --> Running transaction check
    hello: ---> Package perl-HTTP-Tiny.noarch 0:0.033-3.el7 will be installed
    hello: ---> Package perl-parent.noarch 1:0.225-244.el7 will be installed
    hello: --> Finished Dependency Resolution
    hello:
    hello: Dependencies Resolved
    hello:
    hello: ================================================================================
    hello:  Package                     Arch        Version                Repository
    hello:                                                                            Size
    hello: ================================================================================
    hello: Installing:
    hello:  vim-enhanced                x86_64      2:7.4.629-6.el7        base      1.1 M
    hello: Installing for dependencies:
    hello:  gpm-libs                    x86_64      1.20.7-6.el7           base       32 k
    hello:  perl                        x86_64      4:5.16.3-295.el7       base      8.0 M
    hello:  perl-Carp                   noarch      1.26-244.el7           base       19 k
    hello:  perl-Encode                 x86_64      2.51-7.el7             base      1.5 M
    hello:  perl-Exporter               noarch      5.68-3.el7             base       28 k
    hello:  perl-File-Path              noarch      2.09-2.el7             base       26 k
    hello:  perl-File-Temp              noarch      0.23.01-3.el7          base       56 k
    hello:  perl-Filter                 x86_64      1.49-3.el7             base       76 k
    hello:  perl-Getopt-Long            noarch      2.40-3.el7             base       56 k
    hello:  perl-HTTP-Tiny              noarch      0.033-3.el7            base       38 k
    hello:  perl-PathTools              x86_64      3.40-5.el7             base       82 k
    hello:  perl-Pod-Escapes            noarch      1:1.04-295.el7         base       51 k
    hello:  perl-Pod-Perldoc            noarch      3.20-4.el7             base       87 k
    hello:  perl-Pod-Simple             noarch      1:3.28-4.el7           base      216 k
    hello:  perl-Pod-Usage              noarch      1.63-3.el7             base       27 k
    hello:  perl-Scalar-List-Utils      x86_64      1.27-248.el7           base       36 k
    hello:  perl-Socket                 x86_64      2.010-5.el7            base       49 k
    hello:  perl-Storable               x86_64      2.45-3.el7             base       77 k
    hello:  perl-Text-ParseWords        noarch      3.29-4.el7             base       14 k
    hello:  perl-Time-HiRes             x86_64      4:1.9725-3.el7         base       45 k
    hello:  perl-Time-Local             noarch      1.2300-2.el7           base       24 k
    hello:  perl-constant               noarch      1.27-2.el7             base       19 k
    hello:  perl-libs                   x86_64      4:5.16.3-295.el7       base      689 k
    hello:  perl-macros                 x86_64      4:5.16.3-295.el7       base       44 k
    hello:  perl-parent                 noarch      1:0.225-244.el7        base       12 k
    hello:  perl-podlators              noarch      2.5.1-3.el7            base      112 k
    hello:  perl-threads                x86_64      1.87-4.el7             base       49 k
    hello:  perl-threads-shared         x86_64      1.43-6.el7             base       39 k
    hello:  vim-common                  x86_64      2:7.4.629-6.el7        base      5.9 M
    hello:  vim-filesystem              x86_64      2:7.4.629-6.el7        base       11 k
    hello:
    hello: Transaction Summary
    hello: ================================================================================
    hello: Install  1 Package (+30 Dependent packages)
    hello:
    hello: Total download size: 18 M
    hello: Installed size: 60 M
    hello: Downloading packages:
    hello: Public key for gpm-libs-1.20.7-6.el7.x86_64.rpm is not installed
    hello: warning: /var/cache/yum/x86_64/7/base/packages/gpm-libs-1.20.7-6.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
    hello: --------------------------------------------------------------------------------
    hello: Total                                              1.5 MB/s |  18 MB  00:12
    hello: Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    hello: Importing GPG key 0xF4A80EB5:
    hello:  Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
    hello:  Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
    hello:  Package    : centos-release-7-8.2003.0.el7.centos.x86_64 (@anaconda)
    hello:  From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    hello: Running transaction check
    hello: Running transaction test
    hello: Transaction test succeeded
    hello: Running transaction
    hello:   Installing : 1:perl-parent-0.225-244.el7.noarch                          1/31
    hello:
    hello:   Installing : perl-HTTP-Tiny-0.033-3.el7.noarch                           2/31
    hello:
    hello:   Installing : perl-podlators-2.5.1-3.el7.noarch                           3/31
    hello:
    hello:   Installing : perl-Pod-Perldoc-3.20-4.el7.noarch                          4/31
    hello:
    hello:   Installing : 1:perl-Pod-Escapes-1.04-295.el7.noarch                      5/31
    hello:
    hello:   Installing : perl-Text-ParseWords-3.29-4.el7.noarch                      6/31
    hello:
    hello:   Installing : perl-Encode-2.51-7.el7.x86_64                               7/31
    hello:
    hello:   Installing : perl-Pod-Usage-1.63-3.el7.noarch                            8/31
    hello:
    hello:   Installing : 4:perl-libs-5.16.3-295.el7.x86_64                           9/31
    hello:
    hello:   Installing : 4:perl-macros-5.16.3-295.el7.x86_64                        10/31
    hello:
    hello:   Installing : perl-Storable-2.45-3.el7.x86_64                            11/31
    hello:
    hello:   Installing : perl-Exporter-5.68-3.el7.noarch                            12/31
    hello:
    hello:   Installing : perl-constant-1.27-2.el7.noarch                            13/31
    hello:
    hello:   Installing : perl-Socket-2.010-5.el7.x86_64                             14/31
    hello:
    hello:   Installing : perl-Time-Local-1.2300-2.el7.noarch                        15/31
    hello:
    hello:   Installing : perl-Carp-1.26-244.el7.noarch                              16/31
    hello:
    hello:   Installing : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                      17/31
    hello:
    hello:   Installing : perl-PathTools-3.40-5.el7.x86_64                           18/31
    hello:
    hello:   Installing : perl-Scalar-List-Utils-1.27-248.el7.x86_64                 19/31
    hello:
    hello:   Installing : perl-File-Temp-0.23.01-3.el7.noarch                        20/31
    hello:
    hello:   Installing : perl-File-Path-2.09-2.el7.noarch                           21/31
    hello:
    hello:   Installing : perl-threads-shared-1.43-6.el7.x86_64                      22/31
    hello:
    hello:   Installing : perl-threads-1.87-4.el7.x86_64                             23/31
    hello:
    hello:   Installing : perl-Filter-1.49-3.el7.x86_64                              24/31
    hello:
    hello:   Installing : 1:perl-Pod-Simple-3.28-4.el7.noarch                        25/31
    hello:
    hello:   Installing : perl-Getopt-Long-2.40-3.el7.noarch                         26/31
    hello:
    hello:   Installing : 4:perl-5.16.3-295.el7.x86_64                               27/31
    hello:
    hello:   Installing : gpm-libs-1.20.7-6.el7.x86_64                               28/31
    hello:
    hello:   Installing : 2:vim-filesystem-7.4.629-6.el7.x86_64                      29/31
    hello:
    hello:   Installing : 2:vim-common-7.4.629-6.el7.x86_64                          30/31
    hello:
    hello:   Installing : 2:vim-enhanced-7.4.629-6.el7.x86_64                        31/31
    hello:
    hello:   Verifying  : perl-HTTP-Tiny-0.033-3.el7.noarch                           1/31
    hello:
    hello:   Verifying  : perl-threads-shared-1.43-6.el7.x86_64                       2/31
    hello:
    hello:   Verifying  : perl-Storable-2.45-3.el7.x86_64                             3/31
    hello:
    hello:   Verifying  : 1:perl-Pod-Escapes-1.04-295.el7.noarch                      4/31
    hello:
    hello:   Verifying  : perl-Exporter-5.68-3.el7.noarch                             5/31
    hello:
    hello:   Verifying  : perl-constant-1.27-2.el7.noarch                             6/31
    hello:
    hello:   Verifying  : perl-PathTools-3.40-5.el7.x86_64                            7/31
    hello:
    hello:   Verifying  : perl-Socket-2.010-5.el7.x86_64                              8/31
    hello:
    hello:   Verifying  : 1:perl-parent-0.225-244.el7.noarch                          9/31
    hello:
    hello:   Verifying  : 4:perl-libs-5.16.3-295.el7.x86_64                          10/31
    hello:
    hello:   Verifying  : perl-File-Temp-0.23.01-3.el7.noarch                        11/31
    hello:
    hello:   Verifying  : 1:perl-Pod-Simple-3.28-4.el7.noarch                        12/31
    hello:
    hello:   Verifying  : perl-Time-Local-1.2300-2.el7.noarch                        13/31
    hello:
    hello:   Verifying  : 4:perl-macros-5.16.3-295.el7.x86_64                        14/31
    hello:
    hello:   Verifying  : 4:perl-5.16.3-295.el7.x86_64                               15/31
    hello:
    hello:   Verifying  : 2:vim-enhanced-7.4.629-6.el7.x86_64                        16/31
    hello:
    hello:   Verifying  : perl-Carp-1.26-244.el7.noarch                              17/31
    hello:
    hello:   Verifying  : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                      18/31
    hello:
    hello:   Verifying  : perl-Scalar-List-Utils-1.27-248.el7.x86_64                 19/31
    hello:
    hello:   Verifying  : perl-Pod-Usage-1.63-3.el7.noarch                           20/31
    hello:
    hello:   Verifying  : 2:vim-common-7.4.629-6.el7.x86_64                          21/31
    hello:
    hello:   Verifying  : 2:vim-filesystem-7.4.629-6.el7.x86_64                      22/31
    hello:
    hello:   Verifying  : perl-Encode-2.51-7.el7.x86_64                              23/31
    hello:
    hello:   Verifying  : perl-Pod-Perldoc-3.20-4.el7.noarch                         24/31
    hello:
    hello:   Verifying  : perl-podlators-2.5.1-3.el7.noarch                          25/31
    hello:
    hello:   Verifying  : perl-File-Path-2.09-2.el7.noarch                           26/31
    hello:
    hello:   Verifying  : perl-threads-1.87-4.el7.x86_64                             27/31
    hello:
    hello:   Verifying  : gpm-libs-1.20.7-6.el7.x86_64                               28/31
    hello:
    hello:   Verifying  : perl-Filter-1.49-3.el7.x86_64                              29/31
    hello:
    hello:   Verifying  : perl-Getopt-Long-2.40-3.el7.noarch                         30/31
    hello:
    hello:   Verifying  : perl-Text-ParseWords-3.29-4.el7.noarch                     31/31
    hello:
    hello:
    hello: Installed:
    hello:   vim-enhanced.x86_64 2:7.4.629-6.el7
    hello:
    hello: Dependency Installed:
    hello:   gpm-libs.x86_64 0:1.20.7-6.el7
    hello:   perl.x86_64 4:5.16.3-295.el7
    hello:   perl-Carp.noarch 0:1.26-244.el7
    hello:   perl-Encode.x86_64 0:2.51-7.el7
    hello:   perl-Exporter.noarch 0:5.68-3.el7
    hello:   perl-File-Path.noarch 0:2.09-2.el7
    hello:   perl-File-Temp.noarch 0:0.23.01-3.el7
    hello:   perl-Filter.x86_64 0:1.49-3.el7
    hello:   perl-Getopt-Long.noarch 0:2.40-3.el7
    hello:   perl-HTTP-Tiny.noarch 0:0.033-3.el7
    hello:   perl-PathTools.x86_64 0:3.40-5.el7
    hello:   perl-Pod-Escapes.noarch 1:1.04-295.el7
    hello:   perl-Pod-Perldoc.noarch 0:3.20-4.el7
    hello:   perl-Pod-Simple.noarch 1:3.28-4.el7
    hello:   perl-Pod-Usage.noarch 0:1.63-3.el7
    hello:   perl-Scalar-List-Utils.x86_64 0:1.27-248.el7
    hello:   perl-Socket.x86_64 0:2.010-5.el7
    hello:   perl-Storable.x86_64 0:2.45-3.el7
    hello:   perl-Text-ParseWords.noarch 0:3.29-4.el7
    hello:   perl-Time-HiRes.x86_64 4:1.9725-3.el7
    hello:   perl-Time-Local.noarch 0:1.2300-2.el7
    hello:   perl-constant.noarch 0:1.27-2.el7
    hello:   perl-libs.x86_64 4:5.16.3-295.el7
    hello:   perl-macros.x86_64 4:5.16.3-295.el7
    hello:   perl-parent.noarch 1:0.225-244.el7
    hello:   perl-podlators.noarch 0:2.5.1-3.el7
    hello:   perl-threads.x86_64 0:1.87-4.el7
    hello:   perl-threads-shared.x86_64 0:1.43-6.el7
    hello:   vim-common.x86_64 2:7.4.629-6.el7
    hello:   vim-filesystem.x86_64 2:7.4.629-6.el7
    hello:
    hello: Complete!
```

Et oilàv, tu viens de te farcir 200 lignes de codes juste pour dire "it works". cool nan ?

![](./images/bipboy.gif)

Ca run, c'est cool donc ca fonctionne, Allé on passe au 2.

## 2 C'était quoi le nom du tp déjà ?

Ah bah non y' rien a faire, bon ok si quand même... le repackage simple efficace rapide.

On installe tout ce que tu demandes dans la vm puis on la pack pour en faire un .iso (ca marcha pareil pour les iso de crack ? (que tu dépack avec après ultraiso))

```powershell
PS P:\Logiciels\HashiCorp\vagrant_tp> vagrant package -output hello-custom.box
The machine with the name 'hello-custom.box' was not found configured for
this Vagrant environment.
PS P:\Logiciels\HashiCorp\vagrant_tp> vagrant package -output hello
==> hello: Attempting graceful shutdown of VM...
    hello: Guest communication could not be established! This is usually because
    hello: SSH is not running, the authentication information was changed,
    hello: or some other networking issue. Vagrant will force halt, if
    hello: capable.
==> hello: Forcing shutdown of VM...
==> hello: Clearing any previously set forwarded ports...
==> hello: Exporting VM...
==> hello: Compressing package to: P:/Logiciels/HashiCorp/vagrant_tp/utput
```

Petite précision, le patron de vm ne s'appelle pas "b2-tp2-centos" comme demandé parce que... full flemme de le changer ? Carrément (c'est une excuse valide au moins ?)

Be like :

![](./images/sloth.jpg)


## 3 Qu'est ce que je fais de ma vie...

Bon, multi nodes... bah grossomerdo, c'est la même chose qu'en haut mais en double sur le même vagrantfile.

![](./images/thor.jpg)

Voilà... pas besoin de trop réfléchir non plus.

```powershell
PS P:\Logiciels\HashiCorp\vagrant_tp> vagrant up
Bringing machine 'node1' up with 'virtualbox' provider...
Bringing machine 'node2' up with 'virtualbox' provider...
==> node1: Box 'utput' could not be found. Attempting to find and install...
    node1: Box Provider: virtualbox
    node1: Box Version: >= 0
==> node1: Box file was not detected as metadata. Adding it directly...
==> node1: Adding box 'utput' (v0) for provider: virtualbox
    node1: Unpacking necessary files from: file://P:/Logiciels/HashiCorp/vagrant_tp/utput
    node1:
==> node1: Successfully added box 'utput' (v0) for 'virtualbox'!
==> node1: Importing base box 'utput'...
==> node1: Matching MAC address for NAT networking...
==> node1: Setting the name of the VM: vagrant_tp_node1_1601925919856_5863
==> node1: Clearing any previously set network interfaces...
==> node1: Preparing network interfaces based on configuration...
    node1: Adapter 1: nat
    node1: Adapter 2: hostonly
==> node1: Forwarding ports...
    node1: 22 (guest) => 2222 (host) (adapter 1)
==> node1: Disk cannot be decreased in size. 10240 MB requested but disk is already 40960 MB.
==> node1: Booting VM...
==> node1: Waiting for machine to boot. This may take a few minutes...
    node1: SSH address: 127.0.0.1:2222
    node1: SSH username: vagrant
    node1: SSH auth method: private key
==> node1: Machine booted and ready!
==> node1: Checking for guest additions in VM...
    node1: No guest additions were detected on the base box for this VM! Guest
    node1: additions are required for forwarded ports, shared folders, host only
    node1: networking, and more. If SSH fails on this machine, please install
    node1: the guest additions and repackage the box to continue.
    node1:
    node1: This is not an error message; everything may continue to work properly,
    node1: in which case you may ignore this message.
==> node1: Setting hostname...
==> node1: Configuring and enabling network interfaces...
==> node2: Box 'utput' could not be found. Attempting to find and install...
    node2: Box Provider: virtualbox
    node2: Box Version: >= 0
==> node2: Box file was not detected as metadata. Adding it directly...
==> node2: Adding box 'utput' (v0) for provider: virtualbox
==> node2: Importing base box 'utput'...
==> node2: Matching MAC address for NAT networking...
==> node2: Setting the name of the VM: vagrant_tp_node2_1601925994880_60911
==> node2: Fixed port collision for 22 => 2222. Now on port 2200.
==> node2: Clearing any previously set network interfaces...
==> node2: Preparing network interfaces based on configuration...
    node2: Adapter 1: nat
    node2: Adapter 2: hostonly
==> node2: Forwarding ports...
    node2: 22 (guest) => 2200 (host) (adapter 1)
==> node2: Disk cannot be decreased in size. 10240 MB requested but disk is already 40960 MB.
==> node2: Booting VM...
==> node2: Waiting for machine to boot. This may take a few minutes...
    node2: SSH address: 127.0.0.1:2200
    node2: SSH username: vagrant
    node2: SSH auth method: private key
    node2: Warning: Connection reset. Retrying...
    node2: Warning: Connection aborted. Retrying...
==> node2: Machine booted and ready!
==> node2: Checking for guest additions in VM...
    node2: No guest additions were detected on the base box for this VM! Guest
    node2: additions are required for forwarded ports, shared folders, host only
    node2: networking, and more. If SSH fails on this machine, please install
    node2: the guest additions and repackage the box to continue.
    node2:
    node2: This is not an error message; everything may continue to work properly,
    node2: in which case you may ignore this message.
==> node2: Setting hostname...
==> node2: Configuring and enabling network interfaces...
```

Happy ? Because me not.


## 4 Je vends mon âme aux plus offrant

... Alors...

Plusieurs points. 
1. Bah ca marche pas 

Pourquoi ? Parce que les ip ont décider de me les briser menu. Sans mythos, je suis resté dessus en tentant de changer les ip, les hosts, les routes, tout ce que je pouvais pour résoudre les bémols, mais toujours le même ou un nouveau qui pop sans prévenir.

![](./images/bug.jpg)

Here un petit appercu (tiens bon lecture):

```powershell
[vagrant@node1 etc]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 75205sec preferred_lft 75205sec
    inet6 fe80::5054:ff:fe4d:77d3/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e7:31:2c brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.21/24 brd 192.168.2.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee7:312c/64 scope link
       valid_lft forever preferred_lft forever
[vagrant@node1 etc]$ sudo nano hosts
[vagrant@node1 etc]$ ping -c 1 node1
PING node1.tp2.b2 (127.0.2.1) 56(84) bytes of data.
64 bytes from node1.tp2.b2 (127.0.2.1): icmp_seq=1 ttl=64 time=0.048 ms

--- node1.tp2.b2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.048/0.048/0.048/0.000 ms
[vagrant@node1 etc]$ ping -c 1 node2
PING node2 (192.168.2.22) 56(84) bytes of data.
From node2 (192.168.2.22) icmp_seq=1 Destination Host Prohibited

--- node2 ping statistics ---
1 packets transmitted, 0 received, +1 errors, 100% packet loss, time 0ms

[vagrant@node1 etc]$ sudo nano hosts
[vagrant@node1 etc]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8
[vagrant@node1 etc]$ cd ./sysconfig/network-scripts/
[vagrant@node1 network-scripts]$ ls
ifcfg-eth0  ifdown-bnep  ifdown-isdn    ifdown-sit       ifup          ifup-ippp  ifup-plusb   ifup-sit       ifup-wireless
ifcfg-eth1  ifdown-eth   ifdown-post    ifdown-Team      ifup-aliases  ifup-ipv6  ifup-post    ifup-Team      init.ipv6-global
ifcfg-lo    ifdown-ippp  ifdown-ppp     ifdown-TeamPort  ifup-bnep     ifup-isdn  ifup-ppp     ifup-TeamPort  network-functions
ifdown      ifdown-ipv6  ifdown-routes  ifdown-tunnel    ifup-eth      ifup-plip  ifup-routes  ifup-tunnel    network-functions-ipv6
[vagrant@node1 network-scripts]$ sudo nano ifcfg-eth1
[vagrant@node1 network-scripts]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 74790sec preferred_lft 74790sec
    inet6 fe80::5054:ff:fe4d:77d3/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e7:31:2c brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.21/24 brd 192.168.2.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee7:312c/64 scope link
       valid_lft forever preferred_lft forever
[vagrant@node1 network-scripts]$ cat ifcfg-eth1
#VAGRANT-BEGIN
# The contents below are automatically generated by Vagrant. Do not modify.
NM_CONTROLLED=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=10.2.2.21
NETMASK=255.255.255.0
DEVICE=eth1
PEERDNS=no
#VAGRANT-END
[vagrant@node1 network-scripts]$ systemctl restart network
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: root
Password:
polkit-agent-helper-1: pam_authenticate failed: Authentication failure
==== AUTHENTICATION FAILED ===
Failed to restart network.service: Access denied
See system logs and 'systemctl status network.service' for details.
[vagrant@node1 network-scripts]$ sudo systemctl restart network
[vagrant@node1 network-scripts]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 86378sec preferred_lft 86378sec
    inet6 fe80::5054:ff:fe4d:77d3/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e7:31:2c brd ff:ff:ff:ff:ff:ff
    inet 10.2.2.21/24 brd 10.2.2.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee7:312c/64 scope link
       valid_lft forever preferred_lft forever
[vagrant@node1 network-scripts]$ ping 10.2.2.222
PING 10.2.2.222 (10.2.2.222) 56(84) bytes of data.
^C
--- 10.2.2.222 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2002ms

[vagrant@node1 network-scripts]$ ping 10.2.2.22
PING 10.2.2.22 (10.2.2.22) 56(84) bytes of data.
From 10.2.2.22 icmp_seq=1 Destination Host Prohibited
From 10.2.2.22 icmp_seq=2 Destination Host Prohibited
From 10.2.2.22 icmp_seq=3 Destination Host Prohibited
From 10.2.2.22 icmp_seq=4 Destination Host Prohibited
From 10.2.2.22 icmp_seq=5 Destination Host Prohibited
From 10.2.2.22 icmp_seq=6 Destination Host Prohibited
^XFrom 10.2.2.22 icmp_seq=7 Destination Host Prohibited
^C
--- 10.2.2.22 ping statistics ---
7 packets transmitted, 0 received, +7 errors, 100% packet loss, time 6021ms
```

Sinon tu dois te demander "pourquoi 10.2.2.21 ?". Laisse moi te répondre, "Rôgarde" :

```powershell
PS C:\Users\marsl> ipconfig

Configuration IP de Windows

[...]

Carte Ethernet VirtualBox Host-Only Network #2 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::a490:4aa4:2138:43a%12
   Adresse IPv4. . . . . . . . . . . . . .: 10.2.2.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
```

Malgrès tout, ça reste un problème de réseau puisque que `Destination Host Prohibited` ramenais encore et toujours sa fraise... donc bon.

![](./images/suicide.jpg)

Mais c'est pas catastrophique non plus... si ?

Ah.