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
* Download the main config file:
```
cd /etc/unbound/unbound.conf.d/
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/unbound.conf
```
* Download additional config files as required:
```
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-expired-records.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-extended-statistics.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-ipv6.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-large-buffers.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-libevent.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-multithreaded-udp.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-optimized-caches.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-optimized-threads.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-own-identity.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-prefetch.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-safe-edns-buffer.conf
sudo wget https://raw.githubusercontent.com/saint-lascivious/unbound-config/master/use-unbound-control.conf
```
* Restart unbound
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
