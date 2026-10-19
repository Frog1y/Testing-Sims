# Active Directory - Windows Server 2025

<img width="2500" height="415" alt="image" src="https://github.com/user-attachments/assets/e56634d1-12c5-47f0-bc49-5db9a78a4e7b" />

Active Directory (AD) is a directory service developed by Microsoft that organizes, manages, and secures identities, computers, and resources across a Windows network.\
Information below outlines the infrastructure that was made for simulating an AD enviornment.

## DC1 (Server, Domain Controller)
Hypervisor: VMWare WorkStation\
VM Name: ADDC\
Domain Controller - Windows Server 2025\
IP : 192.168.64.201\
Name: ADDC\
Domain: home.com\
NETBIOS name: HOME

## PC1 (Client)
Hypervisor: VMWare WorkStation\
VM Name: AD Desktop LJ\
IP: DHCP
Windows 10\
Name: PC1\
Domain: home.com

