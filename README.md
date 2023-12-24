# sysadmin-project
Nama  :   Hamid Azhar Abdillah
NIM   :   22.83.0925
Kelas :   TK03 - SysAdmin

# Final Project SysAdmin membuat sebuah web server dengan instalasi beberapa layanan server seperti berikut :

1. Apache2
2. Grafana
3. Prometheus & Node-Exporter


# Langkah-langkah Instalasi Layanan Server

# =====> Instalasi Apache2 <=====

Install Apache
  
    sudo apt update
    
    sudo apt install apache2
  
Atur Firewall

    sudo ufw app list
    
    sudo ufw allow 'Apache'
    
    sudo ufw status

Periksa Pengaturan Web Server

    sudo systemctl status apache2
    
    hostname -I
    
    curl -4 icanhazip.com

Akses Apache dengan address IP server

    http://ip_server_anda


# =====> Instalasi Grafana <=====

Install the prerequisite packages:

    sudo apt-get install -y apt-transport-https software-properties-common wget

Import the GPG key:

    sudo mkdir -p /etc/apt/keyrings/
    
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

To add a repository for stable releases, run the following command:

    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list  

Run the following command to update the list of available packages:

    sudo apt-get update

To install Grafana OSS, run the following command:

    sudo apt-get install grafana 

To start the service, run the following commands:

    sudo systemctl daemon-reload
    
    sudo systemctl start grafana-server
    
    sudo systemctl status grafana-server

To verify that the service is running, run the following command:

    sudo systemctl status grafana-server

To configure the Grafana server to start at boot, run the following command:

    sudo systemctl enable -- now grafana-server.service

    sudo systemctl status grafana-server

    sudo lsof -n -P -i | grep grafana

akses port localhot:3000


# =====> Instalasi Prometheus & Node-Exporter <=====
 
     wget https://github.com/prometheus/prometheus/releases/download/v2.49.0-rc.1/prometheus-2.49.0-rc.1.linux-amd64.tar.gz

extract file

    tar xvf prometheus-2.49.0-rc.1.linux-amd64.tar.gz

masuk ke direktori prometheus

    cd prometheus-2.49.0-rc.1.linux-amd64/

menjalankan prometheus dengan systemd

membuat user

    sudo groupadd --system prometheus
	  
    sudo useradd --system -s /sbin/nologin -g prometheus prometheus

    ls -l
    
pindahkan file

    sudo mv prometheus promtool /usr/local/bin/

    which prometheus

    which promtool
    
membuat direktori

    ls -l 
    
    sudo mkdir /etc/prometheus
    
    sudo mkdir /var/lib/prometheus

    ls -l /var/lib/

merubah owner prometheus

    sudo chown -R prometheus:prometheus /var/lib/prometheus/

memindahkan file

    ls -l
    
    sudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheus/

    
    cd /etc/promethues/

masuk ke pengaturan prometheus hapus beberapa konfigurasi untuk disederhanakan.

  	sudo nano prometheus.yml


membuatkan service daemon

  	sudo nano /etc/systemd/system/prometheus.service

copy konfigurasi
...

[Unit]
Description=Prometheus
Documentation=htt[s://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
WxwcStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target

...

    sudo systectl daemon-reload

  	sudo systemctl status prometheus

  	sudo systemctl enable --now prometheus

  	sudo systemctl status prometheus.service

  	sudo lsof -n -i | grep prometheus

akses prometheus lewat web

	  localhost:9090


# =====> Instalasi Node Exporter <=====

    wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz

extract file

    tar xvf node_exporter-1.7.0.linux-amd64.tar.gz

masuk ke node-exporter

    cd node_exporter-1.7.0.linux-amd64

    ls -l

    sudo mv node_exporter /usr/local/bin/

    which node_exporter

membuat service daemon

	sudo nano /etc/systemd/system/node-exporter/service

...

[Unit]
Description=Prometheus exporter for machine metrics

[Service]
Restart=always
User=prometheus
ExecStart=/usr/local/bin/node_exporter
ExecReload=/bin/kill -HUP $MAINPID
TimeoutStopSec=20s
SendSIGKILL=no

[Install]
WantedBy=multi-user.target

...

    sudo systemctl daemon-reload
    
    sudo systemctl status node-exporter.service
    
    sudo systemtl enable --now node-exporter.service

    sudo systemctl status node-exporter.service
    
    sudo lsof -n -i | grep node


akses metrics

  	localhost:9100

sesuaikan konfigurasi

    sudo nano /etc/prometheus/prometheus.yml

tambahkan job node-exporter dan port 9100.


    sudo systemctl restart prometheus

	  sudo systemctl status prometheus 
