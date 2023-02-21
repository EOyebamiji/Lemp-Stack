## WEB STACK IMPLEMENTATION (LEMP STACK) USING TERRAFORM
LEMP stack is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x) web server, M for MySQL database, and P for PHP scripting language.

This project uses terraform to provision the infrastrcuture required for this project.

## Requirements for this project are:
- An AWS account
- A Laptop
- Good Internet Connection

Every other requirements and configuration for this project is provisioned using terraform - an IaC tool which enables provides automation and code reuseablilty.

We intend to acheieve the below listed activities/tasks:
    -   INSTALLING THE NGINX WEB SERVER
    -   INSTALLING MYSQL
    -   INSTALLING PHP
    -   CONFIGURING NGINX TO USE PHP PROCESSOR
    -   TESTING PHP WITH NGINX
    -   RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
    -   TESTING OUR NEW PROJECT
Steps one(1) to three(3) will be achieved using the provisioner module within terraform. Hence, this write up will be focused on step five(5) to seven(7).