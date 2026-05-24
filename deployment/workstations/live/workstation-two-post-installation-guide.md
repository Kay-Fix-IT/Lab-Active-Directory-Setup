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
  New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "192.168.0.5" -PrefixLength 24 -DefaultGateway "192.168.0.1"

```
- Configure DNS:

```
powershell
  Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "192.168.0.1"

```

## 2. Verirification
* **Objective:** Verify IP configurations
* **Command:**
```
powershell
  Get-NetIPConfiguration

```
* **Note:** Ensure the Ip Address, Subnet Mask, DefaultGateway, and DNS server is correct.

## 3. Verify connection
* **Objective:** Verify connection to the domain DNS services
* Before we add the workstation to our domain we want to ensure we have a connection to the domain
* **Comands:**
```
powershell
  Test-NetConnection -ComputerName "Domain IP here" -port 53
```
```
cmd
  ping "Domain IP here"
```
* **Note:** For our lab purposes, our domain IP address is `192.168.0.1`

## 4. Add workstation to domain
* **Objective:** Add your workstation to the Active directory domain Network
* **Command:**
```
powershell
  Add-Computer -ComputerName "Domain name here" -Restart
```
* **Note:** For our lab purposes, our FQDN is `klab.local`
  - A secure credential pop up will appear on the screen. Enter your username and password credentials.
    - Username will be your domain\Administrator `(eg. KLAB\Administrator)`
    - Password will be the password created when promoting the Domain controller
      **Note:**- This should be stored in a safe a secure internal location

## 5. Verify Joining
* **Objective:** Verify that your workstation is now part of the Active Directory Domain by logging into your workstation
* **Steps:** 
  1. While on the log in screen, select **Other Users**
  2. Enter your Domain controller username and password
  3. Enter command `(Get-CimInstance Win32_ComputerSystem).Domain` in powershell to confirm new domain
    - **Note** For the purpose of our lab, the domain should be `klab.local`







