# Users and Computers

AD is set up as 1 top level domain to 5 primary top level OUs: Domain Controllers, Domain Users, Domain Computers, Domain Security Groups and Admins.

PIC 1

The OUs are then further segmented by Site and Department;\
Example Structure;

```
Domain Users
    Users - LJ
        Users_LJ_IT
        Users_LJ_HR
        Users_LJ_Finance

Domain Computers
    Computers - MEL
        Computers_MEL_IT
        Computers_MEL_HR
        Computers_MEL_Finance
```
Pic 2

This setup allows me to create users under certain Sites and Departments, as well as delegate OU control over specific Sites and Departments.
For example, I can delegate control over the Computers - LJ OU, only to the LJ_IT_SG security group. So the Ljubljana IT department can manage that site's computers and or users.
This also allows me to apply more granual GPOs based on these OUs. 