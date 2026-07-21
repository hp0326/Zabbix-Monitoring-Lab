# Zabbix Monitoring Lab

This repository documents the process of deploying, configuring, and performing basic infrastructure monitoring using Zabbix Server and Zabbix Agent.

## Technology Stack
* Red Hat Enterprise Linux
* Ubuntu Server
* MySQL
* Apache
* Zabbix Server
* Zabbix Agent

## Zabbix server Installation and Configuration on Red Hat Enterprise Linux:
Update packages:
```bash
dnf update && dnf upgrade -y
```

Install the Zabbix repository:
```bash
rpm -Uvh https://repo.zabbix.com/zabbix/7.2/release/alma/9/noarch/zabbix-release-latest-7.2.el9.noarch.rpm
dnf clean all
```

Install the Zabbix server:
```bash
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent 
```

Install and start MySQL:
```bash
sudo dnf install mysql-server
sudo systemctl start mysqld.service
```

Enable service auto-start on boot:
```bash
sudo systemctl enable mysqld
```

Database configuration:
```bash
mysql -uroot
```

```
create database zabbix character set utf8mb4 collate utf8mb4_bin; 
create user zabbix identified by <PASSWORD>; 
grant all privileges on zabbix.* to zabbix;   
SET GLOBAL log_bin_trust_function_creators = 1; 
quit;
```

Import the database
```bash
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4-uzabbix -p zabbix
```

Edit the configuration file and set the database password:
```bash
nano /etc/zabbix/zabbix_server.conf
```
DBPassword=<PASSWORD>

Restart services and enable them on boot:
```bash
systemctl restart zabbix-server zabbix-agent httpd php-fpm 
systemctl enable zabbix-server zabbix-agent httpd php-fpm
```

Configure firewall:
```bash
firewall-cmd --add-service={http,https} --permanent 
firewall-cmd --add-port={10051/tcp,10050/tcp} --permanent 
firewall-cmd --reload 
firewall-cmd --list-all
```

Update following lines in /etc/php.ini:
   
date.timezone = Europe/Warsaw 
max_execution_time = 600 
max_input_time = 600 
memory_limit = 256M 
post_max_size = 32M 
upload_max_filesize = 16M 
session.auto_start = 0 
   
After updating, restart Apache:
```bash
systemctl restart httpd
```

SELinux configuration:
```bash
setsebool -P httpd_can_network_connect on
setsebool -P zabbix_can_network on
```

Log into the web panel:
http://<YOUR_IP_ADDRESS>/zabbix/setup.php
   
Default credentials: User Admin, Password zabbix

## Zabbix agent Installation and Configuration on Ubuntu server
```bash
sudo dnf install zabbix-agent -y
sudo systemctl enable --now zabbix-agent
sudo systemctl status zabbix-agent
```
Update following lines in /etc/zabbix/zabbix_agentd.conf:
```
Server=<IP Address of your Zabbix server vm>
Hostname=<Hostname of zabbix agent>
# ListenPort=10050 <Uncomment this line>
```

## Monitoring Configuration:
* **Hosts and Interfaces:**
      * Added remote host (Ubuntu Server) and managed network interfaces via Zabbix Frontend.
   <img width="1034" height="576" alt="image" src="https://github.com/user-attachments/assets/0ade460c-dedb-4a54-b6de-3e63b38c8459" />


   * **Items (Data Metrics):** Created custom items for key infrastructure resources:
     * **CPU usage:** Monitoring processor utilization.
     * **Disk usage:** Tracking available storage space.
     * **Network interface:** Measuring network traffic and throughput.
     * **Ping:** Checking host availability and latency.
     * **RAM usage:** Monitoring memory consumption.
     * **SSH listen:** Verifying if the SSH service port is active.
   <img width="1656" height="251" alt="image" src="https://github.com/user-attachments/assets/d2129658-5cb0-4499-9c0d-0f09f1df10c7" />


   * **Triggers (Alert Thresholds):** Configured alert conditions:
     * **High disk usage:** Alerts when storage capacity reaches critical thresholds.
     * **Server does not respond:** Ping-based trigger detecting host downtime.
   <img width="1671" height="129" alt="image" src="https://github.com/user-attachments/assets/0162eb11-952a-42e0-90aa-658d8fa1b046" />


   * **Graphs (Visualizations):** Created custom performance graphs:
     * **CPU usage graph**
     * **Disc usage graph**
     * **RAM usage graph**
  <img width="1000" height="357" alt="image" src="https://github.com/user-attachments/assets/55df623e-3c8e-4d52-88fb-daafb0da2352" />


   * **Templates:**
     * Created a unified custom template containing all the mentioned items, triggers, and graphs to easily apply them to monitored hosts.
   <img width="843" height="65" alt="image" src="https://github.com/user-attachments/assets/3add1852-8935-4f6c-ac5f-ed661d97ddf3" />

















   
   
