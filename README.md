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
* Remove pi-hole.conf if you deployed unbound using [Pi-hole documentation](https://docs.pi-hole.net/guides/unbound/)
```
sudo rm /etc/unbound/unbound.conf.d/pi-hole.conf
```
* Download the base config file:
Switch to the /etc/unbound/unbound.conf.d/ directory
```
cd /etc/unbound/unbound.conf.d/
Download the base config file
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/unbound.conf
```
* Download additional config files as required:

Access control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-access-control.conf
```
Modify cache TTL
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-cache-min-ttl.conf
```
Use capitalization randomization
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-caps-for-id.conf
```
Serve expired records
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-expired-records.conf
```
Extended statistics for unbound-control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-extended-statistics.conf
```
Listen on ipv6
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-ipv6.conf
```
Use large buffers
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-large-buffers.conf
```
Use libevent
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-libevent.conf
```
Multithreaded udp
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-multithreaded-udp.conf
```
Large caches
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-optimized-caches.conf
```
Multithreading
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-optimized-threads.conf
```
Custom server identity
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-own-identity.conf
```
User managed root.hints
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-own-root-hints.conf
```
Cache prefetching
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-prefetch.conf
```
Rate limiting
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-rate-limiting.conf
```
Safe EDNS buffer
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-safe-edns-buffer.conf
```
Use unbound-control
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-unbound-control.conf
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
