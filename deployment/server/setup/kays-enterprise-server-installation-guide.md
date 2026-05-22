# Build Guide: Kays Enterprise Server

## Prerequisites
* **Hypervisor:** Oracle VirtualBox
* **ISO:** Windows Server 2022 Evaluation
* **Network:** Internal Network "Kintnet"

## Step-by-Step Installation
1. **VM Creation:** Allocated 4GB RAM, 2 CPU cores, and 60GB dynamically allocated storage.
2. **Network Configuration:** Set adapter to "Internal Network" (Kintnet).
3. **OS Installation:** Performed standard install of Windows Server 2022 (Desktop Experience).

## Installation Workarounds
- **Bypassing Network Requirements:**
  1. At the initial setup screen, press `Shift + F10` to open the Command Prompt.
  2. Type the following command and press Enter:
     
    ```
    cmd
     OOBE\BYPASSNRO
    ```
  3. The system will restart automatically. 
  4. On the "Let's connect you to a network" screen, select **"I don't have internet"** to proceed with a local account setup
