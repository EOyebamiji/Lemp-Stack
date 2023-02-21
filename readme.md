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

Now you have remotely connected into your instance, you need to carry out a few configurations.
# Nginx
Type in the following commands to confirm the status of your nginx server
```
sudo systemctl status nginx
curl http://localhost:80
```
![nginx](assets/systemctl%20enginx.PNG)

Now, the nginx server is accessible via your publiic ip address, Open your prefered browser and type in
```
"your public ip address":80
```
![nginx](assets/nginx%20default%20portal.PNG)

We can now confirm that the Nginx server is Up.

# MySql
Type in the below command to login to the mysql enviroment and test your installation
```
sudo mysql
```
![sudo-sql](assets/sudo%20mysql.PNG)

In the Mysql enviroment, One of the first thing to do after installing mysql server is to run a security script to remove insecure default settings and lock down access to our database system. Use the following command. You can specify your prefered password in the password phase
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
![mysql-pass](assets/alter%20mysql.PNG)

Now exit the Mysql enviroment by typing "exit". The next step is to setup an interactive scripting plugin to validate our passwords.
```
sudo mysql_secure_installation
```
![mysql-secure-installation](assets/mqsql%20secure%20installation.PNG)

The above command will respond with an interactive session where you have to decide if you want to retain the password your created earlier or change to another one. Once done, progress to the next step which is to test login access to your mysql using your password.
```
sudo mysql -p
```
![mysql-passsword-test](assets/mysql%20password%20test.PNG)