# snort.lua settings used

Main configuration file: `/usr/local/etc/snort/snort.lua`
Custom rules file: `/usr/local/etc/snort/rules/local.rules`

```lua
HOME_NET = '192.168.56.0/24'
```

The custom rules file (`local.rules`) was added to the Snort configuration, and the
configuration was validated with:

```bash
sudo snort -T -c /usr/local/etc/snort/snort.lua -i enp0s3
```

Result: `Snort successfully validated the configuration (with 0 warnings).`
(222 rules loaded; 1 custom rule each for TCP, UDP and ICMP.)
