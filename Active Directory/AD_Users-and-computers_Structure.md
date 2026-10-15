# Users and Computers

AD is set up as 1 top level domain to 5 primary top level OUs: Domain Controllers, Domain Users, Domain Computers, Domain Security Groups and Admins.\
The OUs are then further segmented by Site and Department.

This setup allows me to create users under certain Sites and Departments, as well as delegate OU control over specific Sites and Departments.\
For example, I can delegate control over the Computers - LJ OU, only to the LJ_IT_SG security group. So the Ljubljana IT department can manage that site's computers and or users.
This also allows me to apply more granual GPOs based on these OUs. 

<img width="1680" height="1050" alt="ADDC-2026-10-15-12-52-10" src="https://github.com/user-attachments/assets/16b2d030-bb2e-47f2-bc1a-93c2ffd91113" />
<img width="1680" height="1050" alt="ADDC-2026-10-15-12-57-35" src="https://github.com/user-attachments/assets/6e906c0e-5d55-4aae-82e7-aff967831b67" />


