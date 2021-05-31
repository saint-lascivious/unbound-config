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
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/base.conf
```

* Download any additional config file fragments as required:
Access Control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/access-control.conf
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
Libevent
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/libevent.conf
```
Local Records
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/local-records.conf
```
Multithreaded UDP
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/multithreaded-udp.conf
```
Multithreading
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/multithreading.conf
```
Cache Prefetch (Recommended)
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
Note: Unbound must be compiled with both --with-libhiredis and --enable-cachedb flags enabled.
Check if your version supports this with 'unbound -V', it probably doesn't.
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/redis.conf
```
Remote Control
```   
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/remote-control.conf
```
Root Hints
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/root-hints.conf
```
Safe EDNS Buffer (Reccomended)
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/safe-edns-buffer.conf
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
I have compiled Unbound (and its associated toolset) from [source](https://github.com/NLnetLabs/unbound), with some additional features which may not be present in some distribution packages.

Output from "unbound -V":
```
Version 1.13.2

Configure line: --build=aarch64-linux-gnu --prefix=/usr --includedir=${prefix}/include --mandir=${prefix}/share/man --infodir=${prefix}/share/info --sysconfdir=/etc --localstatedir=/var --disable-option-checking --disable-silent-rules --libdir=${prefix}/lib/aarch64-linux-gnu --libexecdir=${prefix}/lib/aarch64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --disable-rpath --with-pidfile=/run/unbound.pid --with-rootkey-file=/var/lib/unbound/root.key --with-libevent --with-pythonmodule --enable-subnet --enable-dnstap --enable-systemd --with-chroot-dir= --with-dnstap-socket-path=/run/dnstap.sock --libdir=/usr/lib --disable-flto --enable-tfo-client --enable-tfo-server --with-libhiredis --enable-cachedb
Linked libs: libevent 2.1.12-stable (it uses epoll), OpenSSL 1.1.1j  16 Feb 2021
Linked modules: dns64 python cachedb subnetcache respip validator iterator
TCP Fastopen feature available
```
This is (very deliberately) almost identical to the Debian/Ubuntu unbound binary package configuration.

It is safe, but not necessarily recommended to replace the system unbound binaries with those provided.
Any updates to the system package will remove the custom binary.

* What do they run on?
At the present at least, only aarch64 binaries are provided.

* Will you continue to update these binaries?
Probably, yes.

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
