# sysadmin-project
Nama  :   Hamid Azhar Abdillah
NIM   :   22.83.0925
Kelas :   TK03 - SysAdmin

# Final Project SysAdmin membuat sebuah web server dengan instalasi beberapa layanan server seperti berikut :

1. Apache2
2. Grafana
3. Prometheus & Node-Exporter
4. Mail Server


# Langkah-langkah

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
    
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee                 /etc/apt/keyrings/grafana.gpg > /dev/null

    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com             stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com              beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

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

  
