# Troubleshooting: OU Creation Error

## Directory Object Not Found [5/25/2026]

1. Problem
   * **Date:** 2026-05-25
   * **Action:** Attempted to run `New-ADUser` script
   * **Error Message:** Directory object not found
   * **Symptoms:** Command failed to execute and returned red text error in PowerShell.

2. Root Cause
   * The command failed due to a typo in the Organizational Unit (OU) name. 
   * **Example:** Attemped to create user `jsmith` in OU but the `-path` provided in the cmdlet did not match the existing distinguished name of the existing OU (`Standard-Employee` vs `Standard-Employees`)

3. Resolution
   1. Opened the **Active Directory Users and Computers (ADUC)** GUI.
   2. Renamed the target OU to match the script's expected path (`Standard-Employees`).
   3. Reran the `New-ADUser` script to successfully mint the user account.

4. Prevention
   * Always double-check OU string paths for trailing plurals or typos before running automation scripts.
   * *Note:* While Active Directory Distinguished Names (DNs) are generally case-insensitive, exact spelling matches are strictly required.

## UPN value provided for addition/modification is not unique forest-wide [5/25/2026]

1. Problem
   * **Date:** 2026-05-25
   * **Action:** Attempted to run `New-ADUser` script
   * **Error Message:** ADException: The operation failed because UPN value provided for addition/modification is not unique forest-wide
   * **Symptoms:** Command failed to execute and provided red text error in PowerShell

2. Root Cause:
   * The command failed because the User Principa Name (UPN) assigned to jsmith is already in use by an existing user account in the Active Directory forest

3. Resolution:
   * **Steps**:
   1. Search for an existing user account using the sAMAccountName
   ```powershell
   Get-ADUser -filter 'SamAccountName -eq "jsmith"'
   ```
   2. After confirming a duplicate account exists, remove account  
   ```powershell
   Remove-ADUser -identity "jsmith"
   ```
   3. Rerun the script `New-ADUser`

4. Prevention
   * Always check the Active Ditectory environment for existing duplicate UPNS and sAMAccountName before running Automated creation scripts
   * *Note:* Active Directory(AD) requires a User Principel Name to be completely unique across the entire AD forest to prevent authentication conflicts
