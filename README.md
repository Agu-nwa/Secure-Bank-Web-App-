# Secure-Bank-Web-App-

## Project Overview
This project demonstrates the design of a secure AWS VPC for a bank-style web application using public and private subnets.

## Business Scenario
A bank needs a public customer-facing website, but its admin backend and database should not be directly accessible from the internet.

## Architecture
- VPC: 10.0.0.0/16
- Public Subnet: 10.0.1.0/24
- Private Subnet: 10.0.128.0/20
- Internet Gateway
- Public Route Table
- Private Route Table

## Architecture Diagram
```text
Internet
   |
Internet Gateway
   |
Public Route Table
   |
Public Subnet
   |
Private Subnet / Backend Area
```

## ⚙️: Update Software Packages

### Command:
```bash
[ec2-user@ip-10-0-1-112 ~]$ sudo dnf update
Last metadata expiration check: 0:21:05 ago on Tue Jun 30 00:17:58 2026.
Dependencies resolved.
Nothing to do.
Complete!
[ec2-user@ip-10-0-1-112 ~]$

## ⚙️: Upgrade and Install Web Server

### Command:
```bash
[ec2-user@ip-10-0-1-112 ~]$ sudo dnf upgrade
Last metadata expiration check: 0:22:13 ago on Tue Jun 30 00:17:58 2026.
Dependencies resolved.
Nothing to do.
Complete!
[ec2-user@ip-10-0-1-112 ~]$ sudo install -y nginx
install: invalid option -- 'y'
Try 'install --help' for more information.
[ec2-user@ip-10-0-1-112 ~]$ sudo dnf install -y nginx
Last metadata expiration check: 0:23:31 ago on Tue Jun 30 00:17:58 2026.
Dependencies resolved.
==============================================================================================================================================================
 Package                                Architecture                 Version                                          Repository                         Size
==============================================================================================================================================================
Installing:
 nginx                                  x86_64                       1:1.30.2-1.amzn2023.0.1                          amazonlinux                        34 k
Installing dependencies:
 gperftools-libs                        x86_64                       2.9.1-1.amzn2023.0.3                             amazonlinux                       308 k
 libunwind                              x86_64                       1.4.0-5.amzn2023.0.3                             amazonlinux                        66 k
 nginx-core                             x86_64                       1:1.30.2-1.amzn2023.0.1                          amazonlinux                       709 k
 nginx-filesystem                       noarch                       1:1.30.2-1.amzn2023.0.1                          amazonlinux                        10 k
 nginx-mimetypes                        noarch                       2.1.49-3.amzn2023.0.3                            amazonlinux                        21 k

Transaction Summary
==============================================================================================================================================================
Install  6 Packages

Total download size: 1.1 M
Installed size: 3.7 M
Downloading Packages:
(1/6): libunwind-1.4.0-5.amzn2023.0.3.x86_64.rpm                                                                              1.7 MB/s |  66 kB     00:00    
(2/6): nginx-1.30.2-1.amzn2023.0.1.x86_64.rpm                                                                                 823 kB/s |  34 kB     00:00    
(3/6): gperftools-libs-2.9.1-1.amzn2023.0.3.x86_64.rpm                                                                        6.2 MB/s | 308 kB     00:00    
(4/6): nginx-core-1.30.2-1.amzn2023.0.1.x86_64.rpm                                                                             25 MB/s | 709 kB     00:00    
(5/6): nginx-filesystem-1.30.2-1.amzn2023.0.1.noarch.rpm                                                                      336 kB/s |  10 kB     00:00    
(6/6): nginx-mimetypes-2.1.49-3.amzn2023.0.3.noarch.rpm                                                                       789 kB/s |  21 kB     00:00    
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                         9.7 MB/s | 1.1 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                      1/1 
  Running scriptlet: nginx-filesystem-1:1.30.2-1.amzn2023.0.1.noarch                                                                                      1/6 
  Installing       : nginx-filesystem-1:1.30.2-1.amzn2023.0.1.noarch                                                                                      1/6 
  Installing       : nginx-mimetypes-2.1.49-3.amzn2023.0.3.noarch                                                                                         2/6 
  Installing       : libunwind-1.4.0-5.amzn2023.0.3.x86_64                                                                                                3/6 
  Installing       : gperftools-libs-2.9.1-1.amzn2023.0.3.x86_64                                                                                          4/6 
  Installing       : nginx-core-1:1.30.2-1.amzn2023.0.1.x86_64                                                                                            5/6 
  Installing       : nginx-1:1.30.2-1.amzn2023.0.1.x86_64                                                                                                 6/6 
  Running scriptlet: nginx-1:1.30.2-1.amzn2023.0.1.x86_64                                                                                                 6/6 
  Verifying        : gperftools-libs-2.9.1-1.amzn2023.0.3.x86_64                                                                                          1/6 
  Verifying        : libunwind-1.4.0-5.amzn2023.0.3.x86_64                                                                                                2/6 
  Verifying        : nginx-1:1.30.2-1.amzn2023.0.1.x86_64                                                                                                 3/6 
  Verifying        : nginx-core-1:1.30.2-1.amzn2023.0.1.x86_64                                                                                            4/6 
  Verifying        : nginx-filesystem-1:1.30.2-1.amzn2023.0.1.noarch                                                                                      5/6 
  Verifying        : nginx-mimetypes-2.1.49-3.amzn2023.0.3.noarch                                                                                         6/6 

Installed:
  gperftools-libs-2.9.1-1.amzn2023.0.3.x86_64       libunwind-1.4.0-5.amzn2023.0.3.x86_64                 nginx-1:1.30.2-1.amzn2023.0.1.x86_64              
  nginx-core-1:1.30.2-1.amzn2023.0.1.x86_64         nginx-filesystem-1:1.30.2-1.amzn2023.0.1.noarch       nginx-mimetypes-2.1.49-3.amzn2023.0.3.noarch      

Complete!
[ec2-user@ip-10-0-1-112 ~]$


## ⚙️: Creates a simple homepage.

### Command:
```bash
[ec2-user@ip-10-0-1-112 ~]$ sudo echo "<h1>Secure Bank Web App</h1><p>Public web server running inside Bank VPC.</p>" > /var/www/html/index.html
-bash: /var/www/html/index.html: Permission denied
[ec2-user@ip-10-0-1-112 ~]$ echo "<h1>Secure Bank Web App</h1><p>Public web server running inside Bank VPC.</p>" | sudo tee /var/www/html/index.html
<h1>Secure Bank Web App</h1><p>Public web server running inside Bank VPC.</p>
[ec2-user@ip-10-0-1-112 ~]$ systemctl enable httpd
Failed to enable unit: Access denied
[ec2-user@ip-10-0-1-112 ~]$ sudo systemctl enable httpd
[ec2-user@ip-10-0-1-112 ~]$ sudo systemctl start httpd
[ec2-user@ip-10-0-1-112 ~]$ 
