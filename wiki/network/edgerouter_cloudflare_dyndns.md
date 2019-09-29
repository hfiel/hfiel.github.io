# Using Cloudflare API to update a DNS A record with Edgerouter DDNS service

All this is already on the internet, and is a mix of several sources, but specially I put it here because of the "server" option, that caused me some headache.

NOTE: the A record (example.domain.com) MUST exists in Cloudflare before setting this.
You also need the Global API KEY from Cloudflare.

## Asumptioms and clarifications

   -  Your internet interface (i.e., the one with your changing public IP address) is eth0.
    - NOTE: If your ISP provides your service through a VLAN, it may be something like eth0.10. Please verify in the dashboard which interface name you need.
   - Your Cloudflare subdomain is example.domain.com
   - Your Cloudflare zone is domain.com
   - Your Cloudflare username is myuser@cloudflare.com
   - Your Cloudflare Global API KEY is 1234567abcdefg1234567   
   - The name we will define for the service in the router is "custom-cloudflare"
   - You know how to connect to your router by SSH

## Commands for CLI (SSH)

```
configure

set service dns dynamic interface eth0 service custom-cloudflare host-name example.domain.com

set service dns dynamic interface eth0 service custom-cloudflare login myuser@cloudflare.com

set service dns dynamic interface eth0 service custom-cloudflare password 1234567abcdefg1234567

set service dns dynamic interface eth0 service custom-cloudflare protocol cloudflare

set service dns dynamic interface eth0 service custom-cloudflare server api.cloudflare.com/client/v4/

set service dns dynamic interface eth0 service custom-cloudflare options "zone=domain.com use=web ssl=yes"

commit

save
```

## Verify status

```
show dns dynamic status
```

## Force an update

```
update dns dynamic interface eth0
```
