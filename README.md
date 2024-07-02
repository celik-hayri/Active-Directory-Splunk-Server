# Active Directory Project Documentation

This project involves setting up an Active Directory (AD) environment using Windows Server, Ubuntu Server, and Splunk. It includes network configurations, the installation of a universal forwarder, and Sysmon configuration, all done using VirtualBox. Below are the detailed steps for the project.

## Prerequisites

- VirtualBox installed
- Windows Server ISO
- Ubuntu Server ISO
- Splunk installation files
- Sysmon executable
- Screenshots of the process (available)

## Network Diagram

Create a network diagram using Draw.io to illustrate the network setup. The diagram should include the following components:
- Windows Server (Domain Controller)
- Ubuntu Server
- Splunk Server
- Network configurations

## Step-by-Step Guide

### 1. Install Windows Server on VirtualBox

1. **Create a new Virtual Machine (VM):**
   - Name: Windows Server
   - Type: Microsoft Windows
   - Version: Windows 2019 (64-bit)

2. **Allocate resources:**
   - Memory: 4096 MB (4 GB)
   - Hard Disk: Create a virtual hard disk (at least 50 GB)

3. **Mount the Windows Server ISO:**
   - Go to the VM settings -> Storage -> Load the Windows Server ISO file.

4. **Start the VM and follow the installation steps:**
   - Select language, time, and keyboard preferences.
   - Click "Install Now".
   - Select the appropriate edition (e.g., Standard with GUI).
   - Accept the license terms.
   - Choose "Custom: Install Windows only (advanced)".
   - Select the unallocated space and click "Next" to install.

5. **Post-Installation Configuration:**
   - Set a strong administrator password.
   - Log in to the server.

### 2. Configure Active Directory on Windows Server

1. **Install AD DS role:**
   - Open Server Manager -> Add roles and features.
   - Select "Role-based or feature-based installation".
   - Select the local server.
   - Check "Active Directory Domain Services" and add features.
   - Complete the installation process.

2. **Promote the server to a domain controller:**
   - In Server Manager, click on the flag icon and select "Promote this server to a domain controller".
   - Add a new forest (e.g., domain name: example.local).
   - Set a Directory Services Restore Mode (DSRM) password.
   - Complete the wizard and restart the server.

### 3. Install Ubuntu Server on VirtualBox

1. **Create a new VM:**
   - Name: Ubuntu Server
   - Type: Linux
   - Version: Ubuntu (64-bit)

2. **Allocate resources:**
   - Memory: 2048 MB (2 GB)
   - Hard Disk: Create a virtual hard disk (at least 20 GB)

3. **Mount the Ubuntu Server ISO:**
   - Go to the VM settings -> Storage -> Load the Ubuntu Server ISO file.

4. **Start the VM and follow the installation steps:**
   - Select language and keyboard layout.
   - Configure network (use DHCP for IP addresses).
   - Set up storage (use the entire disk and set up LVM).
   - Enter profile information (username, password).
   - Install OpenSSH server.
   - Complete the installation and reboot.

### 4. Set Up Ubuntu Server for Splunk

1. **Update and upgrade the system:**
   ```bash
   sudo apt-get update && sudo apt-get upgrade -y
   ```

2. **Install VirtualBox Guest Additions:**
   ```bash
   sudo apt-get install virtualbox-guest-additions-iso
   sudo adduser <username> vboxsf
   sudo apt-get install virtualbox-guest-utils
   ```

3. **Mount shared folder:**
   ```bash
   mkdir share
   ls
   sudo mount -t vboxsf -o uid=1000,gid=1000 Downloads share/
   ls -la
   ```

### 5. Install Splunk on Ubuntu Server

1. **Install the Splunk package:**
   ```bash
   sudo dpkg -i splunk-9.2.2-d7edf6f0a15-linux-2.6-amd64.deb
   ```

2. **Configure Splunk:**
   ```bash
   cd /opt/splunk
   ls -la
   sudo -u splunk bash
   cd bin
   ./splunk install
   exit
   sudo ./splunk enable boot-start -user splunk
   ```

3. **Start Splunk:**
   ```bash
   sudo /opt/splunk/bin/splunk start --accept-license
   ```

4. **Access Splunk Web Interface:**
   - Open a web browser and go to `http://splunk-server-ip:8000`.
   - Log in with the default credentials (admin/changeme) and change the password.

### 6. Install and Configure Universal Forwarder on Windows Server

1. **Download Universal Forwarder:**
   - Visit the Splunk download page and get the Universal Forwarder package for Windows.

2. **Install Universal Forwarder:**
   - Run the installer and follow the prompts.

3. **Configure Universal Forwarder:**
   - Open a command prompt as Administrator.
   - Set up the forwarder to send data to the Splunk server:
     ```cmd
     cd "C:\Program Files\SplunkUniversalForwarder\bin"
     splunk.exe install forwarder
     splunk.exe add forward-server splunk-server-ip:9997
     splunk.exe add monitor C:\path\to\logs
     splunk.exe restart
     ```

### 7. Install and Configure Sysmon on Windows Server

1. **Download Sysmon:**
   - Download Sysmon from the Microsoft Sysinternals website.

2. **Install Sysmon:**
   - Open a command prompt as Administrator and navigate to the directory where Sysmon is downloaded.
   - Install Sysmon with a configuration file:
     ```cmd
     sysmon -accepteula -i sysmonconfig.xml
     ```

3. **Verify Sysmon Installation:**
   - Check the Sysmon service:
     ```cmd
     sc query sysmon
     ```

4. **Configure Universal Forwarder to Monitor Sysmon Logs:**
   - Add Sysmon log path to the Universal Forwarder:
     ```cmd
     splunk.exe add monitor C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx
     splunk.exe restart
     ```

### 8. Network Configuration

1. **Configure Network Interfaces:**
   - Ensure all VMs are connected to the same internal network in VirtualBox.
   - Use VirtualBox DHCP to assign IP addresses.

2. **Verify Network Connectivity:**
   - Ensure all VMs can ping each other:
     ```bash
     ping windows-server-ip
     ping ubuntu-server-ip
     ```

### 9. Final Checks and Verification

1. **Verify Active Directory:**
   - Log in to the Windows Server and use Active Directory Users and Computers to verify AD setup.

2. **Verify Syslog Forwarding:**
   - Check Splunk for incoming logs from the Ubuntu server.

3. **Verify Universal Forwarder:**
   - Check Splunk for logs forwarded from the Windows Server.

4. **Verify Sysmon Logs:**
   - Ensure Sysmon events are being captured and forwarded to Splunk.

### Screenshots

Include screenshots of the following steps:
- Windows Server installation and configuration.
- Active Directory setup.
- Ubuntu Server installation.
- rsyslog configuration.
- Splunk installation and setup.
- Universal Forwarder installation and configuration.
- Sysmon installation and configuration.
- Network configuration and verification.

## Conclusion

This document provides a comprehensive guide to setting up an Active Directory environment with Windows Server, Ubuntu Server, and Splunk using VirtualBox. Follow each step carefully to ensure a successful setup. For any issues or further assistance, refer to the respective software documentation or community forums.
