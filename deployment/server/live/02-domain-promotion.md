## 3. Role Deployment
* **Objective**: Install AD DS
* **Steps:**
    1. In **Configure this local server**, select **Add roles and features**
    2. Click **Next** on the Before You Begin screen
    3. Select **Role-based or feature-based installation** and click next
    4. Select proper hostname for server and click next
    5. Ensure **File and Storage services, DNS Server, and Active Directory Domain services** are enabled, then click next
    6. Click next, then install.

## 4. Rename Computer
* **Objective**: Rename the default appointed name of Server
* **Steps:** 
    1. Use command `Rename-Computer -NewName "NewName" -Restart` in powershell
        -**Note:** Changes the computer hostname and forces a restart

## 4. Domain Promotion
* **Objective:** Promote the server to a Domain Controller
* **Steps:**
    1. In **Server Manager**, click the **Notifications** flag and select **Promote this server to a domain controller**.
    2. Select **Add a new forest**.
    3. Enter Root domain name: (eg: `klab.lan`)
    4. Set DSRM password. 
       - **Note**: Password saved in secure internal password manager.*
    5. Proceed through remaining prompts with default settings.
    6. Click Install.
* **Post-Action:** The server will automatically restart to complete the promotion.