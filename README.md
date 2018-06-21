# linux-server-configuration
- In this project we have to deploy item catalog project to an instance using a Linux server instance called Amazon Lightsail.
### Procedure for this project:

#### Step:1

- We have to create an amazon account.

- We have to create an instance in lightsail.aws.amazon.com.

- Download putty.exe,puttygen.exe in puti.org.

- By opening putty.exe provide the static IP of instance as hostname.

- select file downloaded from shh-keys eg: LightsailDefaultPrivateKey-ap-south-1.pem and then click ok and rename it as test.pem. Then save private key as testprivatekey.ppk.

- After loading the private key, a new terminal will open and then click no.

- Now you will be connected to SSH command prompt with user ubuntu.

- Download the static IP which we have created in the lightsail.

- Open putty enter your ip and add private key address.

- Use the following command to connect to grader:

  ssh -i path/to/privatekey -p 2200 grader@ip address
  
- Create the grader user:
  
  ubuntu@ip-172-26-0-113:~$ sudo apt update
  
  sudo adduser grader
  
- Now follow the below steps:

  sudo nano /etc/sudoers
  
- Granting the sudo permission to grader

  grader  ALL=(ALL:ALL) ALL
  
- Creating a ssh key pair for grader:

- Open new command prompt and the type:

  ssh-keygen
  
- This will generate public and private ssh keys which is saved to .ssh folder.

- Changing user from ubuntu to grader:

  sudo su - grader
  
- Create a directory .ssh and new file authorized_keys in that directory.

  mkdir .ssh
  
  sudo nano .ssh/authorized_keys
  
- Permissions:

  chmod 700 .ssh
  
  chmod 644 .ssh/authorized_keys
  
- 700 will give read write and execute permission to user.

- 644 prevent other user from writting in to file.

- Restarting the SSH server by typing the following:

  sudo service ssh restart
  
  ssh -i .ssh/id_rsa grader@IPaddress
  
- Changing the port from 22 to 2200:

  sudo nano /etc/ssh/sshd_config
  
- Disabling SSH login as root:

  sudo nano /etc/ssh/sshd_config
  
-  make change PermitRootLogin no

- Uncomplicated firewall(ufw) enbling:

  sudo ufw allow 2200/tcp
  
  sudo ufw allow 80/tcp
  
  sudo ufw allow 123/udp
  
  sudo ufw enable
  
- For viewing ports type:

  sudo ufw status
  
- Changing time Zone:

  sudo dpkg-reconfigure tzdata
  
#### Step:2

##### Software requirements:

- AWS account with lightsail service activated.

- Python Pip

- Postgres

- httplib2

- SQLAlchemy

- Apache2

- Flask

- Virtualenv

- Requests

- Oauth2client

##### Software installation process:

  sudo apt-get install apache2		

  sudo apt-get install python-setuptools libapache2-mod-wsgi

  sudo a2enmod wsgi

  cd /var/www

  sudo mkdir FlaskApp

  sudo apt-get install git

  sudo apt-get install python-pip virtualenv

  sudo virtualenv venv

  sudo pip install Flask

  sudo pip install postgresql oauth2client httplib2 requests psycopg2

  cd /var/www/FlaskApp
 
  sudo rm -r FlaskApp
  
- Rename your main python file in your repository into init.py

  sudo mv project.py __init__.py
  
- Clone your github repository into that directory.

  sudo git clone 'https://github.com/username/filename.git'
  
- By typing ls check whether your files are added or not.

- Changing database in both database_setup.py and init.py

  sudo nano database_setup.py
  
- Open

  https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
  
- Replacing database connection in init.py and database_setup.py

  engine = create_engine('postgresql://catalog:password@localhost/catalog')
  
- Error : While accesssing the client_secrets.json file

- PROJECT_ROOT = os.path.realpath(os.path.dirname(file)) json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
  CLIENT_ID = json.load(open(json_url))['web']['client_id']
  
- Replace client_secrets.json with json_url in scriptfile

  sudo apt-get install postgresql

  sudo su - postgres

  psql
  
- Creating user

  CREATE USER catalog WITH PASSWORD 'password';
  
- Alter user

  ALTER USER catalog CREATEDB;
  
- Database creation

  CREATE DATABASE catalog WITH OWNER catalog;
   
- Switching to database catalog

  \c catalog  
  
- revoke

  REVOKE ALL ON SCHEMA public FROM public;
  
- Granting

   GRANT ALL ON SCHEMA public TO catalog;
   
- now type \q and then exit.

- Change the database connection in both database_setup.py and init.py as

  engine = create_engine('postgresql://catalog:password@localhost/catalog')
  
#### Step:3

- Configure and Enable a New Virtual Host, then type the following code in the shell

<VirtualHost *:80> ServerName mywebsite.com ServerAdmin admin@mywebsite.com WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi <Directory /var/www/FlaskApp/FlaskApp/> Order allow,deny Allow from all Alias /static /var/www/FlaskApp/FlaskApp/static <Directory /var/www/FlaskApp/FlaskApp/static/> Order allow,deny Allow from all ErrorLog ${APACHE_LOG_DIR}/error.log LogLevel warn CustomLog ${APACHE_LOG_DIR}/access.log combined.
  
  sudo a2ensite FlaskApp

  sudo a2dissite 000-default.conf
  
- .wsgi file

  sudo nano flaskapp.wsgi
  
- Add the below code.

