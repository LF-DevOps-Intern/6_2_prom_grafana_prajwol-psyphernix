1. **Installing Prometheus Server:**
			
			$ sudo apt update
			$ sudo apt install nginx
			$ sudo systemctl start nginx
			$ sudo useradd --no-create-home --shell /bin/false prometheus
			$ sudo useradd --no-create-home --shell /bin/false node_exporter
			$ sudo mkdir /etc/prometheus
			$ sudo mkdir /var/lib/prometheus
			$ wget https://github.com/prometheus/prometheus/releases/download/v2.31.1/prometheus-2.31.1.linux-amd64.tar.gz
			$ tar xvf prometheus-2.31.1.linux-amd64.tar.gz
			$ sudo cp prometheus-2.31.1.linux-amd64/prometheus /usr/local/bin/
			$ sudo cp prometheus-2.31.1.linux-amd64/promtool /usr/local/bin/
			$ sudo chown prometheus:prometheus /usr/local/bin/prometheus
			$ sudo cp prometheus-2.31.1.linux-amd64/promtool /usr/local/bin/
			$ sudo chown prometheus:prometheus /usr/local/bin/prometheus
			$ sudo chown prometheus:prometheus /usr/local/bin/promtool
			$ sudo cp -r prometheus-2.31.1.linux-amd64/consoles /etc/prometheus/
			$ sudo cp -r prometheus-2.31.1.linux-amd64/console_libraries /etc/prometheus/
			$ sudo chown prometheus:prometheus /etc/prometheus/consoles
			$ sudo chown prometheus:prometheus /etc/prometheus/console_libraries
			$ sudo vim get-pass.py
	
	![image](https://user-images.githubusercontent.com/34814966/145767552-2d7dedc8-fb89-41c6-85f1-b230f0f53717.png)
			
			$ python3 get-pass.py
			$ sudo apt install python3-bcrypt
			$ sudo vim /etc/prometheus/web.yaml
			
	![image](https://user-images.githubusercontent.com/34814966/145771787-1aab51c7-46fa-473c-ab74-7fbd5cca6a53.png)

			$ sudo nano /etc/prometheus/prometheus.yml
			
	![image](https://user-images.githubusercontent.com/34814966/145751995-46632db2-1422-4a24-b46b-43a2a301da7a.png)
	
			sudo vim /etc/systemd/system/prometheus.service
			
	![image](https://user-images.githubusercontent.com/34814966/145772190-8da79c68-27c2-41ff-87df-41612185429d.png)

			$ sudo systemctl daemon-reload
			$ sudo systemctl start prometheus
			$ sudo systemctl enable prometheus
			$ sudo systemctl status prometheus
					
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


