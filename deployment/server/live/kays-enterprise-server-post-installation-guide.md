# Server Configuration Guide (Live)

## 0. Pre-flight Check
* **Objective:** Identify the correct Network Interface name.
* **Command:**
```
powershell
  Get-NetAdapter

```
* **Note:** Ensure the `InterfaceAlias` in the commands below matches the output from `Get-NetAdapter`.

## 1. Network Configuration
* **Objective:** Apply static addressing and configure DNS.
* **Commands:**
- Set Static IP:

```
powershell
  New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress "192.168.0.1" -PrefixLength 24 -DefaultGateway "192.168.0.1"

```
- Configure DNS:

```
powershell
  Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "127.0.0.1"

```

## 2. Verirification
* **Objective:** Verify IP configurations
* **Command:**
```
powershell
  Get-NetIPConfiguration

```
* **Note:** Ensure the Ip Address, Subnet Mask, DefaultGateway, and DNS server is correct.

## 3. Role Deployment
* **Objective**: Install AD DS
* **Steps:**
    1. In **Configure this local server**, select **Add roles and features**
    2. Click **Next** on the Before You Begin screen
    3. Select **Role-based or feature-based installation** and click next
    4. Select proper hostname for server and click next
    5. Ensure **File and Storage services, DNS Server, and Active Directory Domain services** are enabled, then click next
    6. Click next, then install


