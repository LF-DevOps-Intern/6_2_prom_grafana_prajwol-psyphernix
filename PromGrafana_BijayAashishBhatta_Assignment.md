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


2. **Installing node exporter on Kali Linux on Raspberry Pi 4 (ARM):**

		$ wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-armv7.tar.gz
   		$ tar xvfz node_exporter-1.3.1.linux-armv7.tar.gz
	   	$ sudo mv node_exporter-1.3.1.linux-armv7.tar.gz/node_exporter /usr/bin/
		$ sudo vim /etc/systemd/system/node_exporter.service
	
	![image](https://user-images.githubusercontent.com/34814966/145945255-0333b40f-6004-465f-820c-59121155185b.png)
	
	![image](https://user-images.githubusercontent.com/34814966/145945540-e2bc6ed7-6b5f-4895-b32b-e77f14fad377.png)

	![image](https://user-images.githubusercontent.com/34814966/145945652-9210cc25-7f0b-463b-8a0e-81b71a8fab62.png)

		$ sudo vim /etc/prometheus/prometheus.yml
	
	![image](https://user-images.githubusercontent.com/34814966/145946788-3d8c62e1-3d9e-46ec-ab7f-0484e3a3a324.png)

	![image](https://user-images.githubusercontent.com/34814966/145947928-ed4134f7-3b54-4929-b1c1-0fa389b784ec.png)

3. **Installing grafana server on same server as prometheus:**

	 	$ sudo apt-get install -y apt-transport-https
		$ sudo apt-get install -y software-properties-common wget
		$ wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
		$ echo "deb https://packages.grafana.com/enterprise/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
		$ sudo apt-get update
		$ sudo apt-get install grafana
		$ sudo systemctl daemon-reload
		$ sudo systemctl start grafana-server
		$ sudo systemctl enable grafana-server.service
		$ sudo systemctl status grafana-server
		
	![image](https://user-images.githubusercontent.com/34814966/145954172-a00451da-eb70-40ba-85f2-eaeb04a9ec1e.png)

	![image](https://user-images.githubusercontent.com/34814966/145954525-111c7a7f-27fd-4692-b39f-3c2daedc83f7.png)
	
	![image](https://user-images.githubusercontent.com/34814966/145954980-d99cb587-e49c-48fb-aab1-d4fdc8a5fb67.png)

	![image](https://user-images.githubusercontent.com/34814966/145955123-f55ca259-836d-4041-b2ef-0e9d9e702ad2.png)

	![image](https://user-images.githubusercontent.com/34814966/145955694-10e29701-9b3b-4651-9a3a-16dc1dcba734.png)

	![image](https://user-images.githubusercontent.com/34814966/145955806-ad0b6a4c-eadf-4d20-90cf-9d07ce6bb178.png)

	![image](https://user-images.githubusercontent.com/34814966/145960607-e3cbb92f-2c11-46e7-91aa-27d294e1dff5.png)

	![image](https://user-images.githubusercontent.com/34814966/145960711-173ad34a-4bfc-47e7-9ef3-798aa7eaa9e0.png)

	![image](https://user-images.githubusercontent.com/34814966/145960768-bc60a1bc-2c23-4d37-b74c-26f65470a5f9.png)

	![image](https://user-images.githubusercontent.com/34814966/145961279-4f18ec5b-a909-4a38-ab46-73ee79bb6d87.png)

	![image](https://user-images.githubusercontent.com/34814966/145961766-ac15017f-3707-4c1d-877d-de481849ee0f.png)

	![image](https://user-images.githubusercontent.com/34814966/145962400-24757818-ec21-4ac1-8266-eb94bf365ba5.png)
