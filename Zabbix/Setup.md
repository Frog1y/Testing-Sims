# Zabbix Server Setup & Host Monitoring

This documentation outlines the setup process for a **Zabbix 7.4** monitoring environment on Ubuntu 26.04 and provides a step-by-step procedure for configuring and monitoring new target hosts using Zabbix Agent 2.

---

## 1. Zabbix Server Details & Environment Setup

The Zabbix Server was installed with the combined `Server-Frontend-Agent` components using MySQL as the backend database and Apache as the web server.

### Server Environment
* **OS**: Ubuntu 26.04 LTS
* **Hostname**: `zabbix`
* **IP Address**: `192.168.64.244`
* **System User**: `andraz`

### Database Configuration
* **Database Name**: `zabbix`
* **MySQL User**: `zabbix`
* **MySQL Password**: `[*]`

### Web Frontend Access
* **URL**: `http://192.168.64.244/zabbix`
* **Default Username**: `Admin`
* **Password**: `[*]`

> **Installation Reference**: [Zabbix 7.4 Installation Instructions (Ubuntu 26.04 + MySQL + Apache)](https://www.zabbix.com/download?zabbix=7.4&os_distribution=ubuntu&os_version=26.04&components=server_frontend_agent&db=mysql&ws=apache)

---

## 2. Monitoring a New Host

> **Note**: While the example below demonstrates setting up a **Linux Server host using Zabbix Agent 2**, Zabbix can monitor a wide array of resources (including network switches/routers via SNMP, virtual machines, containers, databases, and web applications).

### Workflow Overview
```
+-------------------+                      +---------------------+
|   Target Host     |                      |    Zabbix Server    |
| (Zabbix Agent 2)  |<---------------------|   192.168.64.244    |
+-------------------+                      +---------------------+
```

---

### Step-by-Step Configuration

#### Step 1: Install Zabbix Agent 2
On the target machine, download and install the Zabbix Agent package corresponding to the host operating system.

* Official Download Portal: [Zabbix Downloads](https://www.zabbix.com/download?zabbix=7.4)

#### Step 2: Configure the Zabbix Agent
Edit the configuration file on the target host:

```bash
sudo nano /etc/zabbix/zabbix_agent2.conf
```

Update the key directives to point to your Zabbix server IP and define a unique hostname:

```ini
Server=192.168.64.244
ServerActive=192.168.64.244
Hostname=<UNIQUE_HOST_NAME>
```

> ⚠️ **Important**: The `Hostname` value defined here **must exactly match** the hostname specified later in the Zabbix Web Console.

Restart and enable the agent service:
```bash
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
```

#### Step 3: Firewall Configuration
Zabbix servers and Proxies listen to incoming connections on port `10051` (When host agents are sending data). While the agents are receiving connections and request from the servers/proxies on port `10050`.\
When configuring firewall rules ~ Open connections from specific ip addresses to specific ports like the example below.\

Agent Machine:
```bash
sudo ufw allow from 192.168.64.244 to any port 10050 proto tcp
```

Server/Proxy Machine:
```bash
sudo ufw allow from 192.168.64.211 to any port 10051 proto tcp
```

#### Step 4: Register the Host in the Web Console

1. Open the web interface at `http://192.168.64.244/zabbix` and log in.
2. Navigate to **Data Collection** > **Hosts**.
3. Click **Create Host** (top right corner).
4. Fill out the core configuration parameters:
   * **Host name**: Enter the exact `<UNIQUE_HOST_NAME>` configured in `/etc/zabbix/zabbix_agent2.conf`.
   * **Templates**: Select **Linux by Zabbix agent** (or the template matching your target system/device).
   * **Host groups**: Select an appropriate group (e.g., `Linux servers`).
   * **Interfaces**: Add an **Agent** interface and enter the IP address of the target host.
5. Click **Add** to save.

---

### Verification
After saving, check the status under **Data Collection > Hosts**. The **Availability** indicator for `ZBX` should turn green within a couple of minutes, confirming active telemetry ingestion.

---

## 20. 10. 2026 Update ~ Proxy server
Installed and configured a proxy server inside the same infrastructure.\
```
+-------------------+                      +---------------------+                      +---------------------+
|   Target Host     |                      |    Zabbix Proxy     |                      |    Zabbix Server    |
| (Zabbix Agent 2)  |<---------------------|   192.168.64.212    |<---------------------|   192.168.64.244    |
+-------------------+                      +---------------------+                      +---------------------+
```
### Notes
Configuring a proxy server is similar to any other Zabbix installation. I used an Ubuntu OS with an SQLite database.

Documentation: https://www.zabbix.com/download?zabbix=7.4&os_distribution=ubuntu&os_version=26.04&components=proxy&db=sqlite3&ws=

Important Configs: ```/etc/zabbix/zabbix_proxy.conf```

Set Server IP to your Zabbix server IP. Set Hostname to exactly like you set it in the web console. ⚠️ **DBName**: This is your SQLite database. Set this to a path which the zabbix user and group can access. Either create one and configure permissions or use a path where zabbix already has permissions.

Ensure your host and proxy/server firewall rules are updated to allow for connections shown in the diagam above.

Ensure your hosts agent configuration ```/etc/zabbix/zabbix_agent2.conf``` now points to the Proxy.




