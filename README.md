# WRT32XRouterConfig
This is configuration of openwrt for FRPS etc

## Install Noip.com DDNS
Under software install luci-app-ddns and ddns-scripts-noip <br/>
Service Menu will appear, just configure accordingly

## Modify web interface to port 8081
Edit the file in /etc/config/uhttpd from port 80 to 8081

## Open Ports and Port forwarding
Under Firewall Menu /Port forward 
```
WAN TCP port 443 -> 192.168.1.100 port 5333
```
Under Traffic Rules, open port 80, 995 and 998:

```
Incoming IPV4 TCP From WAN port 80 to this device port 80 
Incoming IPV4 TCP From WAN port 995 to this device port 995
Incoming IPV4 TCP From WAN port 998 to this device port 998
```

## Install frps
Download binary with
```
wget https://github.com/fatedier/frp/releases/download/v0.40.0/frp_0.40.0_linux_arm.tar.gz
tar -zxvf frp_0.40.0_linux_arm.tar.gz
cd frp_0.40.0_linux_arm
cp frps /usr/bin
cp frps.ini /usr/bin
vi /etc/init.d/frps
```

Paste the following content in /etc/init.d/frps file:

```
#!/bin/sh /etc/rc.common
START=98

start() {
        /usr/bin/frps -c /usr/bin/frps.ini
}

stop() {
        killall frps
        return 0
}
```
Execute this
```
chmod +x /etc/init.d/frps
/etc/init.d/frps enable
```

## Install Openvpn
Under Software, install openvpn-openssl and luci-app-openvpn.
OpenVPN menu will appear

Reboot


# For Default WRT32X
change /etc/config/uhttpd
```
80 to 8080
443 to 943
```
add /etc/config/firewall with:
```
config rule
        option src 'wan'
        option dest '*'
        option dest_port '995'
        option proto 'tcp'
        option target 'ACCEPT'

config rule
        option src 'wan'
        option dest '*'
        option dest_port '998'
        option proto 'tcp'
        option target 'ACCEPT'
config rule
        option src 'wan'
        option dest '*'
        option dest_port '80'
        option proto 'tcp'
        option target 'ACCEPT'

```

Paste the following content in /etc/init.d/frps file:

```
#!/bin/sh /etc/rc.common
START=98

start() {
        /usr/bin/frps -c /usr/bin/frps.ini
}

stop() {
        killall frps
        return 0
}
```
Execute this
```
chmod +x /etc/init.d/frps
/etc/init.d/frps enable
```
