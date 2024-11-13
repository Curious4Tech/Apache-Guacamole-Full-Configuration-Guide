# Guacamole-Setup-On-Debian 12-Server
A comprehensive installation guide for Apache Guacamole on Debian 12 Server, including MySQL authentication setup, Tomcat configuration, and security best practices. Perfect for sys admins and developers setting up remote desktop access.

# Apache Guacamole Installation Guide for Debian

This guide provides step-by-step instructions for installing Apache Guacamole on Debian. Apache Guacamole is a clientless remote desktop gateway that supports standard protocols like VNC, RDP, and SSH.

# Prerequisites

- Debian 12 operating system
- Root or sudo privileges
- Terminal access
- Internet connection

# Installation Steps

## Setp 1. Install Dependencies

First, update your package list and install required dependencies:

### Update package list

```bash
sudo apt-get update && sudo apt-get upgrade -y

```

![image](https://github.com/user-attachments/assets/41bbd5fb-440a-4d46-a32f-2668cd2fe2a3)


### Install required packages

```bash
sudo apt-get install -y build-essential libcairo2-dev libjpeg62-turbo-dev libpng-dev libtool-bin uuid-dev libossp-uuid-dev libavcodec-dev libavformat-dev libavutil-dev libswscale-dev freerdp2-dev libpango1.0-dev libssh2-1-dev libtelnet-dev libvncserver-dev libwebsockets-dev libpulse-dev libssl-dev libvorbis-dev libwebp-dev
```


![image](https://github.com/user-attachments/assets/8a0e9edd-9657-453d-a296-68908ba4047c)


## Step 2. Download and Build Guacamole Server

```bash
# Create and navigate to `/tmp`directory
 cd /tmp

# Download Guacamole server

sudo wget https://downloads.apache.org/guacamole/1.5.5/source/guacamole-server-1.5.5.tar.gz

```


![image](https://github.com/user-attachments/assets/3c9f7afb-73d3-4191-9673-1c8fdb4a0e0e)


### Extract and build

```bash
sudo tar -xzf guacamole-server-1.5.5.tar.gz
cd guacamole-server-1.5.5
sudo ./configure --with-init-dir=/etc/init.d

```

![image](https://github.com/user-attachments/assets/543ba282-081d-4b13-a536-3326047bb56e)

### Run these following commands to install guacamole server.
```bash
sudo make
sudo make install
sudo ldconfig
```

![image](https://github.com/user-attachments/assets/72ba9ce7-0b4a-4cae-86cc-7e0a097984a8)


## Step 3. Configure and Start guacd Service

```bash
# Start and enable guacd service
sudo systemctl daemon-reload
sudo systemctl start guacd
sudo systemctl enable guacd
sudo systemctl status guacd
```

![image](https://github.com/user-attachments/assets/cf8f0f46-967b-48a1-b121-394e80f71d6c)

The problem is realated to **`sudo ./configure --with-init-dir=/etc/init.d`** command that we excuted previously. After running or executing the  **`sudo ./configure --with-init-dir=/etc/init.d`** command if you see that  **` Systemd units: no`**, then you may accounter the same problem as mine here. Normally, you have to see : **` Systemd units:  /etc/systemd/system`**. So if you don't accounter that kind of problem, you can skip the **`Step 4`**.


![image](https://github.com/user-attachments/assets/1eacc668-0329-4824-9379-421ab0ceb29d)


## Step 4. Solve problem (Optional, in case you accounter that type of problem)

It looks like we've successfully configured some parts of Guacamole, but the systemd units are not installed. This is what causing issues with starting guacd as a systemd service. Here’s how to solve it.

Let's make sure the systemd service file is correctly set up.

### Create a systemd service file:

   1. Open a text editor and create a new file named **`guacd.service`** in the **`/etc/systemd/system/`** directory:
```bash
sudo nano /etc/systemd/system/guacd.service

```
  2. Edit the service file, add the following content to the guacd.service file:
  
```bash
[Unit]
Description=Guacamole Daemon
After=network.target

[Service]
ExecStart=/tmp/guacamole-server-1.5.5/src/guacd/guacd -f
User=root
Group=root
Restart=always

[Install]
WantedBy=multi-user.target

```


![image](https://github.com/user-attachments/assets/e9d3325b-3069-444f-ba0d-8439563906f1)


  3. Save and Close the File: Save the changes and close the text editor.

  4. Reload the systemd manager configuration to apply the changes, start and enable guacd service **(Repeate the Step 3)**.

```bash

sudo systemctl daemon-reload
sudo systemctl start guacd
sudo systemctl enable guacd
sudo systemctl status guacd

```

![image](https://github.com/user-attachments/assets/a5b6c39f-dc89-4657-afcf-ccd3626ff110)


## Step 5. Since Guacamole is not compatible with Tomcat 10 (default in Debian 12), we need to install Tomcat 9:

 ### Add Debian 11 (Bullseye) repository
 
```bash
# Create new source file
sudo nano /etc/apt/sources.list.d/bullseye.list

# Add this line to the file:
 deb http://deb.debian.org/debian/ bullseye main
```

![image](https://github.com/user-attachments/assets/34f4e4c2-264e-418f-826f-656dfb47f893)


### Update and install Tomcat 9

```bash
sudo apt-get update
sudo apt-get install -y tomcat9 tomcat9-admin tomcat9-common tomcat9-user
```

![image](https://github.com/user-attachments/assets/aef56255-b35f-4f6c-a684-9cc3321caf26)


### Step 6. Install Guacamole Web Application

# Download and deploy web application

```bash
cd /tmp
wget https://downloads.apache.org/guacamole/1.5.5/binary/guacamole-1.5.5.war
sudo mv guacamole-1.5.5.war /var/lib/tomcat9/webapps/guacamole.war

```

![image](https://github.com/user-attachments/assets/765db6da-94be-433d-ad30-a42f239b0ab5)


# Create configuration directories
sudo mkdir -p /etc/guacamole/{extensions,lib}


### Step 7. Set Up MariaDB

```bash

# Install MariaDB
sudo apt-get install -y mariadb-server

# Secure MariaDB installation
sudo mysql_secure_installation

```


![image](https://github.com/user-attachments/assets/330ccc1e-4bdb-4476-936c-341e5f46fdf1)


# Log in to MariaDB and create a database and userfor our Guacamole server

```bash
# Log in to MariaDB
sudo mysql -u root -p

# Create database and user (in MySQL prompt)
CREATE DATABASE guacadb;
CREATE USER 'guacadmin'@'localhost' IDENTIFIED BY 'Your_Strong_Password_Here'; # Replace with your password
GRANT SELECT,INSERT,UPDATE,DELETE ON guacadb.* TO 'guacadmin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

![image](https://github.com/user-attachments/assets/1d3693e5-6284-4f0e-9d24-adae7399ce35)


### Step 7. Install MySQL Extension and Connector


# Download and install JDBC extension

```bash
cd /tmp
sudo wget https://downloads.apache.org/guacamole/1.5.5/binary/guacamole-auth-jdbc-1.5.5.tar.gz
sudo tar -xzf guacamole-auth-jdbc-1.5.5.tar.gz
sudo mv guacamole-auth-jdbc-1.5.5/mysql/guacamole-auth-jdbc-mysql-1.5.5.jar /etc/guacamole/extensions/

```


![image](https://github.com/user-attachments/assets/b4e7ce66-3569-4f51-bb7f-3e4ac54eb2ea)


# Download and install MySQL Connector

```bash
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-9.1.0.tar.gz
sudo tar -xzf mysql-connector-j-9.1.0.tar.gz
sudo cp mysql-connector-j-9.1.0/mysql-connector-j-9.1.0.jar /etc/guacamole/lib/

```


![image](https://github.com/user-attachments/assets/ab49a75b-ebbf-4e34-aba3-453f627df0c8)


# Initialize database schema

```bash

cd guacamole-auth-jdbc-1.5.5/mysql/schema/
cat *.sql | sudo mysql -u root -p guacadb

```

### Step 8. Configure Guacamole

```bash
# Create and edit the properties file:

sudo nano /etc/guacamole/guacamole.properties

# Add the following content (replace with your password):

mysql-hostname: 127.0.0.1
mysql-port: 3306
mysql-database: guacadb
mysql-username: guacadmin
mysql-password: Your_Strong_Password_Here  # Replace with the same password used in Step 6
```


![image](https://github.com/user-attachments/assets/b877df17-99af-4fc0-a491-dd574b932d3d)


### Step 9. Configure guacd

```bash

# Create and edit the configuration file:

sudo nano /etc/guacamole/guacd.conf

# Add the following content:
[server]
bind_host = 0.0.0.0
bind_port = 4822
```

![image](https://github.com/user-attachments/assets/7000177c-8b0b-4450-8f71-d35ce075a025)


### 10. Final Steps

```bash
# Restart all services
sudo systemctl restart tomcat9 guacd mariadb
```

## Accessing Guacamole

1. Open your web browser and navigate to:
```
http://<your-server-ip>:8080/guacamole
```

2. Default login credentials:
   - Username: `guacadmin`
   - Password: `guacadmin`

> **Important Security Note**: Make sure to change the default password immediately after your first login!

## Troubleshooting

If you encounter any issues:
1. Check service status: `sudo systemctl status guacd tomcat9 mariadb`
2. View logs: `sudo tail -f /var/log/tomcat9/catalina.out`
3. Ensure all services are running and enabled
