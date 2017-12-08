## cdituri/openconnect

`cdituri/openconnect` is a lightweight image for connecting to Cisco AnyConnect and Juniper SSL-based VPNs.

This image achieves its lightweight nature by leveraging alpine linux, multi-stage docker builds, and direct compilation from `openconnect` source.

Please see http://www.infradead.org/openconnect/ for more information on openconnect.


### building
```
docker build -t cdituri/openconnect .
```

### usage

In order to route **all** host traffic through the VPN tunnel--and not just the container's traffic--this image must be run privileged.

In other words, it creates a `tun` interface on the host and manages the routing tables via the hosts' procfs.

```
docker run -it --rm --net=host --privileged cdituri/openconnect -u <short_username> sslvpn.example.com
```

Simply `ctrl-c` to kill the tunnel - your credentials will never hit the disk.
