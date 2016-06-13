# Overthebox

BRANCH WITH 0.3.7 working on PI2.

Overthebox is an open source solution developed by OVH to aggregate and encrypt multiple internet connections and terminates it over OVH/Cloud infrastructure which make clients benefit security, reliability, net neutrality, as well as dedicated public IP.

The aggregation is based on MPTCP, which is ISP, WAN type, and latency independent "whether it was Fiber, VDSL, SHDSL, ADSL or even 4G, ", different scenarios can be configured to have either aggregation, load-balancing or failover based on MPTCP or even Openwrt mwan3 package.

The solution takes advantage of the latest Openwrt system, which is user friendly and also the possibility of installing other packages like VPN, QoS, routing protocols, monitoring, etc. through web-interface or terminal.


More information is available here :
[https://www.ovhtelecom.fr/overthebox/](https://www.ovhtelecom.fr/overthebox/)


## Prerequisite

* PI2


## Install from pre-compiled images

The version 0.3.7 has been successfull released for the PI2 downloads follows

### PI2 image :
[http://www.bruni68510.me/overthebox/0.3.7/openwrt-brcm2708-bcm2709-rpi-2-b-ext4-sdcard.bin.gz](http://www.bruni68510.me/overthebox/0.3.7/openwrt-brcm2708-bcm2709-rpi-2-b-ext4-sdcard.bin.gz)


## Compile from source

First, you need to clone our patched version of Openwrt which is available on github: [https://github.com/bruni68510/overthebox-openwrt](https://github.com/bruni68510/overthebox-openwrt)


### Preparation

```shell
git clone https://github.com/ovh/overthebox-openwrt.git
cd overthebox-openwrt
cp feeds.conf.default feeds.conf
echo src-git overthebox https://github.com/bruni68510/overthebox-feeds.git >> feeds.conf
./scripts/feeds update -a
./scripts/feeds install -a -p overthebox
./scripts/feeds install -p overthebox -f netifd
./scripts/feeds install -p overthebox -f dnsmasq
./scripts/feeds install -a
```


### Compile 

```shell
make -j9 V=s
```


### if compilation fails

it happens :) Please try to recompile with -j1 to see the error.

```shell
make -j1 V=s
```


### Compilation issues encountered 

#### ntpd and libevent 

"OpenWrt-libtool: link: cannot find the library `../sntp/libevent/libevent_core.la' or unhandled argument `../sntp/libevent/libevent_core.la'" 
is due to an error in order of compilation

Fix :

```
    make package/libevent/compile
    make package/ntpd/clean
    make package/ntpd/compile
    make V=s -j9
``` 


#### bmon

From some reason, bmon does not want to compile

Fix:

```
sed -i "s/^CONFIG_PACKAGE_bmon=.*/# CONFIG_PACKAGE_bmon is not set/" .config
```


## Credits

Our solution is mainly based on : 
* Openwrt : [https://openwrt.org](https://openwrt.org)
* Multipath TCP : [https://multipath-tcp.org](https://multipath-tcp.org)
