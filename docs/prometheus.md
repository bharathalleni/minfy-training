## Installing Prometheus

### Installation on Ubuntu 16.04/18.04 :

Requirements :
-   A server running Ubuntu 18.04 LTS.
-   A non-root user with sudo privileges.
By default, Prometheus is not available in the Ubuntu 18.04 LTS (Bionic Beaver) default repository. So you will need to add the repository for that.

First, download and add the GPG key with the following command:

```
wget https://s3-eu-west-1.amazonaws.com/deb.robustperception.io/41EFC99D.gpg | sudo apt-key add -
```

Next, update the repository and install Prometheus with the following command:

```
sudo apt-get update -y
sudo apt-get install prometheus prometheus-node-exporter prometheus-pushgateway prometheus-alertmanager -y
```

Once the installation is completed, start Prometheus service and enable it to start on boot time with the following command:

```
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

You can also check the status of Prometheus service with the following command:

```
sudo systemctl status prometheus
```
### Installation on Docker:

 - [Installing Prometheus on Docker](https://github.com/bharathalleni/minfy-training/blob/master/docs/prometheus-on-docker.md)

## Installing Node Exporter (Optional)

> This section of the tutorial is only for older versions of Ubuntu. Please skip this step if you are on Ubuntu 16.04 or later.

To expand Prometheus beyond metrics about itself only, we’ll install an additional exporter called Node Exporter. Node Exporter provides detailed information about the system, including CPU, disk, and memory usage.

First, download the current stable version of Node Exporter into your home directory. You can find the latest binaries along with their checksums on  [Prometheus’ download page](https://prometheus.io/download/).

```
cd ~
curl -LO https://github.com/prometheus/node_exporter/releases/download/v0.15.1/node_exporter-0.15.1.linux-amd64.tar.gz
```
Now, unpack the downloaded archive.

```
tar xvf node_exporter-0.15.1.linux-amd64.tar.gz
```

This will create a directory called  `node_exporter-0.15.1.linux-amd64`  containing a binary file named  `node_exporter`, a license, and a notice.

Copy the binary to the  `/usr/local/bin`  directory and set the user and group ownership to the  **node_exporter**  user that you created in Step 1.

```
sudo cp node_exporter-0.15.1.linux-amd64/node_exporter /usr/local/bin
```

Lastly, remove the leftover files from your home directory as they are no longer needed.

```
rm -rf node_exporter-0.15.1.linux-amd64.tar.gz  node_exporter-0.15.1.linux-amd64
```
The steps for running Node Exporter are similar to those for running Prometheus itself. Start by creating the Systemd service file for Node Exporter.
```
sudo nano /etc/systemd/system/node_exporter.service
```

This service file tells your system to run Node Exporter as the  **node_exporter**  user with the default set of collectors enabled.

Copy the following content into the service file:

> Node Exporter service file - /etc/systemd/system/node_exporter.service

```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=ubuntu
Group=ubuntu
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

```
Save the file and close your text editor.

Finally, reload  `systemd`  to use the newly created service.

```
sudo systemctl daemon-reload
```

You can now run Node Exporter using the following command:

```
sudo systemctl start node_exporter
```

Verify that Node Exporter’s running correctly with the  `status`  command.

```
sudo systemctl status node_exporter
```

Like before, this output tells you Node Exporter’s status, main process identifier (PID), memory usage, and more.

If the service’s status isn’t  `active`, follow the on-screen messages and re-trace the preceding steps to resolve the problem before continuing.
Lastly, enable Node Exporter to start on boot.

```
sudo systemctl enable node_exporter
```

With Node Exporter fully configured and running as expected, we’ll tell Prometheus to start scraping the new metrics.

### Configuring Prometheus to Scrape Node Exporter
Because Prometheus only scrapes exporters which are defined in the  `scrape_configs`  portion of its configuration file, we’ll need to add an entry for Node Exporter, just like we did for Prometheus itself.

Open the configuration file.
```
sudo nano /etc/prometheus/prometheus.yml
```

At the end of the  `scrape_configs`  block, add a new entry called  `node_exporter`.

> Prometheus config file  - /etc/prometheus/prometheus.yml

```
 - job_name: 'node_exporter'
   scrape_interval: 5s
   static_configs:
     - targets: ['localhost:9100']  
```

Because this exporter is also running on the same server as Prometheus itself, we can use localhost instead of an IP address again along with Node Exporter’s default port,  `9100`.

Your whole configuration file should look like this:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']  
```
Prometheus provides a basic web interface for monitoring the status of itself and its exporters, executing queries, and generating graphs. But, due to the interface’s simplicity, the Prometheus team  [recommends](https://prometheus.io/docs/visualization/browser/)  [installing and using Grafana](https://prometheus.io/docs/visualization/grafana/)  for anything more complicated than testing and debugging.

In this tutorial, we’ll use the built-in web interface to ensure that Prometheus and Node Exporter are up and running, and we’ll also take a look at simple queries and graphs.

To test, point your web browser to  `http://your_server_ip:9090`
Once logged in, you’ll see the  **Expression Browser**, where you can execute and visualize custom queries.

![Prometheus Dashboard Welcome](http://assets.digitalocean.com/articles/install-prometheus-on-ubuntu-16-04/Prometheus-Dashboard-Welcome.png)

Before executing any expressions, verify the status of both Prometheus and Node Explorer by clicking first on the  **Status**  menu at the top of the screen and then on the  **Targets**  menu option. As we have configured Prometheus to scrape both itself and Node Exporter, you should see both targets listed in the  `UP`  state.

![Prometheus Dashboard Targets](http://assets.digitalocean.com/articles/install-prometheus-on-ubuntu-16-04/Prometheus-Dashboard-Targets.png)

Next: [Installing Grafana](https://github.com/bharathalleni/minfy-training/blob/master/docs/grafana.md)
