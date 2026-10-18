# Proxmox Backup Server (PBS) Infrastructure Documentation

## Overview

This document outlines the disk configuration, datastore structure, retention policies, and maintenance jobs implemented on the Proxmox Backup Server node running **PBS 4.2.6**.

> **Note:** This specific configuration was built for experimentation, testing, and learning purposes. As noted during testing, saving low numbers of frequent snapshots on small virtual disks represents a non-optimal production architecture.

---

## Disk & Storage Layout

The underlying system storage consists of local virtual disks formatted with LVM and ext4 filesystems:

* **System Storage (`/dev/sda`):**
  * **Capacity:** 34.36 GB (32 GB virtual disk)
  * **Partitions:** `sda1` (1.03 MB BIOS boot), `sda2` (536.87 MB EFI), `sda3` (33.82 GB LVM)
  * **LVM Logical Volumes:** `pbs-root` mounted at `/` (27.6 GB) and `pbs-swap` (3.9 GB)
* **Datastore Storage (`/dev/sdb`):**
  * **Capacity:** 10.74 GB (10 GB virtual disk)
  * **Partition:** `/dev/sdb1` formatted as **ext4**
  * **Mount Point:** Mounted at `/mnt/datastore/backups1`

---

## Datastore & Namespace Hierarchy

The datastore is initialized as `backups1` and utilizes dedicated namespaces to segregate backup workloads and prune schedules:

* **Datastore Name:** `backups1`
* **Total Size:** 9.91 GB total space (3.27 GB used / 6.64 GB available)
* **Root Namespace (`/`):**
  * Contains virtual machine backups, such as `vm/101` (`ubuntu-1`).
* **Sub-Namespace (`vms-daily`):**
  * Dedicated namespace housing daily scheduled backups.

---

## Retention Policies & Prune Jobs

Prune jobs run on scheduled intervals to enforce backup retention limits across namespaces:

* **Root Namespace (`- (Root)`):**
  * **Job ID:** `s-95f9c7cd-1dbe`
  * **Schedule:** `hourly`
  * **Retention:** Keeps the last **3 hourly backups** (targeting VMs backed up every 30 minutes).
* **Daily Namespace (`vms-daily`):**
  * **Job ID:** `s-d9e00df0-93a4`
  * **Schedule:** `*:0/30` (Every 30 minutes)
  * **Retention:** Keeps the last **2 backups** (targeting daily jobs).

---

## Maintenance & Garbage Collection

To clean up unreferenced data chunks after pruning, Garbage Collection is configured at the datastore level:

* **Garbage Collection Schedule:** Executed daily at `0/2:00` (every 2 hours starting at midnight).
* **Data Status:** Automatically purges unreferenced backup data removed by prune jobs to maintain datastore capacity.