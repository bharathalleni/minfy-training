## Installing Grafana

In this first step, you will install Grafana onto your Ubuntu 18.04 server. You can install Grafana either by  [downloading it directly from its official website](https://grafana.com/grafana/download)  or by going through an  [APT repository](https://www.digitalocean.com/community/tutorials/ubuntu-and-debian-package-management-essentials#debian-package-management-tools-overview). Because an APT repository makes it easier to install and manage Grafana’s updates, you’ll use that method in this tutorial.

Although Grafana is available in  [the official Ubuntu 18.04 packages repository](https://packages.ubuntu.com/bionic/), the version of Grafana there may not be the latest, so use Grafana’s official repository.

Download the Grafana  [GPG key](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages)  with  [`wget`](https://www.gnu.org/software/wget/), then  [pipe the output](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection#pipes)  to  `apt-key`. This will add the key to your APT installation’s list of trusted keys, which will allow you to download and verify the GPG-signed Grafana package.

```
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

In this command, the option  `-q`  turns off the status update message for  `wget`, and  `-O`  outputs the file that you downloaded to the terminal. These two options ensure that only the contents of the downloaded file are pipelined to  `apt-key`.

Next, add the Grafana repository to your APT sources:

```
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

Refresh your APT cache to update your package lists:

```
sudo apt update
```
You can now proceed with the installation:

```
sudo apt install grafana
```

Once Grafana is installed, use  `systemctl`  to start the Grafana server:

```
sudo systemctl start grafana-server
```

Next, verify that Grafana is running by checking the service’s status:

```
sudo systemctl status grafana-server
```

You will receive output similar to this:

```
grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; disabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-08-13 08:22:30 UTC; 11s ago
     Docs: http://docs.grafana.org
 Main PID: 13630 (grafana-server)
    Tasks: 7 (limit: 1152)
```

This output contains information about Grafana’s process, including its status, Main Process Identifier (PID), and more.  `active (running)`  shows that the process is running correctly.

Lastly, enable the service to automatically start Grafana on boot:

```
sudo systemctl enable grafana-server
```

You can now access the default Grafana login screen by pointing your web browser to `http://your_ip:3000`. If you’re unable to reach Grafana, verify that your firewall is set to allow traffic on port `3000` and then re-trace the previous instructions.
