# unbound-config

configuration files for [unbound](https://nlnetlabs.nl/projects/unbound/about/) recursive dns resolver

Settings and values have been derived in part by some suggested
defaults in [docs.pi-hole.net/guides/unbound](https://docs.pi-hole.net/guides/unbound/) in order to avoid
some possible interoperability issues and also from the documentation
found at [nlnetlabs.nl/documentation/unbound/howto-optimise](https://nlnetlabs.nl/documentation/unbound/howto-optimise/) 
specifically regarding optimising unbound

## Usage
* Install dependencies
[unbound](https://packages.debian.org/buster/unbound)
[libevent-dev](https://packages.debian.org/buster/libevent-dev)
```
apt-get install unbound libevent-dev
```

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
