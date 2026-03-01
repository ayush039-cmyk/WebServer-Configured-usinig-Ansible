# E-Commerce Infrastructure Automation with Ansible
This repository contains the Ansible configuration used to automate the deployment of a high-performance, secure web and database environment on CentOS. It replaces manual setup with a repeatable "Infrastructure as Code" approach.

## Project Overview
The goal was to deploy a two-tier application stack:

Database Tier: A dedicated MariaDB server for data persistence.

Web Tier: An Apache (HTTPD) server with PHP-MySQL integration to serve the web application.

# Configuration Details
## 1. Global Dependencies
The playbook first ensures all managed nodes have the necessary backend tools for secure operations:

Installed python3-libselinux and python3-libsemanage for SELinux management.

Enabled firewalld across all systems to manage network security.

## 2. Database Server (MariaDB)
Managed under the centos_db host group:

Conflict Resolution: Automatically stops and disables Nginx to prevent port overlaps.

Installation: Installs the MariaDB server stack and PyMySQL for Python integration.

Initialization: Deploys a custom my.cnf and imports the database schema via a shell-executed SQL script (db-load-script.sql).

Network: Configures the firewall to allow traffic on port 3306.

## 3. Web Server (Apache & PHP)
Managed under the centos-s-1vcpu-1gb-blr1-01 host:

Service Cleanup: Ensures Port 80 is free by stopping Nginx.

Stack Setup: Installs httpd, php, and php-mysqlnd for database communication.

App Logic: * Uses replace to modify httpd.conf, setting index.php as the default directory index.

Clones the application source code from a remote Git repository into /var/www/html/.

Deploys a custom index.php for database connectivity testing.

Network: Configures the firewall to allow public HTTP traffic.

## How to Use
Prepare Inventory: Add your server details to inventory.txt.

Execute Playbook:

ansible-playbook -i inventory.txt playbook.yaml -e "repository=<your_repo_url>"
