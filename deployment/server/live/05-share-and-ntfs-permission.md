# Share and NTFS Permission Configuration

## Network(SMB) Share Configuration
* **Objective:** Create a **Resource** folder that to be accessed by all devices part of domain network. 
  * **Commands:**
    ```powershell
    New-SmbShare -Name "<Designated_Folder_Name>" -Path "<Folder_Path>" -<Access_Flag> "<Group_Name>"
    ```
    * **Lab Context:**
      ```powershell
      New-SmbShare -Name "Resources" -Path "C:\Resources" -FullAccess "Everyone"
      ```
      * *Note:* Standard Full Access is provided to all users in the network for the resources folder. Everyone can read, write, modify data, and permissions in the folder.

## New Technology File System (NTFS) Permission Configuration
* **Objective:** Add security permissions to **Resource** for different groups.
  * **Commands/Script:**
    ```powershell
    # Read the folder
    $<Folder_Variable> = Get-Acl -Path "<Folder_Path>"

    # $true, $true = Block parent changes, but keep safe copies of current Admin/SYSTEM rules
    $<Folder_Variable>.SetAccessRuleProtection($true, $true)

    # Define the target user or group
    $<Target_Group_Variable> = New-Object System.Security.Principal.NTAccount("Domain_Name\Group_Name")
    $<Folder_Variable>.RemoveAccessRuleAll((New-Object System.Security.AccessControl.FileSystemAccessRule($<Target_Group_Variable>, "<Permission_Type>", "Allow")))

    # Add your custom group rules
    $<Rule_Variable> = New-Object System.Security.AccessControl.FileSystemAccessRule("<Group_Name>", "<Permission_Type>", "<Inheritance_Flags>", "<Propagation>", "Allow")
    $<Folder_Variable>.AddAccessRule($<Rule_Variable>)

    # Another method to add rights and rules
    $<Rights_Variable> = [System.Security.AccessControl.FileSystemRights]::<Permission_Type> -bor [System.Security.AccessControl.FileSystemRights]::<Permission_Type>
    $<Law_Variable> = New-Object System.Security.AccessControl.FileSystemAccessRule("<Group_Name>", $<Rights_Variable>, "<Permission_Type", "<Propagation>", "Allow")
    $<Folder_Variable>.AddAccessRule($<Law_Variable>)

    # Save the ntfs changes
    Set-Acl -Path "<Folder_Path>" -AclObject $<Folder_Variable>
    ```
    * **Lab Context:**
      * **Commands:**
        ```powershell
        # READ THE FOLDER AND SEVER INHERITANCE
        $ACL = Get-Acl -Path "C:\Resources"

        # $true, $true = Block parent changes, but keep safe copies of current Admin/SYSTEM rules
        $ACL.SetAccessRuleProtection($true, $true)

        # TARGET AND PURGE ONLY THE BUILTIN\USERS GROUP
        $UsersGroup = New-Object System.Security.Principal.NTAccount("BUILTIN\Users")
        $ACL.RemoveAccessRuleAll((New-Object System.Security.AccessControl.FileSystemAccessRule($UsersGroup, "ReadAndExecute", "Allow")))


        # ADD YOUR CUSTOM DIRECTORY GROUPS
        # GPO Admins (Full Control)
        $GPOAdminRule = New-Object System.Security.AccessControl.FileSystemAccessRule("GPO_Admins", "FullControl", "ContainerInherit, ObjectInherit", "None", "Allow")
        $ACL.AddAccessRule($GPOAdminRule)

        # HR (Read & List Only)
        $HRRights = [System.Security.AccessControl.FileSystemRights]::ReadData -bor [System.Security.AccessControl.FileSystemRights]::ListDirectory
        $HRRule = New-Object System.Security.AccessControl.FileSystemAccessRule("HR", $HRRights, "ContainerInherit, ObjectInherit", "None", "Allow")
        $ACL.AddAccessRule($HRRule)

        # Tier 2 Support (Modify)
        $Tier2SupportRights = [System.Security.AccessControl.FileSystemRights]::Modify
        $Tier2SupportRule = New-Object System.Security.AccessControl.FileSystemAccessRule("Tier2_Support", $Tier2SupportRights, "ContainerInherit, ObjectInherit", "None", "Allow")
        $ACL.AddAccessRule($Tier2SupportRule)

        # STEP 4: SAVE THE TRUE NTFS CHANGES TO THE FOLDER
        Set-Acl -Path "C:\Resources" -AclObject $ACL
        ```
## Verify your permission Configuration
* **Commands:**
  ```powershell
  $<Folder_Variable>.Access | Format-Table IdentityReference, FileSystemRights, InheritanceFlags, AccessControlType -AutoSize
  ```
  * *Note:* Use the same folder variable as your previous commands

  * **Lab Context:**
  ```powershell
  $ACL.Access | Format-Table IdentityReference, FileSystemRights, InheritanceFlags, AccessControlType -AutoSize
  ```
  
--------------------
* *Note:* See Troubleshooting logs for Error regarding builtin users and inherited permissions
  [NTFS Errors Guide](../../../troubleshooting/permission-management/ntfs-errors.md).

