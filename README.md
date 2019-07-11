# InfluxData images for DigitalOcean Marketplace

This repository contains the source scripts for building InfluxData's listing
on the DigitalOcean Marketplace. Documentation for partners using the
DigitalOcean Marketplace can be found
[here](https://github.com/digitalocean/marketplace-partners).

## Listing

1. InfluxDB (TICK stack)
    - This is a single image that contains the open source TICK stack
      components: Telegraf, InfluxDB, Chronograf, Kapacitor.

## Updating the listing

Briefly review the changes to the [`packer`](https://github.com/digitalocean/marketplace-partners/tree/master/packer) directory and compare them to the contents of the `packer` directory in this repository.
Specifically, check whether there have been any updates to the the contents of the following files:

- `marketplace-image.json`
- `scripts/90-cleanup.sh`
- `scripts/99-img_check.sh`

The versions for the various TICK stack components should be updated in these files:

- `packer/scripts/01-test`
- `packer/docs.md`

## Building the image

Install [Packer](https://www.packer.io/), which is used to automatically build the image.

Set the DigitalOcean access token.

```
export DIGITALOCEAN_TOKEN=xxxx
```

Build the image with Packer.

```
packer build marketplace-image.json
```

## Submitting the listing

Resolve any issues with the Packer build reported before submitting the listing.

Lookup the ID of the new snapshot by running the following [`doctl`](https://github.com/digitalocean/doctl) command. (Note: `doctl` requires a DigitalOcean access token to be set to authenticate.)

```sh
doctl compute image list-user
```

Submit the full text of the line that contains the ID of the new snapshot to the DigitalOcean Marketplace team.
If the [application documentation](packer/docs.md) has been updated, that will also need to be submitted to the DigitalOcean Marketplace team.
