# Troubleshooting: Permission Errors

## Delegation Control Error [6/7/2026]

1. Problem
   * **Date:** 2026/06/07
   * **Action:** Attempted disable account for user `jsmith` but was denied access
   * **Error Message:** Unable to disable account because access was denied
   * **Symptoms:** Denied access to perform actions and editing user account

2. Root Cause
   * Investigation:
     * Log into the domain Server Active Directory Domain Services gui
     * Go to tools and select the **Active Directory Users and Computers tool**
     * Cick view and select **Advanced Features**
     * Go to your desired OU, and user.
     * **Lab context:**, 
       * *Note:* it is Klab-Enterprise -> Standard Employee -> John smith
     * Right click on user, `jsmith`
     * Select **properties**
     * Select the **security tab**
     * Select **Advanced**
     * Select the **Effective Access** tab
     * Select select user next to **User/Group**
     * Type in the `HelpDesk_Admin` user account name. 
       * **Lab context:**
         * `hdesk01`
         * *Note:* You should see a UPN in front of User/Group
     * Select **view effective access**
     * You can view the enabled and disabled permisssions.
       * **Lab Context:** The user `hdesk01` did not have permissions to change password, reset password, perform user account control, and more on the user `jsmith` 
     * The code `dsacls /G <OUPath> "<Group>:<permissions>"`was used to provide permission to the group `HelpDesk_Admin`.
       * *Note:* By not including a /I:S flag, the Grant permission is only being applied to the `<OUPath>` but not the its descendants. 
  
3. Resolution
   * Run dsacls command but use the /I:S flag
     * **Lab Context:**
       * **Commands:**
         ```powershell
         # Delegate permissions with Object Inheritance (/I:S)
         # LCRP = List Contents, Read Permissions
         # RPWP = Reset Password, Write Password
         # CA = Control Access (required for Reset/Change Password)
         # WP = Write Property (specific attributes)

         $OUPath = "OU=Standard-Employees,OU=Klab-Enterprise,DC=klab,DC=local"

         # 1. Grant List and Read permissions
         dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:LCRP;;user"

         # 2. Grant Password Control
         dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:CA;Change Password;user"
         dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:CA;Reset Password;user"

         # 3. Grant specific attribute modification
         dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:WP;pwdLastSet;user"
         dsacls $OUPath /I:S /G "KLAB\HelpDesk_Admins:WP;userAccountControl;user"
       * *Note:* Granting specific permissions also help promote least priviledge ideology

      * Verify permissions
        * Log into your helpdesk user account on workstation with RSAT ADUC tool, `hdesk01`
        * Select desired user account to perform admin actions
          * **Lab context:** Klab-Enterprise -> Standard-Employees -> jsmith
      
4. Prevention
   * Verify the priviledges you assign to all users and groups before attempting access.
     * *Note:* You can use Effective Access method again



