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
   rpm -Uvh [https://repo.zabbix.com/zabbix/7.2/release/alma/9/noarch/zabbix-release-latest-7.2.el9.noarch.rpm](https://repo.zabbix.com/zabbix/7.2/release/alma/9/noarch/zabbix-release-latest-7.2.el9.noarch.rpm)
   dnf clean all
   ```

