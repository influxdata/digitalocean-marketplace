# InfluxData images for DigitalOcean Marketplace

This repository contains the source scripts for building InfluxData's listings
on the DigitalOcean Marketplace. Documentation for partners using the
DigitalOcean Marketplace can be found
[here](https://github.com/digitalocean/marketplace-partners).

## Listings

1. InfluxDB (TICK stack)
    - This is a single image that contains the open source TICK stack
      components: Telegraf, InfluxDB, Chronograf, Kapacitor.

## Building the images

[Hashicorp's Packer](https://www.packer.io/docs/builders/digitalocean.html) is
used to build all DigitalOcean Marketplace images.

Before running Packer, you will need to acquire a DigitalOcean API token and set
it as the value for a `DIGITALOCEAN_API_TOKEN` environment variable. (Note the
DO account under the pilots@errplane.com address contains the official
Marketplace images.)

To run Packer and build an
image, switch to the directory of the listing you wish to build and run the
following command.

```sh
export DIGITALOCEAN_API_TOKEN=xxxx
packer build <template>.json
```
