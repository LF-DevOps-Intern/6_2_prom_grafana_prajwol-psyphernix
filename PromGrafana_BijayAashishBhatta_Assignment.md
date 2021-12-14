1. **Installing Prometheus Server on Kubuntu 20.04 LTS:**
			
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
 
		$ for i in rules rules.d files_sd; do sudo chown -R prometheus:prometheus /etc/prometheus/${i}; done
		$ for i in rules rules.d files_sd; do sudo chmod -R 775 /etc/prometheus/${i}; done
		$ sudo chown -R prometheus:prometheus /var/lib/prometheus/
		$ sudo systemctl daemon-reload
		$ sudo systemctl start prometheus
		$ sudo systemctl enable prometheus
		$ sudo ufw allow 9090/tcp
		$ systemctl status prometheus
	
	![image](https://user-images.githubusercontent.com/34814966/145926048-5bb1f8af-becb-4993-828e-e714c7011784.png)
	
		$ sudo apt install python3-bcrypt
		$ sudo vim gen-pass.py
		
	![image](https://user-images.githubusercontent.com/34814966/145926260-b63cc3fe-6f63-4aaf-beaf-cd5ab93d3f24.png)

		$ python3 gen-pass.py
		$ sudo vim /etc/prometheus/web-config.yml
		
	![image](https://user-images.githubusercontent.com/34814966/145926529-0d9f5a8c-4b49-4ca1-93dc-85ea5962e6f1.png)
	
		$ promtool check web-config web-config.yml
		$ sudo vim /etc/systemd/system/prometheus.service
	
	![image](https://user-images.githubusercontent.com/34814966/145928190-1a64df75-dca5-485e-a9ec-9065e10f7a09.png)
	
	![image](https://user-images.githubusercontent.com/34814966/145942425-8772e6bc-08fc-4e4d-b0a4-91c351d6700b.png)

	![image](https://user-images.githubusercontent.com/34814966/145942513-02173229-1f53-4aea-9228-d7b8b58c0790.png)


2. **Install node exporter on Kali Linux on Raspberry Pi 4 (ARM):**

		$ wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-armv7.tar.gz
   		$ tar xvfz node_exporter-1.3.1.linux-armv7.tar.gz
	   	$ sudo mv node_exporter-1.3.1.linux-armv7.tar.gz/node_exporter /usr/bin/
		$ sudo vim /etc/systemd/system/node_exporter.service
	
	![image](https://user-images.githubusercontent.com/34814966/145945255-0333b40f-6004-465f-820c-59121155185b.png)
	
	![image](https://user-images.githubusercontent.com/34814966/145945540-e2bc6ed7-6b5f-4895-b32b-e77f14fad377.png)

	![image](https://user-images.githubusercontent.com/34814966/145945652-9210cc25-7f0b-463b-8a0e-81b71a8fab62.png)

		$ sudo vim /etc/prometheus/prometheus.yml
	
	![image](https://user-images.githubusercontent.com/34814966/145946788-3d8c62e1-3d9e-46ec-ab7f-0484e3a3a324.png)


- Add that machine target to server configuration
- Share screenshot from status->targets to show the available nodes
- Share configuration of node exporter & prometheus server

3. Install grafana server on same server as prometheus 
- Add prometheus data source to grafana, should be connected through basic auth
- Screenshot of working data source config
- Import & apply dashboard for node_exporter
- Screenshot of dashboard of nodes with live metrics shown.
