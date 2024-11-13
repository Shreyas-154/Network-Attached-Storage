# Secure NAS Setup with OpenMediaVault on Raspberry Pi or Old PC

This guide will help you create a secure Network Attached Storage (NAS) using OpenMediaVault (OMV) on a Raspberry Pi or old PC. We’ll also add OpenVPN, SSH, HTTPS, and Fail2Ban for enhanced security.

## Prerequisites

- **Hardware**: Raspberry Pi (3 or 4 recommended) or an old PC.
- **Storage**: USB drives or internal hard drives for storage.
- **Software**: [Balena Etcher](https://www.balena.io/etcher/) or [Rufus](https://rufus.ie/) for creating a bootable SD card/USB drive.

---

## Step 1: Install OpenMediaVault (OMV)

### For Raspberry Pi

1. **Download the OMV Image**:
   - Download the Raspberry Pi image from the [OpenMediaVault website](https://www.openmediavault.org/download.html).
   
2. **Write the OMV Image**:
   - Use Balena Etcher or Rufus to flash the OMV image to an SD card.

3. **Boot the Raspberry Pi**:
   - Insert the SD card into your Pi, connect it to Ethernet, and boot it up.

4. **Access the OMV Web Interface**:
   - In a browser, go to `http://<device-ip>` and log in with default credentials:
     - **Username**: `admin`
     - **Password**: `openmediavault`

### For Debian-based OS on PC (e.g., Debian or Ubuntu)

If you’re installing OMV on a PC running Debian or Ubuntu, use the following command to install OMV directly:

```bash
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
## Step 2: Configure Storage
```

1. **Attach Storage Drives**: Connect your storage drives (USB or internal).
2. **Create a Filesystem**:
    - In the OMV interface, go to **Storage** > **File Systems**.
    - Click **Create**, select your drive, and choose `ext4` as the filesystem.
    - Mount the filesystem after creation.
3. **Create Shared Folders**:
    - Go to **Access Rights Management** > **Shared Folders** and add a new shared folder.

---

## Step 3: Set Up SMB/CIFS for File Sharing

1. **Enable SMB/CIFS**:
    - Go to **Services** > **SMB/CIFS** in the OMV interface and enable it.
2. **Add Shared Folders**:
    - Add the folder you created under **Shares** and configure access permissions.

---

## Step 4: Install and Configure Security Protocols (OpenVPN, SSH, HTTPS, and Fail2Ban)

### Install Necessary Plugins

1. **Update Plugins**:
    - Go to **System** > **Plugins** and click **Check** to refresh the plugin list.
2. **Install Security Plugins**:
    - Open a terminal on the machine running OMV and use the following commands to install OpenVPN, SSH, and Fail2Ban:

    ```bash
    sudo apt update
    sudo apt install openvpn openssh-server fail2ban
    ```

### OpenVPN (For Secure Remote Access)

1. **Configure OpenVPN**:
    - Go to **Services** > **OpenVPN** in OMV (if installed through the plugin) or configure via terminal.
    - Generate certificates, set up configurations, and download the `.ovpn` file for your client.
2. **Start OpenVPN**:
    - In terminal, use:

    ```bash
    sudo systemctl enable openvpn
    sudo systemctl start openvpn
    ```

### SSH (For Encrypted Remote Access)

1. **Enable SSH**:
    - Go to **Services** > **SSH** in OMV and enable it, or in terminal:

    ```bash
    sudo systemctl enable ssh
    sudo systemctl start ssh
    ```

2. **Set up Key Authentication** (optional for more security):
    - Generate SSH keys on your client machine with:

    ```bash
    ssh-keygen
    ```

    - Copy the public key to your NAS:

    ```bash
    ssh-copy-id username@<device-ip>
    ```

### HTTPS (For Secure Web Interface Access)

1. **Enable HTTPS**:
    - Go to **System** > **General Settings** > **Web Administration** and enable **HTTPS**.
2. **Generate SSL Certificate**:
    - To generate a self-signed certificate, use the following command in the terminal:

    ```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/omv.key -out /etc/ssl/certs/omv.crt
    ```

    - Import the certificate in OMV under **Certificates** > **SSL**.

### Fail2Ban (For Automated Intrusion Prevention)

1. **Configure Fail2Ban**:
    - Go to **Services** > **Fail2Ban** in OMV, or configure directly via terminal.
2. **Configure Fail2Ban via Terminal**:
    - Edit the Fail2Ban configuration file:

    ```bash
    sudo nano /etc/fail2ban/jail.local
    ```

    - Set up jails for SSH, HTTP, and other services, then restart Fail2Ban:

    ```bash
    sudo systemctl restart fail2ban
    ```

---

## Step 5: Testing and Maintenance

1. **Test Access**:
    - Test SMB/CIFS to confirm file sharing works.
    - Test OpenVPN or SSH for secure remote access.
2. **Regular Updates**:
    - Keep OMV and plugins updated via **System** > **Update Management** in OMV or use:

    ```bash
    sudo apt update && sudo apt upgrade
    ```

---

## Conclusion

Your NAS setup is now complete with OpenMediaVault, enhanced by secure access protocols. You’re ready to store and share files safely with OpenVPN, SSH, HTTPS, and Fail2Ban.

Enjoy your secure NAS!






