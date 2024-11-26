- [https://vpn.domainku.com/windows](https://vpn.domainku.com/windows)

pake preauthkeys, expire setelah 24 jam, untuk user default
```
$ headscale preauthkeys create -e 24h --user default

$ headscale --user default preauthkeys list  
ID | Key | Reusable | Ephemeral | Used | Expiration | Created  
1 | 57e419c40e30b0dxxxxxxxf15562c18a8c6xxxx28ae76f57 | false | false | false | 2022-05-30 07:14:17 | 2022-05-29 07:14:17
```
via admin ?
![[0_pSaJT2LsR6ZmOd26.webp]]
In the interface that appears, hit `Create` next to "PreAuth Keys," set an expiration time (such as 100 years), tick `Reusable` if you plan to reuse this Key, and then click ✅:
![[0_LyyeWP0xDDBMP1h6.webp]]
After creation, copy the PreAuth Key by clicking the highlighted area in red:
![[0_IZm9FKfu6cmOeSDr.webp]]
With this, new nodes can effortlessly connect directly, bypassing the need for server approval:
```
$ tailscale up --login-server=http://<HEADSCALE_PUB_ENDPOINT>:8080 --accept-routes=true --accept-dns=false --authkey $KEY
```
