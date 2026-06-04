# Troubleshooting: Authentication Errors

## Secure-Trust-Error [6/4/2026]

1. Problem
   * **Date:** 2026/06/04
   * **Action:** Attempted to log into Workstation as recognized user `jsmith` but was denied access
   * **Error Message:** The security database on the server does not have a computer account for this workstation trust relationship.
   * **Symptoms:** Unable to log in as a user into the domain

2. Root Cause
   * **Steps:**
     * Login into the local admin account
       * Select Other users
       * Enter `.\<LocalAdmin>` as username
       * Enter `<password>` into password 
       * **Lab Context:**
         * Select other users
         * Enter `.\ws02` as username
         * Enter `<password>` into password
     * Verify Connection to domain account
       * **Command:**
         ```powershell
         Test-ComputerSecureChannel
         ```
         * Returned false, Authentication failed because the trust relationship between Active directory and workstation is not working properly

3. Resolution
   *  **Steps:**
      * Remove workstation from workgroup
        * Search for and select **Advanced System Settings** from the start menu
        * Go to the **Computer Name tab**
        * Select **Change**
        * Select **Workgroup** under the **Memberof** tab
        * Select **Ok** and provide the necessary credentials
        * Restart the workstation

      * Rejoin the AD domain
        * Use command `Add-Comuter -DomainName "<Domain_Name> -Restart` and enter the necessary credentials for Domain server to join the Domain.

      * Verify Domain Connection
        * Use command `(Get-CimInstance Win32_ComputerSystem).Domain`

4. Prevention
   * If a domain joined workstation remains offline for an extended period of time, the trust relationship might expire between the Active Directory Server and that workstation
   * Periodically run Test-ConputerSecureChannel to monitor device health. 



         

   
   





