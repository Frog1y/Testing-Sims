Basic group policy configuration.
Default domain policy that is enforced.
GPO that Enables RDP on the Domain Computers OU. This enables RDP connections, opens ports for RDP to all computers in the OU for adiminstration.
GPO that disables cmd and powershell along with registry editors in the Users OU. As well as disables the control panel and settings.
The IT department disables inheritance, so those OUs do not apply to them. But the default domain policy does as it is enforced.

I've also created a new OU for testing. This is an OU with a single machine and user - both of which are used for testing purposes before actual GPO deployment.

This OU structure allows for granual control. I can link computer GPOs to the computer OUs and user GPOs to the user OUs. As well as different sites and departments.
I can also filter further by security groups in the GPO itself.

<img width="1680" height="1050" alt="ADDC-2026-10-16-16-44-21" src="https://github.com/user-attachments/assets/10fd654e-1eba-41d7-9181-854782e02498" />


