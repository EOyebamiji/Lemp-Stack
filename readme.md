## WEB STACK IMPLEMENTATION (LEMP STACK) USING TERRAFORM
LEMP stack is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x) web server, M for MySQL database, and P for PHP scripting language.

This project uses terraform to provision the infrastrcuture required for this project.

### Requirements for this project are:
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

Steps one(1) to three will be achieved using the provisioner module within terraform. Hence, this write up will be focused on step five(5) to seven(7).

## Prequisite Step
- Clone the repo
- Open the main.tf file (Make adjustment where necessary e.g. Key Pair Name, Tags etc)
- Open your preferred cli (My prefered cli tool for windows is Git Bash)
- export your aws profile (If you have multiple profiles like)
```
export AWS_PROFILE="profile name"
```
- Initialise the project - Terraform initializes the working directory containing the configuration file and install the necessary plugins 
```
terraform init
```
![terraform-init](assets/terraform%20init.PNG)

- Plan the project - This provodes a summary of the changes about to be effected into your AWS account
```
terraform plan
```
![terraform-plan](assets/terraform%20plan.PNG)

- After you confirm the planned changes, apply the configuration into your account
```
terraform apply -auto-approve
```
![terraform-apply](assets/terraform%20apply.PNG)

Once the above step is completed, terraform runs the script and provides an output of the public and private ip address of the newly created EC2 instance running an Ubuntu Operating System.
Next step is to ssh into the ec2 instance using the public ip from the output and the private key that will now be downloaded to your working directory.
```
ssh -i "name of your key pair" "user"@"your public ip address"
ssh ubuntu@"public ip address"
```
![SSH-EC2](assets/ssh%20to%20ubuntu.PNG)

Now you have remotely connected into your instance, you can confirm the installation of PHP, MySql and Nginx which was done by our terraform script.
- Open your prefered browser and input the below command to confirm the successful installation of nginx.
```
"your public ip":80
```
![nginx](assets/nginx%20default%20portal.PNG)

Once you have done that, now let's progress to step four (4).

### CONFIGURING NGINX TO USE PHP PROCESSOR


