# unbound-config

configuration files for [unbound](https://nlnetlabs.nl/projects/unbound/about/) recursive dns resolver

Settings and values have been derived in part by some suggested
defaults in [docs.pi-hole.net/guides/unbound](https://docs.pi-hole.net/guides/unbound/) in order to avoid
some possible interoperability issues and also from the documentation
found at [nlnetlabs.nl/documentation/unbound/howto-optimise](https://nlnetlabs.nl/documentation/unbound/howto-optimise/)
specifically regarding optimising unbound

## Usage
* Backup existing environment files
```
cd ~
mkdir .backup
sudo cp /etc/unbound/unbound.conf.d/*.conf ~/.backup
```
* Switch to the /etc/unbound/unbound.conf.d/ directory
```
cd /etc/unbound/unbound.conf.d/
```

* Remove pi-hole.conf if you deployed unbound using [Pi-hole documentation](https://docs.pi-hole.net/guides/unbound/)
```
sudo rm pi-hole.conf
```

* Download the base config:

Base (Required)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/base.conf
```

* Download any additional config file fragments as required:

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
After any changes to the server configuration the server must be restarted
```
sudo service unbound restart
```

## Source Compiled Unbound Binaries

* What are they?

I have compiled Unbound (and its associated toolset) from [source](https://github.com/NLnetLabs/unbound), with some additional features which may not be present in some distribution packages (cachedb, ipsecmod, ipset, DNSCrypt, TFO).

Example output from "unbound -V" (aarch64 version):
```
Version 1.13.3

Configure line: --build=aarch64-linux-gnu --prefix=/usr --includedir=${prefix}/include --mandir=${prefix}/share/man --infodir=${prefix}/share/info --sysconfdir=/etc --localstatedir=/var --disable-option-checking --disable-silent-rules --libdir=${prefix}/lib/aarch64-linux-gnu --libexecdir=${prefix}/lib/aarch64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --disable-rpath --with-pidfile=/run/unbound.pid --with-rootkey-file=/var/lib/unbound/root.key --with-libevent --enable-subnet --enable-dnstap --enable-systemd --with-chroot-dir= --with-dnstap-socket-path=/run/dnstap.sock --libdir=/usr/lib --disable-flto --enable-cachedb --enable-dnscrypt --enable-ipsecmod --enable-ipset --enable-tfo-client --enable-tfo-server --with-libhiredis --with-libnghttp2
Linked libs: libevent 2.1.12-stable (it uses epoll), OpenSSL 1.1.1j  16 Feb 2021
Linked modules: dns64 cachedb ipsecmod subnetcache ipset respip validator iterator
DNSCrypt feature available
TCP Fastopen feature available

BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl or https://github.com/NLnetLabs/unbound/issues
```
This is (very deliberately) almost identical to the Debian/Ubuntu unbound binary package configuration.

It is safe, but not necessarily recommended to replace the system unbound binaries with those provided.
Any updates to the system package will remove the custom binary.

* What do they run on?

At the present, aarch64 and armhf (armv6l, armv7l) binaries are provided.

* Will you continue to update these binaries?

Probably, yes.

* Download and install unbound-config unbound binaries
```
cd /tmp
```
```
wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/unbound-config && chmod +x unbound-config
```
```
./unbound-config --install-unbound
```

## Additional Features
A number of additional features are available via the unbound-config helper script.

The full --help text for unbound-config is as follows:
```
Usage: unbound-control OPTION

Where OPTION is one of

    -b                      Backup the current Unbound configuration to a
    backup                  .tar.gz archive located within
    --backup-config         /etc/unbound/unbound.conf.d-backup
                            The backup ID naming convention is YYYYMMDDHHMM

    -c                      Make a backup of the Unbound configuration directory
    config                  /etc/unbound/unbound.conf.d
    --config-recommended    Remove the current Unbound configuration, and
                            install unbound-config base config and additional
                            recommended config fragments

                            Requires libevent-dev to be installed

    -d                      Download unbound-config Unbound binaries in a
    download                .tar.gz archive to /tmp
    --download-binaries     Takes the optional parameter --force to remove an
                            existing binary package before downloading a new
                            one

    -D                      Delete all Unbound configuration backups that
    delete                  unbound-config --backup has made
    --delete-backups

    -h                      Displays this help dialogue
    help
    --help

    -i                      Install unbound-config unbound binaries:
    unbound                 unbound, unbound-anchor, unbound-checkconf,
    --install-unbound       unbound-control, unbound-control-setup, unbound-host

    -I                      Download and install the unbound-config script to
    script                  local storage, or update an existing locally
    --install-script        installed copy

    -l                      List possible backup IDs found in
    list                    /etc/unbound/unbound.conf.d-backup
    --list-backups          Useful for getting backup IDs for --restore-backup

    -r                      Remove the current Unbound configuration
    remove
    --remove-config

    -R ID                   Restore a backup of your Unbound configuration to
    restore-backup ID       the Unbound configuration directory
    --restore-backup ID     Use --list-backups to list possible backup IDs

    -u                      Uninstall unbound-config unbound binaries
    uninstall
    --uninstall-binaries

    -v                      Displays the unbound-config version
    version                 Current unbound-config version v0.9.9
    --version
```

## Notes On Additional System Configuration
* TCP Fast Open
If your kernel supports it, you may want to add the following the following to /etc/sysctl.conf or /etc/sysctl.d/99-tcp-fastopen.conf (you will need to create this file) and restarting the machine.
```
net.ipv4.tcp_fastopen=3
```

* Large Buffers
Large buffer values may print a warning about insufficient net.core memory values.

You can address this adding the following to /etc/sysctl.conf or /etc/sysctl.d/99-net-core-mem.conf (you will need to create this file) and restarting the machine.
```
net.core.rmem_default=2097152
net.core.wmem_default=2097152
net.core.rmem_max=4104304
net.core.wmem_max=4194304
```

* Redis CacheDB
If using Redis and the cachedb module, you may want the following to /etc/sysctl.conf or /etc/sysctl.d/99-overcommit-memory.conf (you will need to create this file) and restarting the machine.
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
