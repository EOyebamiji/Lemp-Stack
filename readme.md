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

You can confirm that the ec2 instance is running via the aws console (GUI) and also confirm the public ip address
![aws-gui](assets/ec2%20instance.PNG)

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

once you type the above command, input your password and type enter. Then exit mysql.

## CONFIGURING NGINX TO USE PHP PROCESSOR
Now that you can confirm all requiremnts installed and configured. We need to configure Nginx to use Php.
Now create the root web directory for your "domain"
```
sudo mkdir /var/www/LEMP
```
Next step is to change ownership of the created directory to the $USER
```
sudo chown -R $USER:$USER /var/www/LEMP
```
Once done, create and open a new configuration file in Nginx's directory using your prefered editor (Nano or Vim). I'll be using nano
```
sudo nano /etc/nginx/sites-available/LEMP
```
This is create a blank file in editor mode, copy and paste the below configurations into the created file.
```
server {
    listen 80;
    server_name LEMP www.LEMP;
    root /var/www/LEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
}
```
![nano-lemp](assets/nano%20-lemp.PNG)

![cmd-codes](assets/cmds%20php.PNG)

Once you paste the copied configuration to the created file, use Ctrl+X and Y to exit and save the configuration.

Next step is to activate your new configuration by linking it to the configuration file in the Nginx directory
```
sudo ln -s /etc/nginx/sites-available/LEMP /etc/nginx/sites-enabled/
```
Once the above is done, you need to test your configuration for syntx error 
```
sudo nginx -t
```
![test](assets/nginx%20config%20file.PNG)

Next step is to diable the default Nginx host configured and currently listening on port 80 as we need our new configuration to be accessible through port 80
```
sudo unlink /etc/nginx/sites-enabled/default
```

Your weebsite iw now active but empty, Now we need to create an index.html file and include some configuration
```
sudo echo 'Hello, My name is Emmanuel Oyebamiji, I am a DevOps Intern at SymIOT Limited, This is one of my DevOps project, Creating of a LEMP Stack using terraform. Here is my host name' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/LEMP/index.html
```
You can edit the content to what you like. 

Now our website up and running with content and is accessible via
```
"Your public ip address":80
```

![connection](assets/connection.PNG)