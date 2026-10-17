# Active Directory Group Policy Management

<img width="1680" height="1050" alt="ADDC-2026-10-16-16-44-21" src="https://github.com/user-attachments/assets/10fd654e-1eba-41d7-9181-854782e02498" />

## Overview
This document outlines the organization and structure of the Group Policy Management Console (GPMC) within the `home.com` domain. It details the hierarchical Organizational Unit (OU) design used to delegate controls, deploy software, and target specific user or computer configurations across different geographic locations (Ljubljana, Melbourne, and New York).

---

## Group Policy & OU Structure (`home.com`)

The environment utilizes a clean, granular container layout to manage policies efficiently:

### Organizational Unit Hierarchy
**`Domain Computers`**: Contains sub-OUs segregated by physical branch locations (`Computers - LJ`, `Computers - MEL`, `Computers - NY`). This layout allows precise targeting of computer-level policies (such as firewall rules, startup scripts, and local security settings) to specific offices.
**`Domain Users`**: Contains departmental user sub-OUs organized by location (e.g., `Users - LJ` split into Finance, HR, and IT; alongside `Users - MEL` and `Users - NY`). This permits user-specific configuration deployment (like drive mappings, desktop wallpapers, and permission restrictions).
**`Domain Controllers`**: Contains all of the DCs.
**`Domain Groups`**: Contains security groups.
**`TESTING OU`**: A dedicated staging container used for safely testing configurations, containing child containers like `Computers` (with a linked `test - timeout GPO`) to validate policies before production implementation.

### Group Policy Objects (GPOs)
* **`Default Domain Policy`**: The baseline policy applied across the entire domain, enforcing core security settings like account lockout thresholds and password complexity.
* **Targeted Custom GPOs**: Administrative templates created for specific tasks, such as restricting command prompts/control panels, managing user timeouts, or enabling Remote Desktop (RDP).

---

## How It Works Together
1. **Inheritance and Linkage:** Group policies are linked to domains, sites, or OUs. Policies applied at higher levels (like the domain root) cascade down, while OUs lower in the tree inherit those settings unless blocked or overridden.
2. **Security Filtering & WMI:** Policies assigned to location-based OUs evaluate against the user or computer objects residing within that exact container during logon or background refresh cycles, ensuring regional configurations match physical operational needs.




