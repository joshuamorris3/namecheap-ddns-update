# namecheap-ddns-update
Update the IP address of your [namecheap.com](namecheap.com) Dynamic DNS A records.

[![Build Status](https://travis-ci.org/joshuamorris3/namecheap-ddns-update.svg?branch=master)](https://travis-ci.org/joshuamorris3/namecheap-ddns-update)

## Overview
Use this to update the IP address of A records for a domain that is hosted by [namecheap.com](namecheap.com). If you created one or more A records using [namecheap.com](namecheap.com) Dynamic DNS, then this will update the IP address to:
* The IP address you pass in as an argument using -i
* Or, if the -i is omitted or left blank, the IP address seen by [namecheap.com](namecheap.com)'s servers when you run this script. If you run this from within your network, then the externally visible IP address is used by [namecheap.com](namecheap.com) to update the A record value. If you have a server with a public IP address, then this utility can be run from that server and -i can be omitted.

## Running it

Check the help (-h) for details. The one argument that can only be set as an environment variable is NC_DDNS_PASS. The Dynamic DNS Password from [namecheap.com](namecheap.com)'s dashboard -> Advanced DNS page for the domain with the A records you want to update.

You can set the arguments in one of the following ways:

1. Pass them in on the command line
2. Set them as environment variables e.g. export DOMAIN=domain.tld, or DOMAIN=domain.tld ./namecheap-ddns-update ....
3. Create an environment file .env in the same directory as the script. This file is sourced if it is found and is readable e.g. source ./.env (the [.env.sample](.env.sample) provides an example)
4. Create an environment file called .namecheap-ddns-update, in the directory of the user running this script. This file is sourced if it is found and is readable e.g. source ~/.namecheap-ddns-update

Basic usages is as follows:
```
Usage: namecheap-ddns-update [-h] [-e] [-d DOMAIN] [-s SUBDOMAINS] [-i IP] [-t INTERVAL]
Update the IP address of one or more subdomains, of a domain you own at
namecheap.com. This can only update an existing A record, it cannot create
a new A record. Use namecheap's advanced DNS settings for your domain to
create A records. The args d, s and i have corresponding ENV options. The
Dynamice DNS Password has to be set with the NC_DDNS_PASS environment variable.
You could also create an environment file in the same directory as the script,
called .env, or in directory of the user running this script, called
.namecheap-ddns-update. The .env file is sourced first if found, if it does
not exist, then .namecheap-ddns-update sourced if found.

    -h             display this help and exit
    -e             exit if any call to update a subdomains IP address fails
    -d DOMAIN      the domain that has one or more subdomains (A records)
    -s SUBDOMAINS  comma separated list of subdomains (A records) to update
    -i IP          IP address to set the subdomain(s) to. If blank namecheap
                   will use the callers public IP address.
    -t INTERVAL    set up a interval at which to run this. Uses bash sleep
                   format e.g. NUMBER[SUFFIX] where SUFFIX can be, s for
                   seconds (default), m for minutes, h for hours, d for days
```

### With a known public IP
This example would update the IP address to 127.0.0.1, for the two A records abc and xyz under the domain example.com e.g. abc.mydomain.com and xyz.example.com
```
./namecheap-ddns-update -d example.com -s "abc,xyz" -i 127.0.0.1
```
### I don't know my public IP
This example runs every hour (1h) to update the IP address to the callers public IP address, for the two A records abc and xyz under the domain example.com e.g. abc.mydomain.com and xyz.example.com. This is useful when you sit behind an IP address that can change e.g. home internet service provider who dynamically assigns you a public IP address
```
./namecheap-ddns-update -d example.com -s "abc,xyz" -t 1h
```

### Docker

This docker container should be available from _joshuamorris3/namecheap-ddns-update_

```
docker run -e "NC_DDNS_PASS=123456" -e "DOMAIN=example.com" -e "SUBDOMAINS=abc,xyz" -e "INTERVAL=10s" -d --name nc-ddns joshuamorris3/namecheap-ddns-update
```

### Docker compose instructions

Docker compose is a convienient way to start docker containers. The [docker-compose.yml](docker-compose.yml) provides an example, it uses a file called .env (see [.env.sample](.env.sample) for an example) to configure the namecheap-ddns-update process.

To start the process using docker compose, simply run:
```
docker-compose up -d
```

### Local docker instructions

You can also run this from within a Docker container.

Build your namecheap-ddns-update docker image
```
docker build -t namecheap-ddns-update -f Dockerfile .
```

Run the image you just built, passing in the environment variables to configure the script
```
docker run -e "NC_DDNS_PASS=123456" -e "DOMAIN=example.com" -e "SUBDOMAINS=abc,xyz" -e "INTERVAL=10s" -d --name nc-ddns namecheap-ddns-update
```
