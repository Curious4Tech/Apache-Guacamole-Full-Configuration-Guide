
# Apache Guacamole Server Setup Guide

This guide provides a step-by-step setup for creating users and connecting both Windows and Linux servers to your Apache Guacamole server, enabling remote access via a web browser.

## Prerequisites

- An installed and running Apache Guacamole server.
- SSH access to the Guacamole server.
- Administrator access to the target Windows and Linux servers.
- Proper network configuration (firewalls, etc.) allowing connections on required ports.

---

## Step 1: Access Guacamole and Create Admin User

1. **Log in to the Guacamole Web Interface:**
   - Open a web browser and navigate to your Guacamole server’s IP or domain.
   - Example: `http://<Guacamole_IP>/guacamole`

2. **Log in as Admin:**
   - Default username is `guacadmin` with the password set during installation.
   - **Tip:** Change this default password after initial login for security.

3. **Create a New User:**
   - Go to **Settings > Users**.
   - Click **New User**.
   - Enter a username and password.
   - Assign necessary permissions (e.g., access to specific connections).
   - Save the user.

---

## Step 2: Configure Connections in Guacamole

### A. Adding a Windows Server Connection

1. **Prepare the Windows Server:**
   - Ensure **Remote Desktop (RDP)** is enabled on the Windows server.
   - Open port **3389** in firewall settings for RDP connections.

2. **Add the Windows Connection in Guacamole:**
   - Go to **Settings > Connections**.
   - Click **New Connection**.
   - Set **Protocol** to **RDP** and complete these fields:
     - **Name**: Enter a recognizable name (e.g., “Windows Server”).
     - **Hostname**: Enter the IP or hostname of the Windows server.
     - **Port**: Default is **3389**.
     - **Username and Password**: Enter Windows server credentials.
     - (Optional) **Security Mode**: Set to **NLA** or **RDP** as required.
   - Save the connection.

3. **Test the Connection:**
   - On the Guacamole dashboard, select the Windows connection and verify successful connection.

### B. Adding a Linux Server Connection

1. **Prepare the Linux Server:**
   - Ensure **SSH** is enabled on the Linux server.
   - Open port **22** in firewall settings for SSH access.

2. **Add the Linux Connection in Guacamole:**
   - Go to **Settings > Connections**.
   - Click **New Connection**.
   - Set **Protocol** to **SSH** and complete these fields:
     - **Name**: Enter a recognizable name (e.g., “Linux Server”).
     - **Hostname**: Enter the IP or hostname of the Linux server.
     - **Port**: Default is **22**.
     - **Username and Password**: Enter Linux server credentials (or an SSH key if required).
   - Save the connection.

3. **Test the Connection:**
   - On the Guacamole dashboard, select the Linux connection and verify successful connection.

---

## Step 3: Configure User Access to Connections

1. **Assign Connections to Specific Users:**
   - Go to **Settings > Users**.
   - Click on the user to assign connections.
   - Under **Connections**, check the boxes for the Windows and Linux connections you want the user to access.
   - Save the changes.

2. **Verify User Access:**
   - Log in as the newly created user and confirm they can only access assigned connections.

---

## Step 4: Adjust Security and Optional Settings

1. **Enable Two-Factor Authentication (2FA)** if required (Guacamole supports various 2FA options).
2. **Restrict IP Access** to limit access to Guacamole.
3. **Configure User Groups** for managing multiple users and connections.

---

## Troubleshooting

- **Connection Issues**: Ensure network allows necessary ports (3389 for RDP, 22 for SSH).
- **Permissions**: Verify user permissions for each connection in Guacamole.
- **Firewall Rules**: Confirm firewall rules on both Guacamole and target servers.

---

## Conclusion

Once everything is set up, users will be able to remotely access their assigned servers via the Guacamole web interface.

Happy connecting! 🖥️🔒
