# InfluxDB (TICK stack) One-Click Application

The open source [TICK stack](https://docs.influxdata.com/platform/introduction), which includes [InfluxDB](https://docs.influxdata.com/influxdb/latest/), is a high performance platform to collect, store, visualize and act on [time-series data](https://www.influxdata.com/blog/what-is-time-series-data-and-why-should-you-care/) for DevOps metrics, IoT telemetry, and real-time analytics.

The four TICK stack components include:

- Telegraf, for collecting data
- InfluxDB, for storing time series data
- Chronograf, for graphs and dashboards
- Kapacitor for alerting on changes

DigitalOcean's InfluxDB (TICK stack) One-Click application by [InfluxData](https://www.influxdata.com/) includes all you need to quickly make beautiful dashboards, observe Kubernetes clusters, store syslog messages, and even monitor your smart home.

## Components

| Component  | Version        |
|------------|----------------|
| Linux      | Ubuntu 18.04.1 |
| Telegraf   | 1.9.4          |
| InfluxDB   | 1.7.2          |
| Chronograf | 1.7.7          |
| Kapacitor  | 1.5.2          |

In addition to the package installation, the One-Click also:

- Enables the UFW firewall to allow only SSH (port `22`, rate limited), HTTP (port `80`), and HTTP (port `8086`) access.
- InfluxDB is running on port 8086.
- Chronograf is running on port 80.
- The default InfluxDB admin password is saved in `/root/.digitalocean_password`.

## Quickstart

After the InfluxDB One-Click Droplet is created, you can view the Chronograf setup page immediately by visiting the Droplet’s IP address in your browser.

The first setup prompt requests the InfluxDB username and password. To retrieve this, connect to the Droplet via SSH. You can log into the Droplet as `root` using either the password emailed to you or with an SSH key, if you added one during creation.

```sh
# substitute "$droplet_ip" with the Droplet’s IP address
ssh root@$droplet_ip
```

Once connected to the Droplet, the InfluxDB username and password can be found in the `/root/.digitalocean_password` file.

```sh
sudo cat /root/.digitalocean_password
```

With the InfluxDB username and password in hand, return to the Chronograf setup page and continue. Kapacitor does not require a username or password.

Once Chronograf is configured, additional guides on how to use the UI can be found in the [Chronograf getting started guide](https://docs.influxdata.com/chronograf/v1.7/introduction/getting-started/).

Configuration details for each component can be found in:

- `/etc/telegraf/telegraf.conf` and `/etc/default/telegraf`
- `/etc/influxdb/influxdb.conf` and `/etc/default/influxdb`
- `/etc/default/chronograf`
- `/etc/kapacitor/kapacitor.conf` and `/etc/default/kapacitor`

In addition, there are a few customized setup steps that we recommend you take.

### Collect more data

By default, Telegraf only collects system metrics for the Droplet. To gather additional metrics, Telegraf can be installed on other remote systems and the `url` of the InfluxDB output plugin can be set to your Droplet's IP address.

See the Telegraf [getting started guide](https://docs.influxdata.com/telegraf/v1.9/introduction/getting-started/) for more details on how to set up Telegraf on other systems.

In some cases, the Telegraf running on your droplet can also be used to scrape metrics from other systems, such as the [Prometheus input](https://docs.influxdata.com/telegraf/v1.9/plugins/inputs/#prometheus-format), or accept request with metrics from other services, such as the [HTTP listener input](https://docs.influxdata.com/telegraf/v1.9/plugins/inputs/#http-listener-v2). Telegraf plugins can be enabled via the Telegraf configuration file at `/etc/telegraf/telegraf.conf`.

### Use the InfluxDB API

The InfluxDB is available on the Droplet's IP address at port `8086`. This port is exposed, so you may immediately connect with the [InfluxDB CLI](https://docs.influxdata.com/influxdb/v1.7/tools/shell/).

```sh
# replace $influxdb_username and $influxdb_password with valide InfluxDB user credentials
influx -username $influxdb_username -password $influxdb_password
```

### Use the Kapacitor API

Kapacitor is running on the instance and connected to InfluxDB and Chronograf. This setup is usually sufficient. However, access to the Kapacitor API can enable more advanced use cases, like using the [Kapacitor CLI](https://docs.influxdata.com/kapacitor/v1.5/working/cli_client/) to create TICK scripts.

The Kapacitor is available at port `9092`, but UFW blocks does not allow external traffic to port `9092` by default. To open the Kapacitor API port, run the following command.

```sh
ufw allow 9092/tcp
```

You can now interact with Kapacitor using the [Kapacitor CLI](https://docs.influxdata.com/kapacitor/v1.5/working/cli_client/).

## API Creation

In addition to creating a Droplet from the InfluxDB (TICK stack) One-Click application using the control panel, you can also use the DigitalOcean API.


You can list all One-Click application images using the API. As an example, to create a 4GB InfluxDB (TICK stack) Droplet in the SFO2 region, you can use the following curl command. You’ll need to either save your API access token to an environment variable or substitute it into the command below.

```sh
curl -X POST -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$TOKEN'' -d \
    '{"name":"choose_a_name","region":"sfo2","size":"s-2vcpu-4gb","image":"influxdb-18-04"}' \
    "https://api.digitalocean.com/v2/droplets"
```

## Next Steps

To run InfluxDB in production, there are several additional steps you should take, including:

- Follow the security recommendations for each TICK component:
  - [InfluxDB security](https://docs.influxdata.com/influxdb/v1.7/administration/security/)
  - [Chronograf security](https://docs.influxdata.com/chronograf/v1.7/administration/managing-security/)
  - [Kapacitor security](https://docs.influxdata.com/kapacitor/v1.5/administration/security/)
