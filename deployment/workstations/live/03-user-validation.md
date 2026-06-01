# User Validation: Domain Authentication

## Authentication Verification [06/01/2026]

1. Objective
   * **Target:** Verify that a newly created user (e.g., `jsmith`) can successfully authenticate against the Active Directory domain (e.g., `klab.local`).
   * **Environment:** Virtualized Windows Workstation.

2. Prerequisites
   * The domain controller must be online and reachable.
   * Target user account must be provisioned in Active Directory.
     * *Note:* Windows may allow a false-positive login using cached credentials to an offline Active Domain controller. Ensure that your domain controller server is online to perform a live verification. 

3. Steps
   1. Launch the target Workstation from Virtual Machine Manager.
   2. Send the **Ctrl+Alt+Delete** command to initialize the Windows logon screen.
   3. Select **Other User** to sign into the Active Directory domain.
      * *Note:* If Windows displays a cached user profile, bypass this step and proceed directly to credential entry.
   4. Input the user credentials:
      * **Username:** User Principal Name (e.g., `jsmith@klab.local` or `jsmith`)
      * **Password:** The designated password created for the user account.
   5. Confirm successful desktop loading and profile creation.

4. Validation
   * Open PowerShell and verify the active domain context by running:
     ```powershell
     $env:USERDOMAIN
     ```
   * **Expected Result:** The output must display the NetBIOS domain name (e.g., `KLAB`).