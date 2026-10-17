# Active Directory Users and Computers (ADUC)

<img width="1680" height="1050" alt="ADDC-2026-10-15-13-12-36" src="https://github.com/user-attachments/assets/ad781e60-197e-4e53-a7f7-fe7cd822fe3c" />

## Overview
This document outlines the organization, user account administration, computer management, and Security Group structure within the `home.com` domain using Active Directory Users and Computers (ADUC).

---

## Domain Structure & Container Layout (`home.com`)

To ensure efficient administrative delegation, clear object management, and clean Group Policy targeting, objects are organized into dedicated Organizational Units (OUs) rather than default containers like `Computers` or `Users`.

### User Administration (`Domain Users`)
User accounts are grouped by geographical site and further divided by business function/department to allow role-based access control and granular policy mapping:

**`Users - LJ` (Ljubljana):**
  * `Users_LJ_Finance`: Contains financial and accounting user accounts.
  * `Users_LJ_HR`: Contains human resources user accounts.
  * `Users_LJ_IT`: Contains IT support and system administrator accounts.
**`Users - MEL` (Melbourne):** Regional user accounts for Melbourne staff.
**`Users - NY` (New York):** Regional user accounts for New York staff.

### Computer Management (`Domain Computers`)
Workstations and servers are joined to the domain and sorted into location-specific OUs rather than remaining in the default `CN=Computers` container:

**`Computers - LJ`**: Workstations and local devices residing in the Ljubljana office.
  * `Computers_LJ_Finance`: Contains financial and accounting user accounts.
  * `Computers_LJ_HR`: Contains human resources user accounts.
  * `Computers_LJ_IT`: Contains IT support and system administrator accounts.
**`Computers - MEL`**: Workstations and local devices residing in the Melbourne office.
**`Computers - NY`**: Workstations and local devices residing in the New York office.

### Staging and Testing (`TESTING OU`)
Contains dedicated `Computers` and `Users` sub-containers used to stage new domain objects, test software deployments, and validate Group Policy Objects (GPOs) prior to pushing changes to production regional OUs.

---

### Group Management & Security Architecture

Active Directory Security Groups are used to grant access to network resources (file shares, permissions, remote access) following the AGDLP (Account -> Global -> Domain Local -> Permission) best practice framework:

**Domain Security Groups (`Domain Security Groups`):** Centralized security groups created to manage permissions across resources in `home.com`.
**Administrative Accounts (`Administrators`):** Dedicated privileged containers and accounts isolated from standard end-user accounts to uphold the principle of least privilege.

---

## How It Works Together
**Provisioning:** New accounts are created in their respective regional and departmental OUs (e.g., a new HR employee in Ljubljana is created inside `Domain Users -> Users - LJ -> Users_LJ_HR`).
**Access Assignment:** User accounts are added to Domain Security Groups, which are then granted permissions on network shares, printers, or security policies.
**GPO Targeting:** When a user logs in or a computer boots up, Active Directory evaluates its exact OU location in the hierarchy to apply only the policies relevant to that user's role and physical office location.




