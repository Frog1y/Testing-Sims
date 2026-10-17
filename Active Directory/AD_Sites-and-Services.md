# Active Directory Structure and Site Architecture Reference

## Overview
This document outlines the organization of the Active Directory (AD) environment for the domain `home.com`, incorporating Organizational Unit (OU) structuring and geographic site/subnet considerations as discussed in our network design.

---

## Active Directory Structure (`home.com`)

The screenshot depicts the **Group Policy Management Console (GPMC)**, illustrating how objects and policies are hierarchically organized within the forest.

### 1. Organizational Unit (OU) Hierarchy
The domain utilizes a structured OU layout to segregate user accounts, computer objects, and testing environments:
* **`Domain Computers`**: Contains sub-OUs organized by geographic location or function (e.g., `Computers - LJ`, `Computers - MEL`, `Computers - NY`), allowing targeted computer-level policy deployment.
* **`Domain Users`**: Contains departmental user sub-OUs separated by location (e.g., `Users - LJ` with sub-divisions for Finance, HR, and IT; `Users - MEL`; `Users - NY`).
* **`TESTING OU`**: A dedicated container used for staging and testing configurations, containing child containers like `Computers` (with a linked `test - timeout GPO`) and `Users` to evaluate group policies safely before broad production rollout.

### 2. Group Policy Objects (GPOs)
Global and targeted policies are managed here, including baseline configurations like the `Default Domain Policy` alongside custom administrative templates (e.g., restricting command prompts, control panels, or enabling RDP).

---

## Site Topology and Network Integration

Active Directory Sites and Services maps physical network infrastructure to logical directory components to optimize authentication, domain controller selection, and replication.

### How It Works Together
1. **Subnet-to-Site Association:** Client machines and domain controllers communicate over internal private IP subnets (e.g., `192.168.64.0/24` or branch subnets like `192.168.10.0/24`). 
2. **Client Mapping:** When a client boots up or requests a service, the domain controller inspects the source IP address of the incoming packet against the subnets defined in AD Sites and Services.
3. **Local Authentication:** Matching subnets allow clients to automatically discover and authenticate against the nearest local domain controller rather than traversing expensive WAN links or falling back to the `Default-First-Site-Name`.
4. **Impact of Subnetless Sites:** If a site is left without an assigned subnet, basic TCP/IP connectivity and DNS resolution will still function via static IPs, but Active Directory will fail to automatically map clients to that physical location, forcing them to default to the primary site.