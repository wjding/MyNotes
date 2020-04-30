# Hardening
## Netfilter
   1. Limit connection attempt rate

```shell
iptables -N FREQ_CHECK
iptables -A FREQ_CHECK -m recent --set --name DEFAULT --mask 255.255.240.0 --rsource
# no more than 2 requests in 1 minute
iptables -A FREQ_CHECK -m recent --update --seconds 60 --hitcount 3 --name DEFAULT --mask 255.255.240.0 --rsource -j DROP
iptables -A INPUT -p tcp -m tcp --dport 88 -m conntrack --ctstate NEW -j FREQ_CHECK
```
