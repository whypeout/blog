## Linux

# Integrating Local Area Networks

Up to now, we’ve focused on constructing a point-to-point Mesh network, enabling nodes to connect directly through WireGuard’s private network IPs. However, we can aim higher. Recall our initial discussion about accessing in-home network resources? We have the capability to configure each node to access local area network IPs of other nodes. This functionality opens up numerous possibilities. For instance, you can access NAS systems on your home network or any internal network service. **This can be particularly beneficial for advanced users who wish to access Pod IPs and Service IPs within Kubernetes clusters on cloud services.**

Let’s say your home network includes a Linux device (such as OpenWrt) equipped with the Tailscale client. The goal is for other Tailscale clients to effortlessly access any device on your home network via its local area network IP, like **192.168.100.0/24**.

The configuration process is straightforward. Your first step is to set up IPv4 and IPv6 routing:

$ echo 'net.ipv4.ip_forward = 1' | tee /etc/sysctl.d/ipforwarding.conf  
$ echo 'net.ipv6.conf.all.forwarding = 1' | tee -a /etc/sysctl.d/ipforwarding.conf  
$ sysctl -p /etc/sysctl.d/ipforwarding.conf

To modify the client command for updating registration nodes, add `--advertise-routes=192.168.100.0/24` to the base command. This parameter signals the Headscale server that "this node can reroute to these specified addresses."

$ tailscale up --login-server=http://<HEADSCALE_PUB_ENDPOINT>:8080 --accept-routes=true --accept-dns=false --advertise-routes=192.168.100.0/24 --reset

Checking the routes on the Headscale end reveals that these routes are initially disabled.

$ headscale nodes list|grep openwrt

6 | openwrt | [7LdVc] | default | 100.64.0.6 | false | 2022-03-20 15:50:46 | online | no  
$ headscale routes list -i 6  
Route | Enabled  
192.168.100.0/24 | false

To enable a specific route:

$ headscale routes enable -i 6 -r "192.168.100.0/24"

Route | Enabled  
192.168.100.0/24 | true

For activating multiple routes, use commas to separate them:

$ headscale routes enable -i 6 -r "192.168.100.0/24,xxxx"

To enable all routes at once, use the `-a` option:

$ headscale routes enable -i 6 -a

Other nodes can inspect the routing:

$ ip route show table 52|grep "192.168.100.0/24"  
192.168.100.0/24 dev tailscale0

For these nodes to accept external routes, they must start with the `--accept-routes=true` option, indicating "I accept routes from other nodes."

Now, from any Tailscale client node, you can ping any device within your home network, just as if you were there. Whether at the office or a cafe, you can easily access your home devices using the same IP address, offering remarkable convenience.