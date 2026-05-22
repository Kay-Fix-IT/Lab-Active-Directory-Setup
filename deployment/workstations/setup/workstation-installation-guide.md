## Prerequisites
* **Hypervisor:** Oracle VirtualBox
* **ISO:** Windows 11 25H2 English x64
* **Network:** Internal Network "Kintnet"

## Step-by-Step Installation
1. **Edition:** Windows 11 Pro
2. **VM Creation:** Allocated 4GB RAM, 2 CPU cores, and 80GB dynamically allocated storage.
3. **Network Configuration:** Set adapter to "Internal Network" (Kintnet).
4. **OS Installation:** Performed standard install of Windows 11 Pro.

## Installation Workarounds
- **Bypassing Network Requirements:**
  1. At the initial setup screen, press `Shift + F10` to open the Command Prompt.
  2. Type the following command and press Enter:
     
    ```
     OOBE\BYPASSNRO
    ```
  3. The system will restart automatically. 
  4. On the "Let's connect you to a network" screen, select **"I don't have internet"** to proceed with a local account setup
