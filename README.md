## Uniria UFW Firewall On Linux
### For Debian And Another Dist
![Github Stars](https://img.shields.io/github/stars/Unknown-user-dev/Uniria-UFW?style=flat-square)
![GitHub issues](https://img.shields.io/github/issues-raw/Unknown-user-dev/Uniria-UFW?style=flat-square)

```
The Firewall Config Feat Unira UFW; ⓒ Unknown User
```

### Features

✅ Limit Connection per Class C

✅ Stable

✅ Limit connections per IP

✅ Limit Packets per IP

### Installation

Edit /etc/ufw/before.rules, putting each part where it belongs

```
### Add those lines after *filter near on the beginning 
:ufw-http - [0:0]
:ufw-http-logdrop - [0:0]



### Add those lines near the end of the file

### Start HTTP With UFW ###

# Enter rule Uniria
-A ufw-before-input -p tcp --dport 80   -j ufw-http
-A ufw-before-input -p tcp --dport 443  -j ufw-http

# Limit connections per Class C with logdrop
-A ufw-http -p tcp --syn -m connlimit --connlimit-above 50 --connlimit-mask 24 -j ufw-http-logdrop

# Limit connections per IP with logdrop
-A ufw-http -m state --state NEW -m recent --name conn_per_ip --set
-A ufw-http -m state --state NEW -m recent --name conn_per_ip --update --seconds 10 --hitcount 20 -j ufw-http-logdrop

# Limit packets per IP
-A ufw-http -m recent --name pack_per_ip --set
-A ufw-http -m recent --name pack_per_ip --update --seconds 1  --hitcount 20  -j ufw-http-logdrop

# Finally accept the request
-A ufw-http -j ACCEPT

# Log-A ufw-http-logdrop -m limit --limit 3/min --limit-burst 10 -j LOG --log-prefix "[UFW HTTP DROP] "
-A ufw-http-logdrop -j DROP

### End HTTP ###
```
### Testing the results

Make sure UFW runs to test the results with a ddos service

Great success.

> Made with ❤ by @Unknown User
