# Network Configuration

## Perequsite
*  **Objective:** Identify the correct Network Interface name.
   * **Command:**
     ```powershell
     Get-NetAdapter
     ```
   * *Note:* Ensure the `InterfaceAlias` in the commands below matches the output from `Get-NetAdapter`.

## Network Configuration
*  **Objective:** Apply static addressing and configure DNS.
   * **Commands:**
     * **Set Static IP**:
       ```powershell
       New-NetIPAddress -InterfaceAlias "<Alias_Name>" -IPAddress "<IP_Address>" -PrefixLength 24 -DefaultGateway "<Gateway_IP_Address>"
       ```
       * **Lab Context:**
        ```powershell
        New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "192.168.0.1" -PrefixLength 24 -DefaultGateway "192.168.0.1"
        ```
     * **Configure DNS:**
       ```powershell
       Set-DnsClientServerAddress -InterfaceAlias "<Interface_Name>" -ServerAddresses "<Preferred_IP_Address>"
       ```
       * **Lab Context:**
       ```powershell
       Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "127.0.0.1"
       ```
## Verirification
*  **Objective:** Verify IP configurations
   * **Command:**
   ```powershell
      Get-NetIPConfiguration
   ```
   * *Note:* Ensure the Ip Address, Subnet Mask, DefaultGateway, and DNS server is correct.