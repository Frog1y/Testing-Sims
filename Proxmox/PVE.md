# Proxmox VE Documentation

## Overview
This document outlines the configuration, storage layout, backup policies, user access controls, and firewall rules implemented on the Proxmox Virtual Environment (PVE) cluster node (`proxmox`) running Proxmox VE 9.2.20.\
The configuration and infrastrucutre outlined below was done for educational and testing purposes and is far from effective.

---

## Storage Architecture
The environment utilizes a combination of local storage pools and three external storage backends:

<img width="2109" height="325" alt="pve1" src="https://github.com/user-attachments/assets/1ede1b54-0571-4f5b-8e01-7ab3d81451d6" />

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

<img width="1863" height="315" alt="pv2" src="https://github.com/user-attachments/assets/1bbd46b5-5fb4-4202-b327-30f2e7f321bc" />

* **Frequent VM/Container Backups (`*/30` schedule):** Automated backup jobs targeting ID `101` (VM) and ID `102` (Container) running at frequent intervals to `proxmox-backup-server` and `pbs-vms-daily`.
* **Daily Scheduled Backup:** A dedicated backup task runs every day at 02:00 (`02:00` schedule) targeting node resources.
* **Retention Policy:** Managed via standard storage configuration fallbacks.

---

## User Management & Access Control
User accounts and permissions are managed at the datacenter level:

<img width="1862" height="400" alt="pve3" src="https://github.com/user-attachments/assets/680fe873-1952-466d-a003-492a0442f965" />

* **Users Configured:**
  * `root` (Realm: `pam`, Enabled: Yes, Groups: None)
  * `andraz` (Realm: `pam`, Name: Andraz Hren, Enabled: Yes, Groups: `admins`)
* **Note on PAM Users:** Users created under the PAM realm also maintain direct system access to the underlying Debian OS.

---

## Firewall Configuration
Network security rules are applied at both the datacenter/node level and individual virtual machine instances:

<img width="1972" height="488" alt="pve5" src="https://github.com/user-attachments/assets/365478f3-6e0f-41f3-be16-da7977a59868" />

* **Datacenter / Node Firewall Rules:**
  * **Rule 0:** Accepts incoming **SSH** traffic across interface `vmbr0`.
  * **Rule 1:** Accepts incoming **TCP** traffic on port **8006** (Proxmox Web Console) across interface `vmbr0`.
* **Virtual Machine Firewall Rules (e.g., VM 101 - `ubuntu-1`):**
  * **Rule 0:** Accepts incoming **TCP** traffic on port **443** (HTTPS / Web Server) restricted explicitly to the local subnet source `192.168.64.0/24`.