#!/usr/bin/python import sys import logging logging.basicConfig(stream=sys.stderr) sys.path.insert(0,"/var/www/FlaskApp/") from FlaskApp import app as application application.secret_key = 'Add your secret key'

- press ctrl+x and y

#### Step:4

- Go to google console and change homeurl as

  http://ipaddress.xip.io\callback

  http://ipaddress.xip.io\gconnect

  http://ipaddress.xip.io\login
  
#### Step:5

  sudo service apache2 restart
  
- By typing your IP address in a web browser, we will be redirected into our project.

- For error checking:

  sudo nano /var/log/apache2/error.log
  
- My server Details
  
  Server static IP Address 13.127.156.77
  
- Grader key

  -----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAqPtrZ57kGJiwec/GKzMzDF8SpL/xKFZhiZjUbJOXiRZp/OG3
HtOicT6z/wTZKWb99T6kMzhd16N8HfAdw3CmPct9qWm2I+bUsPexld1zS0jxDxfk
2GflVVGMqpjdOZ7KVSOeFxxuhJQYvze7HDYgslC6/vDDbf9iwLH+zPK/wxGmgp9c
zaJbaf9XGbEZxdDjez9w92HKWkObPdxhPuO4JiueawjglHwBEp7jX5A+JgSaQOUc
HZNCnJHls86IQ33FrmUTEuxEGANTKLSzfFMyhAh/JnPb2TPi8CNTqIUR82VNFhLt
dWXdrCwIZ8pRMAydsMVTe7eQ6LWGNUWNCFzsEQIDAQABAoIBAGeGuROfxdDH4v6N
67PVx0WiDZL/wShcW59oEWR/u8wR/lcelgj90bydlLr9Zmo2HgqdGQ4ET4HoOAdD
b4ioQEEFpXQgPqWaKt5MsfDF3nfWNVFw6xQl+tudfZea1ZYSkZU9oAI6uf7hmJO1
+h1bkAaO7TF5odGHMCIsKpW3dwJzk+s8V2kXmb75Xz9AUvq6pLZot0iXZyMhWZLs
nYtLp4wzi0ICnHW11Qs5KemUtUzNAecC6U8CjsWzQSv/8OFfZvUQwrlyh4+S5ps+
VcXJJLKEP6JeOdeinx6k1UoQgWf/QHyo8fvyovDYHcEP564SQwiAXI9B68Jh3Wrn
eHrLwgECgYEA1OQ0nBvZBR0YTcqQcCMSGDz/nq3oGQIszBi6fHkJD5Wk5pZ4ZBBH
m+CAUUzvYx+TCQRJkYtPj4wA1CCiHIsJPweKPwMngK/Jbo62qapziuK8Uv+udgmG
Mxgc035Uk6ezHjDJDwEXL5X+YMEb14HqNIGas5RL0c6cZtwdlil0VPECgYEAyzMO
swPtce4V0/HoaAVLSOSvEhUmbME7QYCC0u1dUKXT4G3A24qPQqAIY0tQEsKj1+uS
GLQ/v26wIGqb78OZN6UG1h0Cgf1K3vU51OygqfWenL8XzIP/5LE3JJ6t0m+GScby
+UJpZvqsFfW+yV3jJ5PSIFY3/9QWIIXdZtl5iSECgYBNpm8KAZ5GnzYeKaRFQoV3
EciquAPQG1r7lolunTcQ2CQtdvSyir2TvW8QOF+YaAvZXhb3XzjFwusKdFyszImy
0605Do16AqQWDzfQ6rr6DXljTJ71rsOkH0dkXM+8i45plKHBN0Sdtrfx0n21PU2P
mTY0CgMdo56oZeDkxHoKAQKBgFlpwCAZrFQjtcsreV68ZaJPrpHAaMYWSSLLj8WM
2TDxp0fsQ57XW66viMFYlIWMzFfousLQHfT4mdvJzZA1e6g2n8l7vmzArj9pnOcK
sK/Y+tBybeB6fRF+wYsFn+snU+oG90ejZ4n+59ZJ6oflHl19+EpuZfnWs13gScpG
C+rBAoGACOJO338zQ75pGnBX6mxE9NFA/+mOYNsVca5QKQ8eKyNtxDqoarpbh4e9
5JaFw2qMXQo4VoOar9HVL7Wd7Z+FTHzwofJo+DK6Kb/Y3D5T2QRB+bty+u56YYIO
o+JV3UBGt5OMMPz0JNad2Tj9xf3A21kIBUcc8CLeSJdymiC2oXU=
-----END RSA PRIVATE KEY-----

- pub key

  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCo+2tnnuQYmLB5z8YrMzMMXxKkv/EoVmGJmNRsk5eJFmn84bce06JxPrP/BNkpZv31PqQzOF3Xo3wd8B3DcKY9y32pabYj5tSw97GV3XNLSPEPF+TYZ+VVUYyqmN05nspVI54XHG6ElBi/N7scNiCyULr+8MNt/2LAsf7M8r/DEaaCn1zNoltp/1cZsRnF0ON7P3D3YcpaQ5s93GE+47gmK55rCOCUfAESnuNfkD4mBJpA5Rwdk0KckeWzzohDfcWuZRMS7EQYA1MotLN8UzKECH8mc9vZM+LwI1OohRHzZU0WEu11Zd2sLAhnylEwDJ2wxVN7t5DotYY1RY0IXOwR sathish@Sathish
  
- Grader Password

  unix
  
- Outcome:

  CLICK HERE http://13.127.156.77.xip.io
  
- Reference:

  https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

  

  

