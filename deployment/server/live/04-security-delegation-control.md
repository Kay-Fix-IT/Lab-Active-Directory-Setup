# Delegation Control

## Create New Group
* **Objective:** Organizising administrative groups and assigning users to specific groups
  * **Commands:**
    ```Powershell
    New-ADGroup -Name "<Group_Name>" -GroupScope <Scope> -GroupCategroy <Category> -Path "OU=Department_Name, OU=OU_Name, DC=Domain_Name, DC=TLD"
    ```
    * **Lab Context:**
    ```Powershell
    New-ADGroup -Name "HelpDesk_Admins" -GroupScope Global -GroupCategory Security -Path "OU=IT-Staff,OU=Klab-Enterprise,DC=klab,DC=local"

## Verify Creation
* **Objective:** Verify the creation of the new Group
  * **Commands:**
    ```powershell
    Get-ADGroup -Identity "<Group_Name>"
    ```
    * **Lab Context:**
    ```powershell
    Get-ADGroup -Identity "HelpDesk_Admins"
    ```

## Add Member To Group
* **Objective:** Adding a user as a member of the security group
  * **Commands:**
    ```powershell
    Add-ADGroupMember -Identiy "<Group_Name> -Members "<User_Name>"
    ```
    * **Lab Context**
    ```Powershell
    Add-ADGroupMember -Identity "HelpDesk_Admins" -Members "hdesk01"
    ```
    
## Verify Group Member
* **Objective:** Verify the addition of member to group
  * **Commands:**
    ```powershell
    Get-ADGroupMember -Identity "<Group_Name>" | Select-Object Name, SamAccountName, ObjectClass
    ```
    * **Lab Context:**
    ```powershell
    Get-ADGroupMember -Identity "HelpDesk_Admins" | Select-Object Name, SamAccountName, ObjectClass
    ```
## Delegate Control
* **Objective:** Activate priviledges for security group by using Directory Sevices (DSACLS) command utility
  * **Commands:**
  ```powershell
  $OUPath = "OU=Department_Name,OU=OU_Name,DC=Domain,DC=TLD"
  dsacls $OUPath /G "<NETBIOS_Domain_Name\<Group_Name>:<Permissions>"
  ```
  * **Lab Context:**
    ```powershell
    $OUPath = "OU=Standard-Employees,OU=Klab-Enterprise,DC=klab,DC=local"
    dsacls $OUPath /G "KLAB\HelpDesk_Admins:RPWP"
    ```