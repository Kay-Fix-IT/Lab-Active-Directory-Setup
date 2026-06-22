# Troubleshooting: NTFS ERRORS

## Delegation Control Error [6/21/2026]

1. Problem
   * **Date:** 2026/06/21
   * **Action:** Attempted to test the NTFS permissions from an account with only read permissions
   * **Error:** User `jsmith` was able to create a new folder despite having only read NTFS permissions
   * **Symptoms:** Improper permission access. 

2. Root Cause
   * Investigation:
     * Log onto workstation with domain administrative privileges
     * Run **Powershell**
     * **Command:**
       ```powershell
       (Get-Acl -Path "<Folder_Path>").Access | Format-Table IdentityReference, FileSystemRights, InheritanceFlags, AccessControlType -AutoSize
       ```
       * **Lab Context:**
         ```powershell
         (Get-Acl -Path "C:\Resources").Access | Format-Table IdentityReference, FileSystemRights, InheritanceFlags, AccessControlType -AutoSize
         ```
    * **Findings:**
      * The group user `jsmith` belongs to is restricted to read permissions
      * However, the folder inherited permissions from C: drive grant CreateDirectories` and `AppendData` to the BUILTIN\User group
    * **Conclusion:**
      * Since every user logon is automatically added to the local BUILTIN\User group, then `jsmith` inherits the permissions

3. Resolution
   * Remove the BUILTIN\USERS group from permission list
     * *Note:* Use the RemoveAccessRuleAll method related to the Object System.Security.AccessControl.FileSystemAccessRule to delete explicit rules
       * **Commands:**
         ```powershell
         $<Folder_Variable> = Get-ACL -Path "<Folder_Path>"

         #!Converts inherited rules from root drive to explicit rules
         $<Folder_Variable>.SetAccessRuleProtection($true, $true)

         $<Group_Variable> = New-Object System.Security.Principal.NTAccount("BUILTIN\Users")

         $<Folder_Variable>.RemoveAccessRuleAll((New-Object System.Security.AccessControl.FileSystemAccessRule($<Group_Variable>, <PermissionForSearch>, "Allow")))

         Set-Acl -Path "<Folder_Path>" -AclObject $<Folder_Variable>
         ```

         * **Lab Context:**
           * **Commands:**
              ```powershell
              $ACL = Get-ACL -Path "C:\Resources"

              $ACL.SetAccessRuleProtection($true, $true)

              $UserGroup = New-Object System.Security.Principal.NTAccount("BUILTIN\Users")

              $ACL.RemoveAccessRuleAll((New-Object System.Security.AccessControl.FileSystemAccessRule($UserGroup, "ReadAndExecute", "Allow")))

              Set-Acl -Path "C:\Resources" -AclObject $ACL
              ```

    * Verify permissions
      * Use the .access method again to verify that BUILTIN\users group is no longer a part of the table. 
      
4. Prevention
   * Verify the privileges you assign to all users and groups before attempting access.



