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

The images can be built using fabric as shown in the [DO Marketplace docs](https://github.com/digitalocean/marketplace-partners/blob/master/marketplace_docs/build-an-image-fabric.md).

Spin up a build droplet to use to run fabric and an image droplet to use as the target for fabric, which will be used to create the image. (Fabric can be run from your local system, but a new DO environment is generally cleaner to work with.)

Connect via SSH to the build droplet, then [install fabric](http://www.fabfile.org/).

Next, generate a new SSH using `ssh-keygen` and copy the public key to `~/.ssh/authorized_keys` file on the image droplet.

Clone this repository on the build droplet and switch into this directory (`influxdb-tick/`). Then run fabric to prepare the image droplet.

```sh
fab build_image -H 104.248.179.242
```

Now connect to the image droplet and run the [`img_check.sh`](https://raw.githubusercontent.com/digitalocean/marketplace-partners/master/marketplace_validation/img_check.sh) script and confirm there are no issues reported before proceeding. Remove the `img_check.sh` script.

Finally, turn off the image droplet using `shutdown -h now`. In the DO web console, open the detail page for the image droplet. Navigate to the snapshots section and take a new snapshot. This creates the snapshot that will be submitted to DO.

The DO team will need the snapshot ID, which can be found by running the following command against the DO API using [`doctl`](https://github.com/digitalocean/doctl). (Note: `doctl` requires an access token when authenticating which is in 1Password)

```sh
doctl compute image list-user
```

Submit the full text of the line that contains the new snapshot.
