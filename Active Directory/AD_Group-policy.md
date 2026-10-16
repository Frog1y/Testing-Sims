Basic group policy configuration.

Default domain policy that is enforced.\
GPO that Enables RDP on the Domain Computers OU. This enables RDP connections, opens ports for RDP to all computers in the OU for adiministration.\
GPO that disables cmd and powershell along with registry editors in the Users OU. As well as disables the control panel and settings.\
The IT department disables inheritance, so that they can still use cmd, powershell, setting and registry editors. But the default domain policy does as it is enforced.\
I've also created a new OU for testing. This is an OU with a single machine and user - both of which are used for testing purposes before actual GPO deployment.\
(note, this is a very broad GPO setup - in practice, this would be filtered down further in Security Filtering, so that the policies would only apply to certain users/groups)

<img width="1680" height="1050" alt="ADDC-2026-10-16-16-44-21" src="https://github.com/user-attachments/assets/10fd654e-1eba-41d7-9181-854782e02498" />


