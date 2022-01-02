# unbound-config

Configuration & Management Of [NLnet Labs](https://www.nlnetlabs.nl/)' Unbound DNS Resolver


## About

Originally designed purely for personal use, unbound-config is a project that has evolved around the configuration and management of NLnet Labs' [Unbound](https://nlnetlabs.nl/projects/unbound/about/) recursive nameserver ([source](https://github.com/NLnetLabs/unbound/commits/master)).

Three shall be the number thou shalt count, and the number of components in this repository shall be three. Four shalt thou not count, neither count thou two, excepting that thou then proceed to three. Five is right out!

* [Modulur Unbound Configuration Files](https://github.com/saint-lascivious/unbound-config/tree/master/configs)

A range of modular configuration files is offered including a base.conf (required), and multiple optional configuration files adding or defining additional functionality. Intended to serve as a working basis for further configuration while providing reasonably sane defaults, many values supplied are in fact current default values. Constant expirimentation in this area and the want for a ulitility to assist with backup and restore of different configuration profiles during testing lead to the creation and adaptation of the following...

* [Utility Script](https://github.com/saint-lascivious/unbound-config/tree/master/script)

Perhaps confusingly named, unbound-config is also a general management and utility script for Unbound. This utility offers a range of functions including creation, listing, and restoration of Unbound configuration backups, and the ability to install a set of recommended unbound-config configuration files. This part of the project started from an installtion script, that was dumb as a sack of rocks, originally packaged exclusively with the following...

* [Unbound Binaries](https://github.com/saint-lascivious/unbound-config/tree/master/binaries)

Finding limitation in the Unbound binaries distributed via various system package managers, I found myself compiling from source regularly. I would also regularly find users of this project and others frustrated by either the lack of modules, updates, or both in their system package manager's Unbound binaries eventually leading to my distributing a set of periodically updated Unbound binaries compiled from the latest Unbound via this repository.


## Usage

* Backup And Remove Existing Unbound Configuration

Backup and remove any existing Unbound configuration using the unbound-config utility script.
```
wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/script/unbound-config -P /tmp && chmod +x /tmp/unbound-config
```
You will be prompted to make a backup of your existing configuration before the current configuration is able to be removed.
```
/tmp/unbound-config --remove-config
```
Note: You will be prompted to install any unmet dependencies as they are required.

* Download unbound-config Base Configuration

Base (Required)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/base.conf
```

* Download Additonal Config Fragments As Required

Note: Recommended configuration fragments are marked as such.

Access Control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/access-control.conf
```
Automatic Interface
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/auto-interface.conf
```
Buffers (Recommended)

Note: See notes on additional system configuration below.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/buffers.conf
```
Caches (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/caches.conf
```
Cache TTL
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/cache-ttl.conf
```
Address Capitalization Randomization
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/caps-for-id.conf
```
Deny ANY Requests
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/deny-any.conf
```
DNS64 (Requires NAT64 Gateway)

Note: You probably don't have a NAT64 Gateway.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/dns64.conf
```
Disable Logging
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/disable-logging.conf
```
EDNS Buffer
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/edns-buffer.conf
```
Fast Servers
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/fast-server.conf
```
Fetch Policy
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/fetch-policy.conf
```
Hardening (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/hardening.conf
```
IPv6
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/ipv6.conf
```
Libevent (Recommended)

Note: Requires installation of libevent-dev on the Unbound host.
```
sudo apt install libevent-dev
```
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/libevent.conf
```
Local Records
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/local-records.conf
```
Module Config
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/module-config.conf
```
Multithreaded UDP
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/multithreaded-udp.conf
```
Multithreading (Recommended)

Note: For multi-core machines number of threads equals number of cores is a good rule, should be a factor of two.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/multithreading.conf
```
Prefetch (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/prefetch.conf
```
Private Address Ranges (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/private-ranges.conf
```
Rate Limiting
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/rate-limiting.conf
```
Redis Cache DB

Notes: Requires [module-config.conf](https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/module-config.conf)).

Unbound must be compiled with both --with-libhiredis and --enable-cachedb flags enabled. Check if your version supports this with 'unbound -V', it probably doesn't (but [mine do](https://github.com/saint-lascivious/unbound-config/tree/master/binaries)).

See notes on additional system configuration below.
```
sudo apt install redis-server
```

Note: Configure a single memory limited Redis database with an least recently used eviction policy.
```
file=/etc/redis/redis.conf
sudo sed -i '/databases 16/s/^/#/g' $file
sudo sed -i '/#databases 16/a databases 1' $file
sudo sed -i '/always-show-logo yes/s/^/#/g' $file
sudo sed -i '/#always-show-logo yes/a always-show-logo no' $file
sudo sed -i '/stop-writes-on-bgsave-error yes/s/^/#/g' $file
sudo sed -i '/#stop-writes-on-bgsave-error yes/a stop-writes-on-bgsave-error no' $file
sudo sed -i '/rdbcompression yes/s/^/#/g' $file
sudo sed -i '/#rdbcompression yes/a rdbcompression no' $file
sudo sed -i '/# maxmemory <bytes>/a maxmemory 8M' $file
sudo sed -i '/# maxmemory-policy noeviction/a maxmemory-policy allkeys-lru' $file
sudo sed -i '/slowlog-max-len 128/s/^/#/g' $file
sudo sed -i '/#slowlog-max-len 128/a slowlog-max-len 16' $file
sudo sed -i '/logfile \/var\/log\/redis\/redis-server.log/s/^/#/g' $file
```
```
sudo systemctl restart redis
```
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/redis.conf
```
Remote Control

Note: Remember to run unbound-control-setup on the Unbound host before trying to use unbound-control.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/remote-control.conf
```
Root Hints
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/root-hints.conf
```
Serve Expired Records
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/serve-expired-records.conf
```
Server Identity
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/server-identity.conf
```
Verbosity (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/configs/verbosity.conf
```

* Restart unbound

After any changes to the server configuration the server must be restarted.
```
sudo service unbound restart
```


## Alternative Install Method

 * Automated Installation

Install base.conf and recommended config fragments using the unbound-config utility script.

Note: Backup and removal of any existing Unbound configuration is handled semi-automatically, you will be prompted for confirmation.
```
wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/script/unbound-config -P /tmp && chmod +x /tmp/unbound-config
```
```
/tmp/unbound-config --config-recommended
```


## Source Compiled Unbound Binaries

* What Are They?

I have compiled Unbound (and its associated toolset) from [source](https://github.com/NLnetLabs/unbound), with some additional features which may not be present in some distribution packages (cachedb, ipsecmod, ipset, DNSCrypt, TFO).

Example output from "unbound -V" (aarch64 version):
```
Version 1.14.1

Configure line: --build=aarch64-linux-gnu --prefix=/usr --includedir=${prefix}/include --mandir=${prefix}/share/man --infodir=${prefix}/share/info --sysconfdir=/etc --localstatedir=/var --disable-option-checking --disable-silent-rules --libdir=${prefix}/lib/aarch64-linux-gnu --libexecdir=${prefix}/lib/aarch64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --disable-rpath --with-pidfile=/run/unbound.pid --with-rootkey-file=/var/lib/unbound/root.key --with-libevent --with-pythonmodule --enable-subnet --enable-dnstap --enable-systemd --with-chroot-dir= --with-dnstap-socket-path=/run/dnstap.sock --libdir=/usr/lib --disable-flto --enable-cachedb --enable-dnscrypt --enable-ipsecmod --enable-ipset --enable-tfo-client --enable-tfo-server --with-libhiredis --with-libnghttp2
Linked libs: libevent 2.1.12-stable (it uses epoll), OpenSSL 1.1.1j  16 Feb 2021
Linked modules: dns64 python cachedb ipsecmod subnetcache ipset respip validator iterator
DNSCrypt feature available
TCP Fastopen feature available

BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl or https://github.com/NLnetLabs/unbound/issues
```
This is (very deliberately) almost identical to the Debian/Ubuntu unbound binary package configuration.

It is safe, but not necessarily recommended to replace the system unbound binaries with those provided.
Any updates to the system package will remove the custom binary.

* What Platforms Do unbound-config Unbound Binaries Run On?

At the present, aarch64 (armv8) and armhf (armv6l, armv7l) binaries are provided. Only tested on Debian and Ubuntu derivaties.

* Will You Continue To Update unbound-config Unbound Binaries?

Probably, yes.


## Download And Install unbound-config Unbound Binaries

Download and install unbound-config Unbound binaries using the unbound-config utility script.
```
wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/script/unbound-config -P /tmp && chmod +x /tmp/unbound-config
```
```
/tmp/unbound-config --install-unbound
```


## Additional unbound-config Features

A number of additional features are available via the unbound-config utility script.

The full --help text for unbound-config is as follows:
```
Usage: unbound-control [OPTION [PARAM]]

Where OPTION is one (1) of

    -b                      Backup the current Unbound configuration to a
    --backup-config         .tar.gz archive located within
                            /etc/unbound/unbound.conf.d-backup

                            Takes an optional parameter (to be normalised and)
                            used as the backup ID, IDs containing spaces
                            must be quoted, e.g. "my unbound backup"
                            The default is a timestamp in the format:
                            YYYYMMDDHHMM

                            If [--rfc3339] is used as the backup ID an rfc3339
                            compliant timestamp will be used instead:
                            YYYY-MM-DDTHH:MM:SS±00:00

    examples:               unbound-config --backup-config "my unbound backup"
                            unbound-config --backup-config --rfc3339

    -c                      Install recommended unbound-config config
    --config-recommended    fragments:
                            Base (Required) Buffers, Caches, Hardening,
                            Libevent, Multithreading, Prefetch, Private
                            Address Ranges, Verbosity

    -C                      Install and configure a 16MB Redis persistent
    --configure-cachedb     cache database with an LRU eviction policy for
                            use with the Unbound cachedb module

                            Your Unbound binaries probably don't support this
                            but unbound-config Unbound binaries do

    -d                      Download unbound-config Unbound binaries in a
    --download-unbound      .tar.gz archive to /tmp

                            Takes an optional parameter [--force] to remove an
                            existing binary package before downloading a new
                            one

    -D ID                   Delete an unbound-config backup with a specified
    --delete-backup ID      backup ID

                            Use --list-backups to list possible backup IDs

                            The [--all] flag may be provided in place of a
                            backup ID to delete all unbound-config backups

    examples:               unbound-config --delete-backup my_unbound_backup
                            unbound-config --delete-backup --all

    -h                      Display this help dialogue
    --help

    -i                      Install unbound-config unbound binaries built from
    --install-unbound       Unbound master 1.14.1 source:
                            unbound, unbound-anchor, unbound-checkconf,
                            unbound-control, unbound-control-setup,
                            unbound-host

                            Takes an optional parameter [--unbound-only] to
                            install only the unbound binary

                            Note: legacy unbound-checkconf and unbound-control
                            may fail on more modern unbound configuration
                            options

    examples:               unbound-config --install-unbound
                            unbound-config --install-unbound --unbound-only

    -I                      Download and install the unbound-config script to
    --install-script        local storage, or update an existing locally
                            installed copy

    -l                      List possible backup IDs found in
    --list-backups          /etc/unbound/unbound.conf.d-backup
                            Useful for getting backup IDs for --delete-backup
                            and --restore-backup

    -r                      Remove the current Unbound configuration
    --remove-config         A backup is required before removing any existing
                            configuration, prompts for backup if none exist

    -R ID                   Restore a backup of your Unbound configuration to
    --restore-backup ID     the Unbound configuration directory located at
                            /etc/unbound/unbound.conf.d

    example:                unbound-config --restore-backup my_unbound_backup

    -t                      Test the validated resolution capabilities of the
    --test-unbound          local Unbound installation by querying external
                            domains with known broken and known good DNSSEC
                            records

    -T                      Test for errors in the Unbound configuration by
    --test-config           running unbound-checkconf on all .conf files in
                            the configuration directory located at
                            /etc/unbound/unbound.conf.d

    -u                      Uninstall any unbound binaries unbound-config may
    --uninstall-unbound     have installed

    -v                      Displays the unbound-config version
    --version
                            Current unbound-config version v1.8"
```

The full list of unbound-config dependencies is as follows:
```
dpkg init-system-helpers libevent-dev libhiredis-dev redis-server sudo tar unbound wget whiptail
```
```
    Package:                Explanation:

    dpkg                    dpkg-query is used to test for the presence of
                            dependencies as required when required

    init-system-helpers     Used for unbound service management

    libevent-dev            Used in recommended config for large potentially
                            very large outgoing port ranges
                            Will not notably impact performance
                            Unused in the absence of libevent.conf

    libhiredis-dev          Used for Redis database backend cachedb access

    redis-server            Used for providing a Redis database

    sudo                    Used for priveleged system access if the user is
                            not root

    tar                     Used in the creation and extraction of .tar.gz
                            archives processed by unbound-config

    unbound                 Expected to exist for configuration and/or
                            providing the init system for unbound-config
                            provided Unbound binaries

    wget                    Used for downloading unbound-config unbound binary
                            archives and configuration fragments

    whiptail                Used for the display of interactive terminal user
                            confirmation prompts and notices
```


## Notes On Additional System Configuration

* TCP Fast Open

If using my Unbound binaries and your kernel supports it, you may want to add the following the following to /etc/sysctl.conf or /etc/sysctl.d/99-tcp-fastopen.conf (you will need to create this file) and restarting the machine.
```
net.ipv4.tcp_fastopen=3
```

* Large Buffers

Large buffer values may print a warning about insufficient net.core memory values.

You can address this by adding the following to /etc/sysctl.conf or /etc/sysctl.d/99-net-core-mem.conf (you will need to create this file) and restarting the machine.
```
net.core.rmem_default=2097152
net.core.wmem_default=2097152
net.core.rmem_max=4104304
net.core.wmem_max=4194304
```

* Redis Cache Database

Redis may print a warming regarding vm.overcommit and loss of data on background save.

You can address this by adding the following to /etc/sysctl.conf or /etc/sysctl.d/99-overcommit-memory.conf (you will need to create this file) and restarting the machine.

Note : See the Redis FAQ entry ['Background saving fails with a fork() error under Linux even if I have a lot of free RAM!'](https://redis.io/topics/faq#background-saving-fails-with-a-fork-error-under-linux-even-if-i-have-a-lot-of-free-ram) for more information.
```
vm.overcommit_memory=1
```
The names of the files used for `/etc/sysctl.d/` are descriptive for your reference but can be arbitrary.

Any of these flags can also be enabled without restarting by using
```
sudo sysctl FLAG=VALUE
```
Example:
```
sudo sysctl vm.overcommit_memory=1
```


## Contact

* Discord
[SaintLascivious](https://discord.gg/NC7taVyn)

* Email
saint@sainternet.xyz

* IRC
[##saint-lascivious](https://webchat.freenode.net/##saint-lascivious)

* Reddit
[saint-lascivious](https://www.reddit.com/user/saint-lascivious)

![alt text][logo]

[logo]:https://vignette.wikia.nocookie.net/pokemon/images/7/76/265Wurmple.png "Using the spikes on its rear end, Wurmple peels the bark off trees and feeds on the sap that oozes out. This Pokémon's feet are tipped with suction pads that allow it to cling to glass without slipping."
