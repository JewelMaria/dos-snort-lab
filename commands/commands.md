# Commands used

Run only inside an isolated lab network that you own.

## Ubuntu (target and Snort sensor, 192.168.56.113, interface enp0s3)

```bash
ip addr
snort -V
sudo cat /usr/local/etc/snort/rules/local.rules
sudo snort -T -c /usr/local/etc/snort/snort.lua -i enp0s3          # validate config
sudo snort -c /usr/local/etc/snort/snort.lua -i enp0s3 -A alert_fast   # run Snort (passive IDS)
sudo tcpdump -ni enp0s3 'tcp[tcpflags] & tcp-syn != 0' -c 10        # verify SYN packets
sudo tcpdump -ni enp0s3 udp -c 10                                    # verify UDP packets
sysctl net.ipv4.icmp_ratelimit net.ipv4.icmp_ratemask               # ICMP rate-limit settings
```

## Kali Linux (traffic source, 192.168.56.102, interface eth0)

```bash
ip addr
ping -c 4 192.168.56.113
sudo hping3 -1 -c 10 192.168.56.113                       # ICMP baseline (about 1 packet/s)
sudo hping3 -1 -c 25 -i u50000 192.168.56.113             # ICMP burst (20 packets/s)
sudo hping3 -S -p 80 -c 25 -i u50000 192.168.56.113       # TCP SYN burst
sudo hping3 -2 -p 53 -c 25 -i u50000 192.168.56.113       # UDP burst
sudo hping3 -S -p 80 -c 10 -i u100000 192.168.56.113      # SYN, for tcpdump verification
sudo hping3 -2 -p 53 -c 10 -i u100000 192.168.56.113      # UDP, for tcpdump verification
```

`-c 25` sends 25 packets. `-i u50000` waits 50,000 microseconds between packets (20 per second).
