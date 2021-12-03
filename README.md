# unbound-config

## About

Originally designed purely for personal use, unbound-config is a project that has evolved around the configuration and management of NLnet Labs' [Unbound](https://nlnetlabs.nl/projects/unbound/about/) recursive nameserver.

* Configuration Files

A set of modular configuration files are offered including a base.conf (required), and multiple optional configuration files adding or defining additional functionality. Intended to serve as a working basis for further configuration while providing reasonably sane defaults, many values supplied are in fact current default values. Constant expirimentation in this area and the want for a ulitility to assist with backup and restore of different configuration profiles during testing lead to the following.

* Utility Script

Perhaps confusingly named, unbound-config is also a general management and utility script for Unbound. This utility offers a range of functions including creation, listing, and restoration of Unbound configuration backups, and the ability to install a set of recommended unbound-config configuration files. This part of the project started from an installtion script, that was dumb as a sack of rocks, originally packaged exclusively with the following.

* Unbound Binaries

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
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/base.conf
```

* Download Additonal Config Fragments As Required

Note: Recommended configuration fragments are marked as such.

Access Control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/access-control.conf
```
Automatic Interface
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/auto-interface.conf
```
Buffers (Recommended)

Note: See notes on additional system configuration below.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/buffers.conf
```
Caches (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/caches.conf
```
Cache TTL
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/cache-ttl.conf
```
Address Capitalization Randomization
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/caps-for-id.conf
```
Deny ANY Requests
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/deny-any.conf
```
DNS64 (Requires NAT64 Gateway)

Note: You probably don't have a NAT64 Gateway.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/dns64.conf
```
EDNS Buffer
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/edns-buffer.conf
```
Fast Servers
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/fast-server.conf
```
Fetch Policy
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/fetch-policy.conf
```
Hardening (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/hardening.conf
```
IPv6
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/ipv6.conf
```
Libevent (Recommended)

Note: Requires installation of libevent-dev on the Unbound host.
```
sudo apt install libevent-dev
```
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/libevent.conf
```
Local Records

Note: Example Only - Must be edited, contains deliberately garbage values.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/local-records.conf
```
Module Config
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/module-config.conf
```
Multithreaded UDP
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/multithreaded-udp.conf
```
Multithreading (Recommended)

Note: For multi-core machines number of threads equals number of cores is a good rule, should be a factor of two.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/multithreading.conf
```
Prefetch (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/prefetch.conf
```
Private Address Ranges (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/private-ranges.conf
```
Rate Limiting
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/rate-limiting.conf
```
Redis Cache DB

Notes: Requires [module-config.conf](https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/module-config.conf)).

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
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/redis.conf
```
Remote Control

Note: Remember to run unbound-control-setup on the Unbound host before trying to use unbound-control.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/remote-control.conf
```
Root Hints
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/root-hints.conf
```
Serve Expired Records
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/serve-expired-records.conf
```
Server Identity
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/server-identity.conf
```
Verbosity (Recommended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/verbosity.conf
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
Version 1.14.0

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
Usage: unbound-control OPTION

Where OPTION is one of

    -b                      Backup the current Unbound configuration to a
    backup                  .tar.gz archive located within
    --backup-config         /etc/unbound/unbound.conf.d-backup

                            Takes an optional parameter to be normalised and
                            used as the backup ID, strings containing spaces
                            must be quoted, e.g. "my unbound backup"

                            The default backup ID naming scheme is:
                            YYYYMMDDHHMM


    -c                      Install recommended unbound-config config
    config                  fragments:
    --config-recommended    Base (Required) Buffers, Caches, Hardening,
                            Libevent, Multithreading, Prefetch, Private
                            Address Ranges, Verbosity


    -d                      Download unbound-config Unbound binaries in a
    download                .tar.gz archive to $tmp
    --download-binaries
                            Takes the optional parameter --force to remove an
                            existing binary package before downloading a new
                            one


    -D                      Delete any Unbound configuration backups that
    delete                  unbound-config --backup has made
    --delete-backups


    -h                      Display this help dialogue
    help
    --help


    -i                      Install unbound-config unbound binaries:
    unbound                 unbound, unbound-anchor, unbound-checkconf,
    --install-unbound       unbound-control, unbound-control-setup,
                            unbound-host

                            Takes the optional parameter --unbound-only to
                            install only the unbound binary

    -I                      Download and install the unbound-config script to
    script                  local storage, or update an existing locally
    --install-script        installed copy


    -l                      List possible backup IDs found in
    list                    /etc/unbound/unbound.conf.d-backup
    --list-backups
                            Useful for getting backup IDs for --restore-backup


    -r                      Remove the current Unbound configuration
    remove
    --remove-config         A backup is required before removing any existing
                            configuration, prompts for backup if none exist


    -R ID                   Restore a backup of your Unbound configuration to
    restore ID              the Unbound configuration directory
    --restore-backup ID
                            Use --list-backups to list possible backup IDs


    -u                      Uninstall any unbound binaries unbound-config may
    uninstall               have installed
    --uninstall-binaries


    -v                      Displays the unbound-config version
    version                 Current unbound-config version v1.2.1
    --version
```

To skip dpkg-based dependency checking on possibly unsupported platforms, you can attempt to run unbound-config with dependency checking disabled using:
```
export SKIP_DEPENDENCY_CHECKS=true
```
Then run unbound-config as normal.

Current unbound-config utility script dependencies:
```
init-system-helpers tar unbound wget whiptail
```

Required if installing unbound-config Unbound binaries:
```
libevent-dev
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
