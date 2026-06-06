# Application Management Errors

## Rsat Failed installation - Network Isolation [6/4/2025]

1. Problem
   * **Date:** 6/4/2026
   * **Action:** Attempted to install RSAT Active directory and Domain Serives into a workstation
   * **Error message:** Couldn't add
   * **Symptoms:** The Installation of the RSAT feature could not be completed. 

2. Root Cause
   * Worksation exists in an isolated internal network using the Main Active Directory Server as its DNS and default gateway.
   * RSAT tools features need connection to the internet to be downloaded over the web.

3. Resolution
   * **Steps:**
     * Select Network settings in the Vm settings for the worksation and change the adpater type to **NAT** or **Bridged**. 
     * Restart the worksation Vm.
     * Try to install the RSAT tool again
     * Change the network settings and the adapter type back to the domain internal network
     * Restart the worksation vm and setup preferred static addresses, DNS, and gateway [Worksation2 installation guide](Lab-Active-Directory-Setup\deployment\workstations\live)

4. Prevention
   * Future lab builds that requires Features on demand should have temporary internet connection enabled or utilize an offline feauture on demand iso media mounted on the vm. 
