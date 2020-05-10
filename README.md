# Minfy-Prometheus and Grafana training
This tutorial walks you through setting up Prometheus, Node exporters and Grafana dashboards.
## Target Audience
The target audience for this tutorial are someone planning to support a Prometheus/Grafana deployment and 

 - Cloud Engineers
 - Developers
 - System Admins
 - DevOps Engineers

## Prometheus Installation and Setup

Heading

Installation on Ubuntu 16.04/18.04 :

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
