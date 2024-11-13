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
   - Example: `http://<Guacamole_Server_IP>/guacamole`

2. **Log in as Admin:**
   - Default username is **`guacadmin`** with the password set during installation.
   - **Tip:** Change this default password after initial login for security.


![image](https://github.com/user-attachments/assets/6b0694c1-3227-4828-b6a2-62e3ba2ef29c)


3. **Create a New User:**
   - Go to **Settings > Users**
   - Click **New User**.
  

![image](https://github.com/user-attachments/assets/6ad9822b-f766-422f-89d8-31c9905e52a3)


   - Enter a **username** and **password**.
   - Assign necessary **permissions** (e.g., access to specific connections).
   - Save the user.

![image](https://github.com/user-attachments/assets/cada6946-904a-45b7-9b1d-66160cb408e5)

---

## Step 2: Configure Connections in Guacamole

### A. Adding a Windows Server 2022 Connection

1. **Prepare the Windows Server 2022:**
   - Ensure **Remote Desktop (RDP)** is enabled on the Windows server.
   - Open port **3389** in firewall settings for RDP connections.
  

![image](https://github.com/user-attachments/assets/ce5772ed-8e86-481e-b545-33b633ff488a)


2. **Add the Windows Connection in Guacamole:**

But before that, we’ll create a new group, as these groups will help organize the connections: **`Settings > Connection > New Group`**


![image](https://github.com/user-attachments/assets/1d7b4dc2-9844-449d-85db-8c4c5565233d)


In this example, I’m creating a group named **`Servers`**. It will be placed under the **`ROOT`**  location, which is the root of the hierarchy. The group type **`Organizational`** should be selected for all groups intended to organize connections. Click on **`Save`** to create the group.


![image](https://github.com/user-attachments/assets/83356e35-7ab7-4666-8f1b-f944f7fcf8d5)


   - Go to the new group you created, expand it and then click on **`New Conncetion``**.


![image](https://github.com/user-attachments/assets/9580ac91-6ffc-4f90-bddc-fafcb409b675)


   - **Name**: Enter a recognizable name (**`your server name`**).
   - **Location**: Choose the group you previously created.
   - **Protocol** to **RDP** and complete these fields.
![image](https://github.com/user-attachments/assets/811002e3-705f-4c99-8066-481a1ce0b235)

   
     - **Hostname**: Enter the IP or hostname of the Windows server.
     - **Port**: Default is **3389**.
     - **Username and Password**: Enter Windows server credentials.
     - (Optional) **Security Mode**: Set to **NLA** or **RDP** as required.
     - **Ignore the server certificate** : check it.
![image](https://github.com/user-attachments/assets/f07da366-a60a-41d9-a0ac-a7ce89795f7b)

      - **Performance**: Check your preferred ones.

 
 ![image](https://github.com/user-attachments/assets/7e7fb205-5aa2-4db9-ab7d-e58565a969d0)
     - Save the connection: Scroll down a little bit and then click  on **`Save`**
     
![image](https://github.com/user-attachments/assets/8f0de78d-3e24-41fe-8705-e2a187308be8)

You can also configure **time zone** and **keyboard layout** depending your location and language.


3. **Test the Connection:**
   - On the Guacamole dashboard, select the Windows connection and verify successful connection.

### B. Adding a Linux Server Connection

1. **Prepare the Linux Server:**
   - Ensure **SSH** is enabled on the Linux server.
   - Open port **22** in firewall settings for SSH access.


![image](https://github.com/user-attachments/assets/71a41224-bdcc-45e3-9d4f-743333cb7139)


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

