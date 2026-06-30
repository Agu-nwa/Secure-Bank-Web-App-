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

````bash
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

## ⚙️: Verify

###: Command
```bash
[ec2-user@ip-10-0-1-112 ~]$ sudo systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: active (running) since Tue 2026-06-30 00:18:05 UTC; 43min ago
       Docs: man:httpd.service(8)
   Main PID: 3786 (httpd)
     Status: "Total requests: 2; Idle/Busy workers 100/0;Requests/sec: 0.000772; Bytes served/sec:   0 B/sec"
      Tasks: 230 (limit: 1059)
     Memory: 17.1M
        CPU: 2.873s
     CGroup: /system.slice/httpd.service
             ├─ 3786 /usr/sbin/httpd -DFOREGROUND
             ├─ 3859 /usr/sbin/httpd -DFOREGROUND
             ├─ 3863 /usr/sbin/httpd -DFOREGROUND
             ├─ 3864 /usr/sbin/httpd -DFOREGROUND
             ├─ 3866 /usr/sbin/httpd -DFOREGROUND
             └─26038 /usr/sbin/httpd -DFOREGROUND

Jun 30 00:18:05 ip-10-0-1-112.eu-north-1.compute.internal systemd[1]: Starting httpd.service - The Apache HTTP Server...
Jun 30 00:18:05 ip-10-0-1-112.eu-north-1.compute.internal systemd[1]: Started httpd.service - The Apache HTTP Server.
Jun 30 00:18:05 ip-10-0-1-112.eu-north-1.compute.internal httpd[3786]: Server configured, listening on: port 80
[ec2-user@ip-10-0-1-112 ~]$

## ⚙️: Copy the Private Key to the Public EC2 Instance

### Command:
```bash
scp -i ~/Downloads/pro.pem ~/.ssh/bank.pem ec2-user@51.20.127.173:/home/ec2-user/bank.pem
bank.pem                                                                                                                                                                  100% 1678    11.3KB/s   00:00
charles@Dev ~ %


## ⚙️: SSH From Public EC2 to Private EC2

###: Command
```bash
[ec2-user@ip-10-0-1-112 ~]$ ssh -i pro.pem ec2-user@10.0.137.215
The authenticity of host '10.0.137.215 (10.0.137.215)' can't be established.
ED25519 key fingerprint is SHA256:curxrrrxiCw+aRVpgLc8mkxCvzvnLO+aCH+13QqkS+s.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.137.215' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip-10-0-137-215 ~]$

## ⚙️: Private EC2 Internet Access Test

### Command
```bash

[ec2-user@ip-10-0-137-215 ~]$ sudo dnf install -y httpd
Amazon Linux 2023 repository                                    [                                 ===                       ] ---  B/s |   0  B     --:-- ETA




## 📂: Create NAT Gateway in Public Subnet

