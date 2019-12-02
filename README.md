---
author:
- Nicolas Vadkerti
title: Le Protocole MQTT
---

<https://github.com/SlaynPool/CR_LXC-LXD/>

Container LXC sosu Linux
========================

Installation de LXC
-------------------

Pour installer LXC, on doit suivre le TP, càd :

-   ``` {#commande/1.txt .default caption="Installation de LXC" label="commande/1.txt" style="Style1"}
    apt-get purge lxd
    apt-get install lxc  lxc-templates lxc-utils bridge-utils debootstrap yum libvirt0 libpam-cgfs
    ```

Pour vérifier que l'installation fonctionne coorectement on peut
utiliser les commandes lxc-\...

-   ``` {#commande/2.txt .default caption="Verification de LXC" label="commande/2.txt" style="Style1"}
    root@ubuntu:~# lxc-checkconfig 
    Kernel configuration not found at /proc/config.gz; searching...
    Kernel configuration found at /boot/config-4.15.0-29-generic
    --- Namespaces ---
    Namespaces: enabled
    Utsname namespace: enabled
    Ipc namespace: enabled
    Pid namespace: enabled
    User namespace: enabled
    Network namespace: enabled

    --- Control groups ---
    Cgroups: enabled

    Cgroup v1 mount points: 
    /sys/fs/cgroup/systemd
    /sys/fs/cgroup/rdma
    /sys/fs/cgroup/memory
    /sys/fs/cgroup/cpuset
    /sys/fs/cgroup/net_cls,net_prio
    /sys/fs/cgroup/devices
    /sys/fs/cgroup/cpu,cpuacct
    /sys/fs/cgroup/hugetlb
    /sys/fs/cgroup/blkio
    /sys/fs/cgroup/pids
    /sys/fs/cgroup/freezer
    /sys/fs/cgroup/perf_event

    Cgroup v2 mount points: 
    /sys/fs/cgroup/unified

    Cgroup v1 clone_children flag: enabled
    Cgroup device: enabled
    Cgroup sched: enabled
    Cgroup cpu account: enabled
    Cgroup memory controller: enabled
    Cgroup cpuset: enabled

    --- Misc ---
    Veth pair device: enabled, not loaded
    Macvlan: enabled, not loaded
    Vlan: enabled, not loaded
    Bridges: enabled, loaded
    Advanced netfilter: enabled, not loaded
    CONFIG_NF_NAT_IPV4: enabled, loaded
    CONFIG_NF_NAT_IPV6: enabled, not loaded
    CONFIG_IP_NF_TARGET_MASQUERADE: enabled, loaded
    CONFIG_IP6_NF_TARGET_MASQUERADE: enabled, not loaded
    CONFIG_NETFILTER_XT_TARGET_CHECKSUM: enabled, loaded
    CONFIG_NETFILTER_XT_MATCH_COMMENT: enabled, not loaded
    FUSE (for use with lxcfs): enabled, not loaded

    --- Checkpoint/Restore ---
    checkpoint restore: enabled
    CONFIG_FHANDLE: enabled
    CONFIG_EVENTFD: enabled
    CONFIG_EPOLL: enabled
    CONFIG_UNIX_DIAG: enabled
    CONFIG_INET_DIAG: enabled
    CONFIG_PACKET_DIAG: enabled
    CONFIG_NETLINK_DIAG: enabled
    File capabilities: 

    Note : Before booting a new kernel, you can check its configuration
    usage : CONFIG=/path/to/config /usr/bin/lxc-checkconfig

    ```

Il ne semble pas avoir de soucis, ce qui est plutot bon signe pour la
suite du TP

Si l'on regarde dans /usr/share/lxc/templates/ on peut voir toutes les
distributions que l'on va pouvoir contenariser:

-   ``` {#commande/3.txt .default caption="listes des distributions contenarisables" label="commande/3.txt" style="Style1"}
    root@ubuntu:~# ls  /usr/share/lxc/templates/
    lxc-alpine     lxc-fedora-legacy  lxc-sabayon
    lxc-altlinux   lxc-gentoo	  lxc-slackware
    lxc-archlinux  lxc-local	  lxc-sparclinux
    lxc-busybox    lxc-oci		  lxc-sshd
    lxc-centos     lxc-openmandriva   lxc-ubuntu
    lxc-cirros     lxc-opensuse	  lxc-ubuntu-cloud
    lxc-debian     lxc-oracle	  lxc-voidlinux
    lxc-download   lxc-plamo
    lxc-fedora     lxc-pld

    ```

Création d'un container LXC Debian stretch.
-------------------------------------------

Pour cela, il faut :

-   ``` {#commande/4.txt .default caption="premier container" label="commande/4.txt" style="Style1"}
    root@ubuntu:~# lxc-create -t debian -n debian-j1 -- -r stretch -a amd64
    ebootstrap is /usr/sbin/debootstrap
    Checking cache download in /var/cache/lxc/debian/rootfs-stretch-amd64 ... 
    gpg: key 7638D0442B90D010: 4 signatures not checked due to missing keys
    gpg: key 7638D0442B90D010: "Debian Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>" not changed
    gpg: Total number processed: 1
    gpg:              unchanged: 1
    Downloading debian minimal ...
    I: Retrieving InRelease 
    I: Retrieving Release 
    ...

    root@ubuntu:~# lxc-info debian-j1
    Name:           debian-j1
    State:          STOPPED
    root@ubuntu:~# lxc-start debian-j1
    root@ubuntu:~# lxc-info debian-j1
    Name:           debian-j1
    State:          RUNNING
    PID:            19922
    CPU use:        0.12 seconds
    BlkIO use:      132.00 KiB
    Memory use:     13.07 MiB
    KMem use:       2.03 MiB
    Link:           veth19GBKG
     TX bytes:      858 bytes
     RX bytes:      780 bytes
    ```

Comme on peux le voir, nous venons de lancer notre premier container via
lxc.

Si l'on recreer un nouveau container avec un nom different, la vitesse
est bien plus rapide. En effet, il ne retelecharge pas toutes l'image,
mais copie simplement la précédente. Le cache est ici :
[var/cache/lxc/debian/rootfs-stretch-amd64](var/cache/lxc/debian/rootfs-stretch-amd64)

Pour déployer d'autre distributions comme centos on peut aussi utiliser
LXC

-   ``` {#commande/5.txt .default caption="LXC centos" label="commande/5.txt" style="Style1"}
    lxc-create -t centos -n centos 
    ```

<!-- -->

-   ``` {#commande/6.txt .default caption="Les commandes utiles" label="commande/6.txt" style="Style1"}
    # Lister les containers
    $ lxc-ls
    centos    debian-j1 debian-j2 

    # Demarrer un containers
    $ lxc-start debian-j1

    # Stopper Un
    ```
