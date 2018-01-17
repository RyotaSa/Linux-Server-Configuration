# Linux-Server-Configuration

**Description**

This project is to aim to deploy a flask application on AWS LightSail and shows steps from creating your instance to your application in Ubuntu.
The application under instructions is from [github](https://github.com/RyotaSa/Item-Catalog)

IP Address: 13.231.15.252
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
For instance ` ssh ubuntu@13.231.15.252 -i LightsailDefaultPrivateKey-ap-northeast-1.pem `

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