### Command
```tsx
Create a NAT Gateway in the public subnet
Allocate or attach an Elastic IP to the NAT Gateway
Wait for the NAT Gateway status to become Available
Navigate to the private route table
Edit routes and add a new route
Set destination to 0.0.0.0/0
Set target to the NAT Gateway

and update the private route table like this:

Private Route Table

10.0.0.0/16 → local
0.0.0.0/0   → NAT Gateway

This would allow the private EC2 instance to install packages and access external services while still remaining private.
````

## ⚙️: Install Apache

### Command:

````bash
sudo dnf install -y httpd
Amazon Linux 2023 repository                                                                                                   69 MB/s |  69 MB     00:00
Amazon Linux 2023 Kernel Livepatch repository                                                                                 421 kB/s |  55 kB     00:00
Dependencies resolved.
==============================================================================================================================================================
 Package                                   Architecture                 Version                                       Repository                         Size
==============================================================================================================================================================
Installing:
 httpd                                     x86_64                       2.4.68-1.amzn2023.0.1                         amazonlinux                        46 k
Installing dependencies:
 apr                                       x86_64                       1.7.5-1.amzn2023.0.4                          amazonlinux                       129 k
 apr-util                                  x86_64                       1.6.3-1.amzn2023.0.2                          amazonlinux                        97 k
 apr-util-lmdb                             x86_64                       1.6.3-1.amzn2023.0.2                          amazonlinux                        13 k
 generic-logos-httpd                       noarch                       18.0.0-12.amzn2023.0.3                        amazonlinux                        19 k
 httpd-core                                x86_64                       2.4.68-1.amzn2023.0.1                         amazonlinux                       1.4 M
 httpd-filesystem                          noarch                       2.4.68-1.amzn2023.0.1                         amazonlinux                        12 k
 httpd-tools                               x86_64                       2.4.68-1.amzn2023.0.1                         amazonlinux                        80 k
 libbrotli                                 x86_64                       1.0.9-4.amzn2023.0.2                          amazonlinux                       315 k
 mailcap                                   noarch                       2.1.49-3.amzn2023.0.3                         amazonlinux                        33 k
Installing weak dependencies:
 apr-util-openssl                          x86_64                       1.6.3-1.amzn2023.0.2                          amazonlinux                        15 k
 mod_http2                                 x86_64                       2.0.42-1.amzn2023.0.1                         amazonlinux                       167 k
 mod_lua                                   x86_64                       2.4.68-1.amzn2023.0.1                         amazonlinux                        59 k

Transaction Summary
==============================================================================================================================================================
Install  13 Packages

Total download size: 2.4 M
Installed size: 7.0 M
Downloading Packages:
(1/13): apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64.rpm                                                                         302 kB/s |  13 kB     00:00
(2/13): apr-1.7.5-1.amzn2023.0.4.x86_64.rpm                                                                                   2.4 MB/s | 129 kB     00:00
(3/13): apr-util-1.6.3-1.amzn2023.0.2.x86_64.rpm                                                                              1.7 MB/s |  97 kB     00:00
(4/13): apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64.rpm                                                                      611 kB/s |  15 kB     00:00
(5/13): generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch.rpm                                                                 673 kB/s |  19 kB     00:00
(6/13): httpd-2.4.68-1.amzn2023.0.1.x86_64.rpm                                                                                1.5 MB/s |  46 kB     00:00
(7/13): httpd-core-2.4.68-1.amzn2023.0.1.x86_64.rpm                                                                            34 MB/s | 1.4 MB     00:00
(8/13): httpd-filesystem-2.4.68-1.amzn2023.0.1.noarch.rpm                                                                     397 kB/s |  12 kB     00:00
(9/13): httpd-tools-2.4.68-1.amzn2023.0.1.x86_64.rpm                                                                          2.5 MB/s |  80 kB     00:00
(10/13): mailcap-2.1.49-3.amzn2023.0.3.noarch.rpm                                                                             1.1 MB/s |  33 kB     00:00
(11/13): libbrotli-1.0.9-4.amzn2023.0.2.x86_64.rpm                                                                            7.9 MB/s | 315 kB     00:00
(12/13): mod_http2-2.0.42-1.amzn2023.0.1.x86_64.rpm                                                                           4.5 MB/s | 167 kB     00:00
(13/13): mod_lua-2.4.68-1.amzn2023.0.1.x86_64.rpm                                                                             2.0 MB/s |  59 kB     00:00
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                          11 MB/s | 2.4 MB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                      1/1
  Installing       : apr-1.7.5-1.amzn2023.0.4.x86_64                                                                                                     1/13
  Installing       : apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64                                                                                           2/13
  Installing       : apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64                                                                                        3/13
  Installing       : apr-util-1.6.3-1.amzn2023.0.2.x86_64                                                                                                4/13
  Installing       : mailcap-2.1.49-3.amzn2023.0.3.noarch                                                                                                5/13
  Installing       : httpd-tools-2.4.68-1.amzn2023.0.1.x86_64                                                                                            6/13
  Installing       : libbrotli-1.0.9-4.amzn2023.0.2.x86_64                                                                                               7/13
  Running scriptlet: httpd-filesystem-2.4.68-1.amzn2023.0.1.noarch                                                                                       8/13
  Installing       : httpd-filesystem-2.4.68-1.amzn2023.0.1.noarch                                                                                       8/13
  Installing       : httpd-core-2.4.68-1.amzn2023.0.1.x86_64                                                                                             9/13
  Installing       : mod_http2-2.0.42-1.amzn2023.0.1.x86_64                                                                                             10/13
  Installing       : mod_lua-2.4.68-1.amzn2023.0.1.x86_64                                                                                               11/13
  Installing       : generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch                                                                                  12/13
  Installing       : httpd-2.4.68-1.amzn2023.0.1.x86_64                                                                                                 13/13
  Running scriptlet: httpd-2.4.68-1.amzn2023.0.1.x86_64                                                                                                 13/13
  Verifying        : apr-1.7.5-1.amzn2023.0.4.x86_64                                                                                                     1/13
  Verifying        : apr-util-1.6.3-1.amzn2023.0.2.x86_64                                                                                                2/13
  Verifying        : apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64                                                                                           3/13
  Verifying        : apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64                                                                                        4/13
  Verifying        : generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch                                                                                   5/13
  Verifying        : httpd-2.4.68-1.amzn2023.0.1.x86_64                                                                                                  6/13
  Verifying        : httpd-core-2.4.68-1.amzn2023.0.1.x86_64                                                                                             7/13
  Verifying        : httpd-filesystem-2.4.68-1.amzn2023.0.1.noarch                                                                                       8/13
  Verifying        : httpd-tools-2.4.68-1.amzn2023.0.1.x86_64                                                                                            9/13
  Verifying        : libbrotli-1.0.9-4.amzn2023.0.2.x86_64                                                                                              10/13
  Verifying        : mailcap-2.1.49-3.amzn2023.0.3.noarch                                                                                               11/13
  Verifying        : mod_http2-2.0.42-1.amzn2023.0.1.x86_64                                                                                             12/13
  Verifying        : mod_lua-2.4.68-1.amzn2023.0.1.x86_64                                                                                               13/13

Installed:
  apr-1.7.5-1.amzn2023.0.4.x86_64                    apr-util-1.6.3-1.amzn2023.0.2.x86_64                    apr-util-lmdb-1.6.3-1.amzn2023.0.2.x86_64
  apr-util-openssl-1.6.3-1.amzn2023.0.2.x86_64       generic-logos-httpd-18.0.0-12.amzn2023.0.3.noarch       httpd-2.4.68-1.amzn2023.0.1.x86_64
  httpd-core-2.4.68-1.amzn2023.0.1.x86_64            httpd-filesystem-2.4.68-1.amzn2023.0.1.noarch           httpd-tools-2.4.68-1.amzn2023.0.1.x86_64
  libbrotli-1.0.9-4.amzn2023.0.2.x86_64              mailcap-2.1.49-3.amzn2023.0.3.noarch                    mod_http2-2.0.42-1.amzn2023.0.1.x86_64
  mod_lua-2.4.68-1.amzn2023.0.1.x86_64

Complete!
[ec2-user@ip-10-0-137-215 ~]$

## 📂: Start, Exit Private EC2 and Test

### Command:
```bash
sudo systemctl enable httpd
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.
[ec2-user@ip-10-0-137-215 ~]$ sudo systemctl start httpd
[ec2-user@ip-10-0-137-215 ~]$ ls
backend-status.txt
[ec2-user@ip-10-0-137-215 ~]$ exit
logout
Connection to 10.0.137.215 closed.
[ec2-user@ip-10-0-1-112 ~]$ curl http://10.0.137.215
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title>It works! Apache httpd</title>
</head>
<body>
<p>It works!</p>
</body>
</html>
[ec2-user@ip-10-0-1-112 ~]$
````
[ec2-user@ip-10-0-1-112 ~]$ 
