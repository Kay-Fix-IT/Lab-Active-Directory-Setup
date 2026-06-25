# GPO Management

## Create Group Policies
* **Objective:** Create a group policy using powershell
  * **Command:** 
    ```powershell
    New-GPO -Name "<GPO_Name>" -Comment "<Short_Summary>"
    ```
    * **Lab Context:** 
    ```powershell
    New-GPO -Name "Map_Resources_drive" -Comment "Policy to mount Resources Share to R: drive letter"
    ```
* **Objective:** Verify new Group Policy Object(GPO)
  * **Command:**
    ```powershell
    Get-GPO -Name "<GPO_Name>"
    ```
    * **Lab Context:**
      ```powershell
      Get-GPO -Name "Map_Resources_Drive"
      ``` 

## Configure Security Filtering
* **Objective:** Assign a group, `GPO_Admins`, priviledges to read and apply group policy
* **Command:**
  ```powershell
  # GpoApply grants the permission for the group to read and accept group policy
  Set-GPPermission -Name "<GPO_Name>" -TargetName "Group_Name" -TargetType <Group_Type> -PermissionLevel <Permission_Level>

  # Restricts Autheticated Users to simply read to prevent other users in the domain from accepting the policy
  Set-GPPermission -Name "Map_Resources_Drive" -TargetName "Authenticated Users" -TargetType Group -PermissionLevel GpoRead
  ```
  * **Lab Context:**
    ```powershell
    # GpoApply grants the permission for the group to read and accept group policy
    Set-GPPermission -Name "Map_Resources_Drive" -TargetName "HR" -TargetType Group -PermissionLevel GpoApply

    # Restricts Autheticated Users to simply read to prevent other users in the domain from accepting the policy
    Set-GPPermission -Name "Map_Resources_Drive" -TargetName "Authenticated Users" -TargetType Group -PermissionLevel GpoRead
    ```
    
## Delegating Organizational Unit linking rights
* **Objective:** Grant linking permission to group
  * **Commands:** 
  ```powershell
  # Grant read and write properties for group policy links
  dsacls "OU=Klab-Enterprise,DC=klab,DC=local" /I:T /G "KLAB\GPO_Admins:RPWP;gPLink;"

  # Grant read and write properties for group policy options
  dsacls "OU=Klab-Enterprise,DC=klab,DC=local" /I:T /G "KLAB\GPO_Admins:RPWP;gPOptions;"
  ```
  * *Note:* Both gPlink and gOptions must be modified because both attributes must be referenced when creating a new gPLink. 

## Linking Group policy
* **Objective:** Link a group policy to an Organization Unit
  * **Commands:**
    ```powershell
    New-GPLink -Name "<GPO_Name>" -Target "OU=OU_Name,DC=Domain,DC=TLD"
    ```
    * **Lab Context:**
    ```powershell
    New-GPLink -Name "Map_Resources_Drive" -Target "OU=Klab-Enterprise,DC=Klab,DC=local"
    ```

-----
* *Note:* See Troubleshooting logs for Errors regarding delegating linking rights in group policy
  [Group Policy Errors Guide](../../../troubleshooting/permission-management/group-policy-object-errors.md).
