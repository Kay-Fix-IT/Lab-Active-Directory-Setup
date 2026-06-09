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
  dsacls $OUPath  /I:S /G "<NETBIOS_Domain_Name\<Group_Name>:<Permissions>"
  ```
  
  * **Lab Context:**
    ```powershell
    $OUPath = "OU=Standard-Employees,OU=Klab-Enterprise,DC=klab,DC=local"
    dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:LCRP;;user"
    dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:CA;Reset Password;user"
    dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:CA;Change Password;user"
    dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:WP;pwdLastSet;user"
    dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:wp;userAccountControl;user"
    ```

# Verification:
  * Reset Password
    * **Steps:**
      * Log into workstation with with RSAT tool: Active Directory USers and Computers (ADUC)
        * **Lab Context:** `WS02`
      * Search for Active Directory Users and Computers in the search engine
      * Select and open the ADUC tool
      * Navigate to the preferred user, right click on the SaMAccountName
        * **Lab Context:** `jsmith`
      * Select **reset password**
      * Enter a new password and reenter for confirmation
      * Select **User must change password at next logon**
      * Select **Ok**
      * Log into a workstation as user
        * **Lab Context:** `WS01, jsmith`
      * Verify that password was reset and input the new password
        * *Note:* Succes
