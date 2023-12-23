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
    
    sudo mkdir -p /etc/apt/keyrings/
    
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee    /etc/apt/keyrings/grafana.gpg > /dev/null

    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com  beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

Updates the list of available packages

    sudo apt-get update

Installs the latest OSS release:

    sudo apt-get install grafana

Installs the latest Enterprise release:

    sudo apt-get install grafana-enterprise

To start the service, run the following commands:

    sudo systemctl daemon-reload
    
    sudo systemctl start grafana-server
    
    sudo systemctl status grafana-server

    sudo systemctl status grafana-server

Configure the Grafana server to start at boot using systemd

    sudo systemctl enable grafana-server.service

Alternatively, create a file in /etc/systemd/system/grafana server.service.d/override.conf

    sudo systemctl edit grafana-server.service

Add the following additional settings to grant the CAP_NET_BIND_SERVICE capability.

    [Service]
    CapabilityBoundingSet=CAP_NET_BIND_SERVICE
    AmbientCapabilities=CAP_NET_BIND_SERVICE

    PrivateUsers=false
    Restart the Grafana server using systemd

    sudo systemctl restart grafana-server

To start the Grafana server, run the following commands:

    sudo service grafana-server start
    
    sudo service grafana-server status

    sudo service grafana-server status

    sudo update-rc.d grafana-server defaults

    sudo service grafana-server restart


# =====> Instalasi Prometheus & Node-Exporter <=====
 
    wget https://github.com/prometheus/prometheus/releases/download/v2.32.1/prometheus-2.32.1.linux-amd64.tar.gz

    tar -xvf prometheus-2.32.1.linux-amd64.tar.gz

    sudo mkdir -p /data /etc/prometheus

    cd prometheus-2.32.1.linux-amd64

    sudo mv prometheus promtool /usr/local/bin/

    sudo mv prometheus.yml /etc/prometheus/prometheus.yml

    sudo chown -R prometheus:prometheus /etc/prometheus/ /data/

    sudo vim /etc/systemd/system/prometheus.service

...

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/data \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle

[Install]
WantedBy=multi-user.target

...

    sudo systemctl enable prometheus

    sudo systemctl start prometheus

    sudo systemctl status prometheus


# =====> Instalasi Node Exporter <=====

    wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz

    tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz

    sudo mv \ node_exporter-1.3.1.linux-amd64/node_exporter \ /usr/local/bin/

    rm -rf node_exporter*

    sudo vim /etc/systemd/system/node_exporter.service

...

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
    --collector.logind

[Install]
WantedBy=multi-user.target

...

     sudo systemctl enable node_exporter

     sudo systemctl start node_exporter

     sudo systemctl status node_exporter

     sudo vim /etc/prometheus/prometheus.yml

...

  - job_name: node
    static_configs:
      - targets: ['localhost:9100']

...

    promtool check config /etc/prometheus/prometheus.yml

    curl -X POST http://localhost:9090/-/reload      
