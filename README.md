# namecheap-ddns-update
Update the IP address of your (namecheap.com)[namecheap.com] Dynamic DNS A records.

## Overview

## Running it

Check the help (-h) for details. Basic usages is as follows:
```
Usage: namecheap-ddns-update [-h] [-e] [-d DOMAIN] [-s SUBDOMAINS] [-i IP]
Update the IP address of one or more subdomains, of a domain you own at
namecheap.com. This can only update an existing A record, it cannot create
a new A record. Use namecheap's advanced DNS settings for your domain to
create A records. The args d, s and i have corresponding ENV options. The
Dynamice DNS Password has to be set with the NC_DDNS_PASS environment variable.

    -h             display this help and exit
    -e             exit if any call to update a subdomains IP address fails
    -d DOMAIN      the domain that has one or more subdomains (A records)
    -s SUBDOMAINS  comma separated list of subdomains (A records) to update
    -i IP          IP address to set the subdomain(s) to. If blank namecheap
                   will use the callers public IP address.
```

### With a known public IP
This example would update the IP address to 127.0.0.1, for the two A records abc and xyz under the domain example.com e.g. abc.mydomain.com and xyz.example.com
```
./namecheap-ddns-update -d example.com -s "abc,xyz" -i 127.0.0.1
```
### I don't know my public IP
This example would update the IP address to the callers public IP address, for the two A records abc and xyz under the domain example.com e.g. abc.mydomain.com and xyz.example.com. This is useful when you sit behind an IP address that can change e.g. home internet service provider who dynamically assigns you a public IP address
```
./namecheap-ddns-update -d example.com -s "abc,xyz"
```
