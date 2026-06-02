# Directory Management

## Creating Organization Unit (OU)
* **Objective:** Creating an organizational unit structure
  * **commands:**
    1. Create top level organizational structure
       ```powershell
       New-ADOrganizationalUnit -Name "<OU_Name>" -Path "DC=Domain,DC=TLD"
       ```
       * **Lab context**: Replace OU Name and path with correct names
       ```powershell
       New-ADOrganizationalUnit -Name "KLAB-Enterprise" -Path "DC=klab,DC=local"
       ```
    2. Create separate boundaries within organization parent
       ```powershell
       New-ADOrganizationalUnit -Name "<Department_Name>" -Path "OU=<OU_Name>, DC=Domain, DC=TLD"
       ```
       * **Lab Context**: Replace Departname name and path with correct names
         ```powershell
         New-ADOrganizationUnit -Name "Corporate-Employees" -Path "Ou=KLAB-Enterprise,DC=Klab,DC=Local"
         New-ADOrganizationUnit -Name "Standard-Employees" -Path "Ou=KLAB-Enterprise,DC=Klab,DC=Local"
         New-ADOrganizationUnit -Name "IT-Staff" -Path "Ou=KLAB-Enterprise,DC=Klab,DC=Local"            
         ```
## Create a new user
* **Objective:** Creating a new user, Jsmith, for the standard employees department
* **Commands:**
  ```powershell
  $Password = ConvertTo-SecureString "<Password>" AsPlainText !Force
     New-ADUser -Name "<Full_Name>" `
           -GivenName "<First_Name>" `
           -Surname "<Last_Name>" `
           -SamAccountName "<Logon_Name>" `
           -UserPrincipalName "<Logon_Name@Domain>" `
           -Path "OU=Department,OU=Top Organizational unit,DC=Domain,DC=TLD" `
           -AccountPassword $Password `
           -Enabled $true `
           -ChangePasswordAtLogon $true
  ```
  * **Lab Context:**
    ```powershell
    $Password = ConvertTo-SecureString "SecureStart2026!" -AsPlainText -Force
    New-ADUser -Name "John Smith" `
         -GivenName "John" `
         -Surname "Smith" `
         -SamAccountName "jsmith" `
         -UserPrincipalName "jsmith@klab.local" `
         -Path "OU=Standard-Employees,OU=KLAB-Enterprise,DC=klab,DC=local" `
         -AccountPassword $Password `
         -Enabled $true `
         -ChangePasswordAtLogon $true
    ```
---
### Troubleshooting
* [OU Creation Errors](../../troubleshooting/ad-management/ou-creation-errors.md)