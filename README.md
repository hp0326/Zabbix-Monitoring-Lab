# Zabbix Monitoring Lab

This repository documents the process of deploying, configuring, and performing basic infrastructure monitoring using Zabbix Server and Zabbix Agent.

## 🛠️ Technology Stack
* Red Hat Enterprise Linux
* Ubuntu Server
* MySQL
* Apache
* Zabbix Server
* Zabbix Agent

---

1. **Environment Installation and Configuration:**
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
