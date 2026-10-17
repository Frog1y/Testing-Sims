## Sites and Services

Active Directory Sites and Services maps physical network infrastructure to logical directory components to optimize authentication, domain controller selection, and replication.

<img width="1680" height="1050" alt="ADDC-2026-10-17-16-00-28" src="https://github.com/user-attachments/assets/aeb3ac0c-dece-426d-9eb1-3250246f2fc5" />

### How It Works Together
**Subnet-to-Site Association:** Client machines and domain controllers communicate over internal private IP subnets (e.g., `192.168.64.0/24` or branch subnets like `192.168.10.0/24`). 
**Client Mapping:** When a client boots up or requests a service, the domain controller inspects the source IP address of the incoming packet against the subnets defined in AD Sites and Services.
**Local Authentication:** Matching subnets allow clients to automatically discover and authenticate against the nearest local domain controller rather than traversing expensive WAN links or falling back to the `Default-First-Site-Name`.
**Impact of Subnetless Sites:** If a site is left without an assigned subnet, basic TCP/IP connectivity and DNS resolution will still function via static IPs, but Active Directory will fail to automatically map clients to that physical location, forcing them to default to the primary site.
**Setup:** 3 subnets for each site `192.168.10.0/24`, `192.168.20.0/24` and `192.168.30.0/24`. Each site has a subnet assigned to it for Ljubljana, New York and Melbourne respective of that order.
**Replication:** NY-LJ-60, and NY-LJ-60 defines replication from the NY to individual sites every 60 minutes. Creating specific point-to-point links (NY-LJ and NY-MEL) allows me to isolate paths, assign tailored replication schedules.
