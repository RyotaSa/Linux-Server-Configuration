# Linux-Server-Configuration

**Description**

This project is to aim to deploy a flask application on AWS LightSail and shows steps from creating your instance to your application in Ubuntu.
The application under instructions is from [github](https://github.com/RyotaSa/Item-Catalog)

IP Address: 52.193.200.99
Port: 

## Instruction
**Prerequisite**

Have a AWS account

**Get started on Lightsail**

1. Log in to Lightsail
2. Create an instance
3. Choose an instance image, so choose OS only and Ubuntu
4. Choose an instance plan
5. Option: you can change an instance name

**Log in to Lightsail by SSH**
1. Download your private key(pem.file) from your account section. The name begins LightsailDefaultPrivateKey~ .pem.  
My pem file is LightsailDefaultPrivateKey-ap-northeast-1.pem
2. Open Git Bush on pem file location
3. Log in to the server by SSH:  
` ssh user@PublicIP -i pemfile name `  
For instance ` ssh ubuntu@52.193.200.99 -i LightsailDefaultPrivateKey-ap-northeast-1.pem `  
we change port after that.  

**Updating your server**  
Once you connect using SSH, you can see ubuntu black screen server.  

1. Do just two commands, then wait a few minutes  
` sudo apt-get update `  
` sudo apt-get upgrade`  

**Create new user and put sudo access**  
we can set new user named *grader*  
1. ` sudo adduser grader `  
pop up password prompt and you can type any password  
2. Give grader the permission to sudo.  
` sudo visudo `  
In privilege section, you can find root user has all the privilege.  
So, you should add  `grader  ALL=(ALL:ALL) ALL`  

**SSH login using key**  
1. `mkdir /home/grader/.ssh` and `cd .ssh`  
2. In your local machine, type `ssh-keygen`  
3. Once created public key, you must write it to `/home/grader/.ssh/authorized_keys`  
4. you can login like `ssh grader@13.115.1.96 -i private_keys_name`

**Timezone to UTC**  
`sudo dpkg-reconfigure tzdata`  
Choose UTC timezone.  

**Change Port number to 2200**  
On AWS Lightsail, Networking tab, you should add port 2200 with yco network.  
Type `sudo vi /etc/ssh/sshd_config`  
Change Port 2200 from Port 22.  

**Set Firewall using UFW**  
Check the status of firewall: `sudo ufw status`  
Then type `sudo ufw default deny incoming` and `sudo ufw default allow outgoing`  
`sudo ufw allow ssh`
`sudo ufw allow 2200/tcp`
`sudo ufw allow www`
`sudo ufw enable`  

Check the status:`sudo ufw status`   

**Installing Apache and mod_wsgi**  
1. `sudo apt-get install apache2`  
2. `sudo apt-get install libapache2-mod-wsgi`  

**Set WSGI file**  


**Installing Git and Flask etc**  
Git: `sudo apt-get install git`  


**Installing Postgres**  
1. `sudo apt-get install postgresql`  



**References**  
http://wsgi.readthedocs.io/en/latest/  



