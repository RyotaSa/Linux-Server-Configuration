# Linux-Server-Configuration

**Description**

This project is to aim to deploy a flask application on AWS LightSail and shows steps from creating your instance to your application in Ubuntu.
The application under instructions is from [github](https://github.com/RyotaSa/Item-Catalog)

IP Address: 13.115.1.96
URL: http://13.115.1.96/  
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
1. `sudo dpkg-reconfigure tzdata`  
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
Then `sudo service apache2 restart`  

**Set up Flask file**  
Create FlaskApp file directory on the `/var/www/`.  
Clone your app project on this directory: `sudo git clone https://github.com/~ FlaskApp`  

**Set up WSGI file**  
On the FlaskApp directory, create flaskapp.wsgi file.  
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'secret_key'
```

**Installing a Virtual Env, Git and Flask etc**  
1. Installing virtualenv: `sudo apt-get install python-pip` `sudo pip install virtualenv`  
2. Install: `sudo apt install python-psycopg2`  
3. Type `cd /var/www/FlaskApp/FlaskApp`  
4. Create an instance of the virtual environment and activate it: `sudo virtualenv venv` `source venv/bin/activate`  
5. Install: Git: `sudo apt-get install git`  
Flask: `sudo pip install Flask`  
`sudo pip install oauth2client`  
`sudo pip install sqlalchemy`  
`sudo pip install bleach `  
`sudo pip install httplib2`  
`sudo pip install requests`  

**Enable a new Virtual host**  
1. Create the FlaskApp.conf on `/etc/apache2/sites-available/`  
2. Write: 　
```
<VirtualHost *:80>
      ServerName 13.115.1.96
      ServerAdmin benz.slr.mclaren@gmail.com
      WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
      <Directory /var/www/FlaskApp/FlaskApp/>
          Order allow,deny
          Allow from all
      </Directory>
      Alias /static /var/www/FlaskApp/FlaskApp/static
      <Directory /var/www/FlaskApp/FlaskApp/static/>
          Order allow,deny
          Allow from all
      </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```
  
3. Restart the apache service: `service apache2 restart`  
4. Enable new virtual host:`sudo a2ensite FlaskApp`  

**Installing Postgres and setup**  
1. `sudo apt-get install postgresql`  
2. Login as user postgres and run psql command.  
3. Create the user name catalog:`create user catalog with password 123456;`  
4. `alter user catalog createdb;`  
5. Create the database:`create database catalog owner catalog;`  

**Edit files for our new database connection**  
1. Rewrite the following files: `__init__.py` `database_setup.py` `data.py`  
`engine = create_engine('postgresql://catalog:123456@localhost/catalog')`  
2. Rewite the `__init__.py` file:  
`CLIENT_ID = json.loads(open('/var/www/FlaskApp/FlaskApp/client_secret.json', 'r').read())['web']['client_id']`  
`app_id = json.loads(open('/var/www/FlaskApp/FlaskApp/fb_client_secrets.json', 'r').read())['web']['app_id']`  

**Edit the json file and OAuth login**  
1. In Facebook App client OAuth Settings, you should add `http://13.115.1.96/`, `http://13.115.1.96/catalog`, `http://13.115.1.96/login`, `http://ec2-13-115-1-96.ap-northeast-1.compute.amazonaws.com`,`http://ec2-13-115-1-96.ap-northeast-1.compute.amazonaws.com/login`,`http://ec2-13-115-1-96.ap-northeast-1.compute.amazonaws.com/catalog` in Valid OAuth redirect URIs.  

**Restart Apache**  
`sudo service apache2 restart`  

Visit site at <http://13.115.1.96/>

**Bibiliography**  
https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html  
http://wsgi.readthedocs.io/en/latest/  
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps  
https://help.ubuntu.com/community/PostgreSQL  
[Steven Wooding](https://github.com/SteveWooding/fullstack-nanodegree-linux-server-config)  
[harushimo](https://github.com/harushimo/linux-server-configuration)

