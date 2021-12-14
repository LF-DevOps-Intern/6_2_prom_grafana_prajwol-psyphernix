1. **Installing Prometheus Server:**
			
		$ sudo groupadd --system prometheus
 		$ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
 		$ sudo mkdir /var/lib/prometheus
 		$ for i in rules rules.d files_sd; do sudo mkdir -p /etc/prometheus/${i}; done
 		$ sudo apt update
 		$ sudo apt -y install wget curl vim
 		$ mkdir -p /tmp/prometheus && cd /tmp/prometheus
 		$ curl -s https://api.github.com/repos/prometheus/prometheus/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
 		$ tar xvf prometheus*.tar.gz
 		$ cd prometheus*/
 		$ sudo mv prometheus promtool /usr/local/bin/
 		$ prometheus --version
		$ promtool --version
 		
	![image](https://user-images.githubusercontent.com/34814966/145925107-8aaced02-226a-460e-bb3b-d5a433838d73.png)

		$ sudo mv prometheus.yml /etc/prometheus/prometheus.yml
 		$ sudo mv consoles/ console_libraries/ /etc/prometheus/
 		$ cd $HOME
 		$ sudo vim /etc/prometheus/prometheus.yml
		
	![image](https://user-images.githubusercontent.com/34814966/145925219-c51a81e0-d96f-412f-816e-74b8770a1bf8.png)

 		$ sudo tee /etc/systemd/system/prometheus.service<<EOF
		[Unit]
		Description=Prometheus
		Documentation=https://prometheus.io/docs/introduction/overview/
		Wants=network-online.target
		After=network-online.target
		[Service]
		Type=simple
		User=prometheus
		Group=prometheus
		ExecReload=/bin/kill -HUP \$MAINPID
		ExecStart=/usr/local/bin/prometheus   --config.file=/etc/prometheus/prometheus.yml   --storage.tsdb.path=/var/lib/prometheus   --web.console.templates=/etc/prometheus/consoles   --web.console.libraries=/etc/prometheus/console_libraries   --web.listen-address=0.0.0.0:9090   --web.external-url=
		SyslogIdentifier=prometheus
		Restart=always
		[Install]
		WantedBy=multi-user.target
		EOF

		 $ sudo vim /etc/systemd/system/prometheus.service 
		 $ for i in rules rules.d files_sd; do sudo chown -R prometheus:prometheus /etc/prometheus/${i}; done
		 $ for i in rules rules.d files_sd; do sudo chmod -R 775 /etc/prometheus/${i}; done
		 $ sudo chown -R prometheus:prometheus /var/lib/prometheus/
		 $ sudo systemctl daemon-reload
		 $ sudo systemctl start prometheus
		 $ sudo systemctl enable prometheus
		 $ sudo ufw allow 9090/tcp
		 $ systemctl status prometheus

					
- Configuration basic authentication username/password
- Screenhot of login prompt while trying to access prometheus
- Screenshot of prometheus dashboard

2. Install node exporter on another machine than the server
- Add that machine target to server configuration
- Share screenshot from status->targets to show the available nodes
- Share configuration of node exporter & prometheus server

3. Install grafana server on same server as prometheus 
- Add prometheus data source to grafana, should be connected through basic auth
- Screenshot of working data source config
- Import & apply dashboard for node_exporter
- Screenshot of dashboard of nodes with live metrics shown.

Sample Server Config:
global:
  scrape_interval: 10s
  scrape_timeout: 6s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.10.4.105:9090']

  - job_name: 'node'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.10.5.218:9100','10.10.4.105:9100']

  - job_name: 'docker'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.10.5.218:9323']

  - job_name: 'mysql'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.10.5.218:9104']

rule_files:
  - "prometheus-rules.yml"

alerting:
  alertmanagers:
  - static_configs:
    - targets: ['localhost:9093']


