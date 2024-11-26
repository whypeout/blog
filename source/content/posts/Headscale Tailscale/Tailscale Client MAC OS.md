
sumber:
- [https://medium.com/@carson_yang/headscale-deployment-and-usage-guide-mastering-tailscales-self-hosting-basics-48d26d7fa9d8](https://medium.com/@carson_yang/headscale-deployment-and-usage-guide-mastering-tailscales-self-hosting-basics-48d26d7fa9d8)

tailscale untuk macos bisa diinstall dari 3 sumber:
- app store, khusus region US
- situs resmi [tailscale](https://pkgs.tailscale.com/stable/#macos)
- github tailscale [github repository](https://github.com/tailscale/tailscale/wiki/Tailscaled-on-macOS)

1. tekan `ALT` sambil click icon di top menu bar (samping network/wifi), arahkan ke `Debug`
   ![[0_aEt_R8fDqE6NyWgi.webp]]
2. pilih `add account`, pilih `custom login server`
3. masukin alamat server `https://vpn.domainku.com`, `add account`
   ![[0_9_OsYttnZZxhRp0s.webp]]
4. register machine di server vpn
   ![[0_yTqPPOBSWrDE1nR9.webp]]

cek di server
```
$ headscale nodes list
```

| ID  | Name                | NodeKey | Namespace | IP addresses | Ephemeral | Last seen           | Online | Expired |
| --- | ------------------- | ------- | --------- | ------------ | --------- | ------------------- | ------ | ------- |
| 1   | coredns             | [Ew3RB] | default   | 100.64.0.1   | false     | 2022-03-20 09:08:58 | online | no      |
| 2   | carsondemacbook-pro | [k7bzX] | default   | 100.64.0.2   | false     | 2022-03-20 09:48:30 | online | no      |


dari mac, cek ping:
```
$ ping -c 2 100.64.0.1  
PING 100.64.0.1 (100.64.0.1): 56 data bytes  
64 bytes from 100.64.0.1: icmp_seq=0 ttl=64 time=37.025 ms  
64 bytes from 100.64.0.1: icmp_seq=1 ttl=64 time=38.181 ms  
  
--- 100.64.0.1 ping statistics ---  
2 packets transmitted, 2 packets received, 0.0% packet loss  
round-trip min/avg/max/stddev = 37.025/37.603/38.181/0.578 ms
```

or ping dari tailscale app
```
$ /Applications/Tailscale.app/Contents/MacOS/Tailscale ping 100.64.0.1  
pong from coredns (100.64.0.1) via xxxx:41641 in 36ms
```

