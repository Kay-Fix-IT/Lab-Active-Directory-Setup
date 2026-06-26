# Editing A Group Policy

## Mapping A drive to A Network Share
* **Objective:** Edit group policy to map a drive to network share
  * *Note:* See [06-gpo-management.md](./06-gpo-management.md) for lab context regarding gpo creation
  * **Steps**
    1. Provide editing capabilitites to object
       * **Commands:** 
         ```powershell
         Set-GPPermission -Name "<GPO_Name>" -TargetName "Group_Name" -TargetType <Group_Type> -PermissionLevel <Permission_Level>
         ```
         * **Lab Context:**
         ```powershell
         Set-GPPermission -Name "Map_Resources_Drive" -TargetName "GPO_Admins" -TargetType group -PermissionLevel GpoEdit
         ```
      
    2. Navigate to workstation with RSAT tools
       * Log in as a member of <Group_name> (LC`tcroft`, `GPO_Admins`)
       * Open group policy management console (gpmc.msc)
       * Navigate to **group policy objects**
       * Right click your <GPO_Name>
       * Select **Edit**
       * On the right side of the panel, Select **User Configuration**
       * Select **Preferences**
       * Select **Windows Settings**
       * Select **Drive Maps**
       * Right click and select **New**
       * Select **Mapped Drive**
       * Right the dropdown menu next to action, Select **Update**
       * In **location**, paste your Folder location
         * *Note:* It should be your UNC Path (LC `\\WIN-2L2INDLOH1I\Resources`)
       * Select your choice of **Drive letter** (LC `R:`)

    3. Verify
       * Log into appropriate user accounts
       * Navigate to **File explorer**
       * Select **This Pc**
       * You should see the drive letter `R:`
