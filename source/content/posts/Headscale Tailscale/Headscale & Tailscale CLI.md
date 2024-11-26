
## headscale CLI tips and trik

```
# user management
---
# user list
headscale user list

# user : create key
headscale --user mis preauthkeys create --reusable --expiration 24h

# user preauthkey
headscale --user mis preauthkeys list

---
# on client to connect
tailscale up --reset --login-server https://vpn.domainku.com --auth-key

# oidc enable login akan diarahkan ke halaman akun google, bisa dilimit dg domain @bosnetdis.com saja
tailscale up --login-server https://vpn.domainku.com --reset

# non oidc login akan ditampilkan halaman utk meng-copy paste command ke server
tailscale up --login-server https://vpn.domainku.com --reset

# on server to register machine/node
headscale nodes register --user <username> --key <machinekey>
headscale nodes register --user mis --key nodekey:adgasdgasdgasdfasdgadfagasdfasf

# server : node list
headscale node list

# server : node delete
headscale node delete -i <id>

# server : mode node to user
headscale node move -i <id> -u <new-user>

# server : rename node/machine
headscale node rename -i <id> <new-name>

# server : logout machine from server
headascale node expire -i <id>

# server : list pre auth, create & expire it
headscale preauthkeys list --user mis
headscale pre create --user mis --reusable --expiration 365d
headscale pre create --user mis --ephemeral --expiration 7d
headscale pre expire --user mis <key>

# server : apikeys for webapp
headscale apikeys create --expiration 90d
headscale api list
headscale api expire --prefix "<prefix>"

# server : routing - enable routes
headscale route list
headscale r en -r 3
headscale r ls

```
![[headscale-ui-1024x323.webp]]

