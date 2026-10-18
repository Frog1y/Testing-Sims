# Proxmox VE Documentation

## Overview
This document outlines the configuration, storage layout, backup policies, user access controls, and firewall rules implemented on the Proxmox Virtual Environment (PVE) cluster node (`proxmox`) running Proxmox VE 9.2.20.\
The configuration and infrastrucutre outlined below was done for educational and testing purposes and is far from effective.

---

## Storage Architecture
The environment utilizes a combination of local storage pools and three external storage backends:

* **Local Storage:**
  * `local` (Directory): Located at `/var/lib/vz`, used for backups, ISO images, container templates, and imports. Default
  * `local-lvm` (LVM-Thin): Utilized for virtual machine disk images and container rootfs storage. Default
* **External Storage:**
  * `pi-storage` (SMB/CIFS): A network share mounted from a Raspberry Pi located at `/mnt/pve/pi-storage`, configured for backups, disk images, ISO imports, and snippets.
  * `pbs-vms-daily` (Proxmox Backup Server): External backup target configured for automated daily virtual machine snapshots.
  * `proxmox-backup-server` (Proxmox Backup Server): Additional backup storage target backed by a dedicated drive on the Proxmox backup server.

---

## Backup Schedules & Retention
Automated backup jobs are configured to protect virtual machines and containers across the environment:

* **Frequent VM/Container Backups (`*/30` schedule):** Automated backup jobs targeting ID `101` (VM) and ID `102` (Container) running at frequent intervals to `proxmox-backup-server` and `pbs-vms-daily`.
* **Daily Scheduled Backup:** A dedicated backup task runs every day at 02:00 (`02:00` schedule) targeting node resources.
* **Retention Policy:** Managed via standard storage configuration fallbacks.

---

## User Management & Access Control
User accounts and permissions are managed at the datacenter level:

* **Users Configured:**
  * `root` (Realm: `pam`, Enabled: Yes, Groups: None)
  * `andraz` (Realm: `pam`, Name: Andraz Hren, Enabled: Yes, Groups: `admins`)
* **Note on PAM Users:** Users created under the PAM realm also maintain direct system access to the underlying Debian OS.

---

## Firewall Configuration
Network security rules are applied at both the datacenter/node level and individual virtual machine instances:

* **Datacenter / Node Firewall Rules:**
  * **Rule 0:** Accepts incoming **SSH** traffic across interface `vmbr0`.
  * **Rule 1:** Accepts incoming **TCP** traffic on port **8006** (Proxmox Web Console) across interface `vmbr0`.
* **Virtual Machine Firewall Rules (e.g., VM 101 - `ubuntu-1`):**
  * **Rule 0:** Accepts incoming **TCP** traffic on port **443** (HTTPS / Web Server) restricted explicitly to the local subnet source `192.168.64.0/24`.