VPN ala ala Wireguard

sumber: 
- [https://medium.com/@carson_yang/headscale-deployment-and-usage-guide-mastering-tailscales-self-hosting-basics-48d26d7fa9d8](https://medium.com/@carson_yang/headscale-deployment-and-usage-guide-mastering-tailscales-self-hosting-basics-48d26d7fa9d8)\
- [https://github.com/juanfont/headscale/blob/main/docs/setup/install/official.md](https://github.com/juanfont/headscale/blob/main/docs/setup/install/official.md)
- [server headscale pake nginx reverse proxy, jadi pake domain bukan ip dan port](https://computingforgeeks.com/install-and-configure-headscale-on-ubuntu/)


## Tailscale sengertiku

Tailscale itu semacam vpn yg setelah kita konek, koneksi kita tidak ter pusat di 1 server ,melainkan kita menggunakan client vpn lain sbg peer
Peer ini kita bisa gunakan sebagai Exit-Gateway untuk mencapai internet, jadi kita diibaratkan pake internet orang lain, bukan internet kita,bukan internet dari server vpn.
Peer ini juga bisa kita gunakan sebagai LAN Access, jadi kita bisa akses jaringan LAN pada sisi client, seperti Site-to-Site atau Client-to-Site tapi site disini bukan hanya pada sisi server VPN.


## install tailscale di linux

umumnya kita pakai tailscale di server linux yang ada di server VPS sbg central/pusat semua akses client nantinya konek/otentikasi ke server ini

```
# download latest versionnya aja
$ wget --output-document=/usr/local/bin/headscale \  
https://github.com/juanfont/headscale/releases/download/v<HEADSCALE VERSION>/headscale_<HEADSCALE VERSION>_linux_<ARCH>  

# beri execute permission pada bin filenya
$ chmod +x /usr/local/bin/headscale

# bikin path config
$ sudo mkdir -p /etc/headscale

# bikin path utk simpan data & certificates
$ sudo mkdir -p /var/lib/headscale

# bikin file sqlite database kosong; selain sqlite kita bisa pakai postgres juga, tapi kita pake sqlite aja yg simple dulu hehe
$ touch /var/lib/headscale/db.sqlite

# download config headscale
$ wget https://github.com/juanfont/headscale/raw/main/config-example.yaml -O /etc/headscale/config.yaml
```

untuk confignya kita perlu rubah di:
- `server_url`: pake ip public / domain. misal `http://vpn.domainku.com`
- `magic_dns`: fungsinya untuk me-resolve dns yang diberikan oleh tail-net; bikin aja `false` biar domain lokal kita gak ketimpa
- `randomize_client_port`: bikin jadi `true` aja
- `ip_prefixes:` enable/uncomment ipv4 aja `- 100.64.0.0/16`
- `unix_socket`: isikan `/var/run/headscale/headscale.sock`

lanjut bikin systemd configuration biar servicenya bisa jalan setiap server reboot

```
# /etc/systemd/system/headscale.service  
[Unit]  
Description=headscale controller  
After=syslog.target  
After=network.target  
  
[Service]  
Type=simple  
User=headscale  
Group=headscale  
ExecStart=/usr/local/bin/headscale serve  
Restart=always  
RestartSec=5  
# Optional security enhancements  
NoNewPrivileges=yes  
PrivateTmp=yes  
ProtectSystem=strict  
ProtectHome=yes  
ReadWritePaths=/var/lib/headscale /var/run/headscale  
AmbientCapabilities=CAP_NET_BIND_SERVICE  
RuntimeDirectory=headscale  
  
[Install]  
WantedBy=multi-user.target

```

buat headscale user, group dan permission

```
useradd headscale -d /home/headscale -m
chown -R headscale:headscale /var/lib/headscale
```

restart daemon systemctl, enable & start daemon headscale

```
systemctl daemon-reload
systemctl enable --now headscale
systemctl status headscale
```

cek port 

```
ss -tulpn|grep headscale
tcp    LISTEN   0        4096            127.0.0.1:8888           0.0.0.0:*      users:(("headscale",pid=1862000,fd=13))
tcp    LISTEN   0        4096            127.0.0.1:8087           0.0.0.0:*      users:(("headscale",pid=1862000,fd=14))
```

## User tailscale

```
# buat user
headscale user create mis

# list user
headscale user list

```


## Install dashboard

paling enak emang pake dashboard sekarang bro, semua tinggal klak klik doang; [https://github.com/GoodiesHQ/headscale-admin](https://github.com/GoodiesHQ/headscale-admin)

```
https://github.com/GoodiesHQ/headscale-admin

# bikin apikey
headscale apikey create

```

## install tailscale client di linux

```
# download
$ wget https://pkgs.tailscale.com/stable/tailscale_1.22.2_amd64.tgz

# extract
$ tar zxvf tailscale_1.22.2_amd64.tgz  
x tailscale_1.22.2_amd64/  
x tailscale_1.22.2_amd64/tailscale  
x tailscale_1.22.2_amd64/tailscaled  
x tailscale_1.22.2_amd64/systemd/  
x tailscale_1.22.2_amd64/systemd/tailscaled.defaults  
x tailscale_1.22.2_amd64/systemd/tailscaled.service

# install ke system
$ cp tailscale_1.22.2_amd64/tailscaled /usr/sbin/tailscaled  
$ cp tailscale_1.22.2_amd64/tailscale /usr/bin/tailscale

# bikin services
$ cp tailscale_1.22.2_amd64/systemd/tailscaled.service /lib/systemd/system/tailscaled.service

# environment
$ cp tailscale_1.22.2_amd64/systemd/tailscaled.defaults /etc/default/tailscaled

# start service
$ systemctl daemon-reload
$ systemctl enable --now tailscaled
$ systemctl status tailscaled
```

## konekin tailscale client ke server headscale

```
$ tailscale up --login-server=http://<HEADSCALE_PUB_ENDPOINT>:8080 --accept-routes=true --accept-dns=false


```

![[0_k5s6g8h2-f4rWbWR.webp]]

## register machine dari server headscale

```
$ headscale nodes register --user mis --key 905cf165204800247fbd33989dbc22be95c987286c45aac3033937041150d846  
Machine register


$ headscale nodes list  
```

| ID  | Name    | NodeKey | Namespace | IP addresses | Ephemeral | Last seen           | Online | Expired |
| --- | ------- | ------- | --------- | ------------ | --------- | ------------------- | ------ | ------- |
| 1   | coredns | [Ew3RB] | default   | 100.64.0.1   | false     | 2022-03-20 09:08:58 | online | no      |

balik ke client 

```
$ ip route show table 52

$ iptables -S  
-P INPUT DROP  
-P FORWARD ACCEPT  
-P OUTPUT ACCEPT  
-N ts-forward  
-N ts-input  
-A INPUT -j ts-input  
-A FORWARD -j ts-forward  
-A ts-forward -i tailscale0 -j MARK --set-xmark 0x40000/0xffffffff  
-A ts-forward -m mark --mark 0x40000 -j ACCEPT  
-A ts-forward -s 100.64.0.0/10 -o tailscale0 -j DROP  
-A ts-forward -o tailscale0 -j ACCEPT  
-A ts-input -s 100.64.0.5/32 -i lo -j ACCEPT  
-A ts-input -s 100.115.92.0/23 ! -i tailscale0 -j RETURN  
-A ts-input -s 100.64.0.0/10 ! -i tailscale0 -j DROP  

$ iptables -S -t nat  
-P PREROUTING ACCEPT  
-P INPUT ACCEPT  
-P OUTPUT ACCEPT  
-P POSTROUTING ACCEPT  
-A ts-postrouting -m mark --mark 0x40000 -j MASQUERADE
```


## nginx proxy untuk headscale

mapping ip dan domain: `vpn.domainku.com` ke `103.54.218.250`.

```
$ host vpn.domainku.com
vpn.domainku.com has address 103.54.218.250
```

install nginx

```
sudo apt update -y
sudo apt install nginx
sudo nano /etc/nginx/sites-available/headscale.conf

map $http_upgrade $connection_upgrade {
  default keep-alive;
  'websocket' upgrade;
  '' close;
}

server {
  listen 80;
  listen [::]:80;
  server_name vpn.domainku.com;
  location / {
    proxy_pass http://127.0.0.1:8888;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_set_header Host $server_name;
    proxy_redirect http:// https://;
    proxy_buffering off;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $http_x_forwarded_proto;
    add_header Strict-Transport-Security "max-age=15552000; includeSubdomains" always;
  }
}

sudo nginx -t
sudo nano /etc/headscale/config.yaml

server_url: http://vpn.domainku.com:80
listen_url: 127.0.0.1:8888

sudo systemctl restart headscale

sudo apt update && sudo apt upgrade -y
sudo apt install snapd
sudo snap install --classic certbot

DOMAIN=vpn.domainku.com
sudo certbot --register-unsafely-without-email --agree-tos --nginx -d vpn.domainku.com 

# the nginx config will be updated with the certificate, cek & confirm
cat /etc/nginx/sites-enabled/headscale.conf

systemctl restart nginx

# update headscale config
nano /etc/headscale/config.yaml
server_url: https://vpn.domainku.com:443

sudo systemctl restart headscale

# note, tanpa nginx kita bisa expose headscale langsung dg ssl
nano /etc/headscale/config.yaml
tls_cert_path: ""
tls_key_path: ""

# for custom ssl, update nginx config
map $http_upgrade $connection_upgrade {
	default keep-alive;
	'websocket' upgrade;
	'' close;
}
server {
	listen 80;
	listen [::]:80;
	listen 443 ssl http2;
	listen [::]:443 ssl http2;
	server_name _your_fqdn_;
	ssl_certificate _path_to_cert_;
	ssl_certificate_key _PATH_CERT_KEY;
	ssl_protocols TLSv1.2 TLSv1.3;
	location / {}
}
```
