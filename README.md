# Guacamole-Setup-On-Debian 12-Server
A comprehensive installation guide for Apache Guacamole on Debian 12 Server, including MySQL authentication setup, Tomcat configuration, and security best practices. Perfect for sys admins and developers setting up remote desktop access.

# Apache Guacamole Installation Guide for Debian

This guide provides step-by-step instructions for installing Apache Guacamole on Debian. Apache Guacamole is a clientless remote desktop gateway that supports standard protocols like VNC, RDP, and SSH.

## Prerequisites

- Debian operating system
- Root or sudo privileges
- Terminal access
- Internet connection

## Installation Steps

### 1. Install Dependencies

First, update your package list and install required dependencies:

```bash
# Update package list
sudo apt-get update

# Install required packages
sudo apt-get install libcairo2-dev libjpeg62-turbo-dev libpng-dev libossp-uuid-dev \
libavcodec-dev libavutil-dev libswscale-dev libfreerdp-dev libpango1.0-dev \
libssh2-1-dev libtelnet-dev libvncserver-dev libpulse-dev libssl-dev \
libvorbis-dev libwebp-dev
```

### 2. Download and Build Guacamole Server

```bash
# Create and navigate to Guacamole directory
mkdir Guacamole
cd /Guacamole

# Download Guacamole server
wget https://downloads.apache.org/guacamole/1.5.5/source/guacamole-server-1.5.5.tar.gz

# Extract and build
tar -xzf guacamole-server-1.5.5.tar.gz
cd guacamole-server-1.5.5.tar.gz
./configure --with-init-dir=/etc/init.d
sudo make
sudo make install
sudo ldconfig
```

### 3. Configure and Start guacd Service

```bash
# Start and enable guacd service
sudo systemctl daemon-reload
sudo systemctl start guacd
sudo systemctl enable guacd
sudo systemctl status guacd
```

### 4. Install Tomcat 9

Since Guacamole is not compatible with Tomcat 10 (default in Debian 12), we need to install Tomcat 9:

```bash
# Add Debian 11 (Bullseye) repository
# Create new source file
sudo nano /etc/apt/sources.list.d/bullseye.list

# Add this line to the file:
# deb http://deb.debian.org/debian/ bullseye main

# Update and install Tomcat 9
sudo apt-get update
sudo apt-get install tomcat9 tomcat9-admin tomcat9-common tomcat9-user
```

### 5. Install Guacamole Web Application

```bash
# Download and deploy web application
cd /Guacamole
wget https://downloads.apache.org/guacamole/1.5.5/binary/guacamole-1.5.5.war
sudo mv guacamole-1.5.5.war /var/lib/tomcat9/webapps/guacamole.war

# Create configuration directories
sudo mkdir -p /etc/guacamole/{extensions,lib}
```

### 6. Set Up MariaDB

```bash
# Install MariaDB
sudo apt-get install mariadb-server

# Secure MariaDB installation
sudo mysql_secure_installation

# Log in to MariaDB
sudo mysql -u root -p

# Create database and user (in MySQL prompt)
CREATE DATABASE guacadb;
CREATE USER 'guacadmin'@'localhost' IDENTIFIED BY 'Your_Strong_Password_Here'; # Replace with your password
GRANT SELECT,INSERT,UPDATE,DELETE ON guacadb.* TO 'guacadmin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 7. Install MySQL Extension and Connector

```bash
# Download and install JDBC extension
cd /Guacamole
wget https://downloads.apache.org/guacamole/1.5.5/binary/guacamole-auth-jdbc-1.5.5.tar.gz
tar -xzf guacamole-auth-jdbc-1.5.5.tar.gz
sudo mv guacamole-auth-jdbc-1.5.5/mysql/guacamole-auth-jdbc-mysql-1.5.5.jar /etc/guacamole/extensions/

# Download and install MySQL Connector
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-9.1.0.tar.gz
tar -xzf mysql-connector-j-9.1.0.tar.gz
sudo cp mysql-connector-j-9.1.0/mysql-connector-j-9.1.0.jar /etc/guacamole/lib/

# Initialize database schema
cd guacamole-auth-jdbc-1.5.5/mysql/schema/
cat *.sql | sudo mysql -u root -p guacadb
```

### 8. Configure Guacamole

Create and edit the properties file:

```bash
sudo nano /etc/guacamole/guacamole.properties
```

Add the following content (replace with your password):
```properties
mysql-hostname: 127.0.0.1
mysql-port: 3306
mysql-database: guacadb
mysql-username: guacadmin
mysql-password: Your_Strong_Password_Here  # Replace with the same password used in Step 6
```

### 9. Configure guacd

Create and edit the configuration file:

```bash
sudo nano /etc/guacamole/guacd.conf
```

Add the following content:
```conf
[server]
bind_host = 0.0.0.0
bind_port = 4822
```

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
