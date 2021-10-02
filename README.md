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

Note: Requires [module-config.conf](https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/module-config.conf)).

Note: Unbound must be compiled with both --with-libhiredis and --enable-cachedb flags enabled.
Check if your version supports this with 'unbound -V', it probably doesn't (but [mine do](https://github.com/saint-lascivious/unbound-config/tree/master/binaries)).
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
I have compiled Unbound (and its associated toolset) from [source](https://github.com/NLnetLabs/unbound), with some additional features which may not be present in some distribution packages (cachedb, DNSCrypt, TFO).

Example output from "unbound -V" (aarch64 version):
```
Version 1.13.3

Configure line: --build=aarch64-linux-gnu --prefix=/usr --includedir=${prefix}/include --mandir=${prefix}/share/man --infodir=${prefix}/share/info --sysconfdir=/etc --localstatedir=/var --disable-option-checking --disable-silent-rules --libdir=${prefix}/lib/aarch64-linux-gnu --libexecdir=${prefix}/lib/aarch64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --disable-rpath --with-pidfile=/run/unbound.pid --with-rootkey-file=/var/lib/unbound/root.key --with-libevent --with-pythonmodule --enable-subnet --enable-dnstap --enable-systemd --with-chroot-dir= --with-dnstap-socket-path=/run/dnstap.sock --libdir=/usr/lib --disable-flto --enable-cachedb --enable-dnscrypt --enable-tfo-client --enable-tfo-server --with-libhiredis
Linked libs: libevent 2.1.12-stable (it uses epoll), OpenSSL 1.1.1j  16 Feb 2021
Linked modules: dns64 python cachedb subnetcache respip validator iterator
DNSCrypt feature available
TCP Fastopen feature available

BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl or https://github.com/NLnetLabs/unbound/issues
```
This is (very deliberately) almost identical to the Debian/Ubuntu unbound binary package configuration.

It is safe, but not necessarily recommended to replace the system unbound binaries with those provided.
Any updates to the system package will remove the custom binary.

* What do they run on?
At the present, aarch64 and armhf binaries are provided.

* Will you continue to update these binaries?
Probably, yes.

* Install

Ensure unbound is installed using the system package (we'll use its init scripts and dependencies)
```
sudo apt install unbound
```

* Download and install updated unbound binaries
```
cd /tmp
```
For aarch64 hosts
```
wget https://github.com/saint-lascivious/unbound-config/raw/master/binaries/aarch64/unbound-1.13.3.tar.gz
```
For armhf hosts
```
wget https://github.com/saint-lascivious/unbound-config/raw/master/binaries/armhf/unbound-1.13.3.tar.gz
```
```
tar -xf unbound-1.13.3.tar.gz
```
```
chmod +x install
```
```
./install
```

## Contact
* Discord
[SaintLascivious](https://discord.gg/9Cq4gRg)

* Email
saint.lascivious@gmail.com

* IRC
[##saint-lascivious](https://webchat.freenode.net/##saint-lascivious)

* Reddit
[saint-lascivious](https://www.reddit.com/user/saint-lascivious)

![alt text][logo]

[logo]:https://vignette.wikia.nocookie.net/pokemon/images/7/76/265Wurmple.png "Using the spikes on its rear end, Wurmple peels the bark off trees and feeds on the sap that oozes out. This Pokémon's feet are tipped with suction pads that allow it to cling to glass without slipping."
