# Installation Guide

This is the **Intheair Saas Platform** installation guide, follow the guide to have the backend up and running on your environement. 

!!! note
    We suggest you to follow the installation to the end before running the server to avoid errors.

!!! warning
    we suppose that you are in a docker or in a virtual environement, create one before following the installation if not !

## Python dependencies
All the django and python dependencies are added to the `requirement.txt`.
To install the dependencies just run 
```shell
pip install -r <yourpath>/requirements.txt
```

## Geospatial dependencies
After installing the python dependencies, we need to install some libraries and packages to use **GDAL** and **GIS** tools.
First we need to run this command to install, directly or by dependency, the required geospatial libraries:
```shell
sudo apt-get install cmake binutils libproj-dev gdal-bin
```
!!! note 
    It may be necessary to run the **ldconfig** command after installing each library. For example:
    ```shell
    sudo make install
    sudo ldconfig
    ```

We will then have to install **GEOS**, a C++ library for performing geometric operations, and is the default internal geometry representation used by GeoDjango (it’s behind the « lazy » geometries). Specifically, the C API library is called (e.g., libgeos_c.so) directly from Python using ctypes.
First, download GEOS from the GEOS website and untar the source archive:
```shell
wget https://download.osgeo.org/geos/geos-3.12.0.tar.bz2
tar xjf geos-3.12.0.tar.bz2
```
Then we have to build and install the package:
```shell
cd geos-3.12.0
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
sudo cmake --build . --target install
```
Then we will have to install **GDAL**, First download the latest GDAL release version and untar the archive:
```shell
wget https://download.osgeo.org/gdal/3.7.2/gdal-3.7.2.tar.gz
tar xzf gdal-3.7.2.tar.gz
```
Then we have to build and install the package:
```shell
cd gdal-3.7.2
mkdir build
cd build
cmake ..
cmake --build .
sudo cmake --build . --target install
```
!!! important
    If you get any errors while trying to install the GIS dependencies, try troubleshooting your environment via this link [Geodjango Troubleshooting](https://docs.djangoproject.com/fr/4.2/ref/contrib/gis/install/#troubleshooting).

## RabbitMQ
The backend uses **celery** in order to handle the async tasks, the celery library needs to connect to a messaging queue server in order to queue its tasks. To achieve that we need to install and run **RabbitMQ** a message queuing server and run it.
For installing RabbitMQ on Linux just run this script and you are good to go:
```shell
#!/bin/sh

sudo apt-get install curl gnupg apt-transport-https -y

## Team RabbitMQ's main signing key
curl -1sLf "https://keys.openpgp.org/vks/v1/by-fingerprint/0A9AF2115F4687BD29803A206B73A36E6026DFCA" | sudo gpg --dearmor | sudo tee /usr/share/keyrings/com.rabbitmq.team.gpg > /dev/null
## Community mirror of Cloudsmith: modern Erlang repository
curl -1sLf https://github.com/rabbitmq/signing-keys/releases/download/3.0/cloudsmith.rabbitmq-erlang.E495BB49CC4BBE5B.key | sudo gpg --dearmor | sudo tee /usr/share/keyrings/rabbitmq.E495BB49CC4BBE5B.gpg > /dev/null
## Community mirror of Cloudsmith: RabbitMQ repository
curl -1sLf https://github.com/rabbitmq/signing-keys/releases/download/3.0/cloudsmith.rabbitmq-server.9F4587F226208342.key | sudo gpg --dearmor | sudo tee /usr/share/keyrings/rabbitmq.9F4587F226208342.gpg > /dev/null

## Add apt repositories maintained by Team RabbitMQ
sudo tee /etc/apt/sources.list.d/rabbitmq.list <<EOF
## Provides modern Erlang/OTP releases
##
deb [signed-by=/usr/share/keyrings/rabbitmq.E495BB49CC4BBE5B.gpg] https://ppa1.novemberain.com/rabbitmq/rabbitmq-erlang/deb/ubuntu jammy main
deb-src [signed-by=/usr/share/keyrings/rabbitmq.E495BB49CC4BBE5B.gpg] https://ppa1.novemberain.com/rabbitmq/rabbitmq-erlang/deb/ubuntu jammy main

# another mirror for redundancy
deb [signed-by=/usr/share/keyrings/rabbitmq.E495BB49CC4BBE5B.gpg] https://ppa2.novemberain.com/rabbitmq/rabbitmq-erlang/deb/ubuntu jammy main
deb-src [signed-by=/usr/share/keyrings/rabbitmq.E495BB49CC4BBE5B.gpg] https://ppa2.novemberain.com/rabbitmq/rabbitmq-erlang/deb/ubuntu jammy main

## Provides RabbitMQ
##
deb [signed-by=/usr/share/keyrings/rabbitmq.9F4587F226208342.gpg] https://ppa1.novemberain.com/rabbitmq/rabbitmq-server/deb/ubuntu jammy main
deb-src [signed-by=/usr/share/keyrings/rabbitmq.9F4587F226208342.gpg] https://ppa1.novemberain.com/rabbitmq/rabbitmq-server/deb/ubuntu jammy main

# another mirror for redundancy
deb [signed-by=/usr/share/keyrings/rabbitmq.9F4587F226208342.gpg] https://ppa2.novemberain.com/rabbitmq/rabbitmq-server/deb/ubuntu jammy main
deb-src [signed-by=/usr/share/keyrings/rabbitmq.9F4587F226208342.gpg] https://ppa2.novemberain.com/rabbitmq/rabbitmq-server/deb/ubuntu jammy main
EOF

## Update package indices
sudo apt-get update -y

## Install Erlang packages
sudo apt-get install -y erlang-base \
                        erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets \
                        erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key \
                        erlang-runtime-tools erlang-snmp erlang-ssl \
                        erlang-syntax-tools erlang-tftp erlang-tools erlang-xmerl

## Install rabbitmq-server and its dependencies
sudo apt-get install rabbitmq-server -y --fix-missing
```
## Configuration
To configure our environment please refer to the **/intheair/.env** file.

### Celery
To use Celery we need to create a RabbitMQ user, a virtual host and allow that user access to that virtual host:
```shell
sudo rabbitmqctl add_user <USER> <PASSWORD>
sudo rabbitmqctl add_vhost <HOST NAME>
sudo rabbitmqctl set_user_tags <USER> <TAG>
sudo rabbitmqctl set_permissions -p <HOST NAME> <USER> ".*" ".*" ".*"
```
Substitute in appropriate values for myuser, mypassword and myvhost above.
See the RabbitMQ [access control](https://www.rabbitmq.com/access-control.html) documentation or the [Admin Guide](https://www.rabbitmq.com/admin-guide.html) for more information.

### Email server
The backend uses an emailing functionality, you can update the email section of the **.env** file to connect the backend to the **smpt** server used in the deployment
```env
EMAIL_HOST=<THE EMAIL SERVER HOST>
EMAIL_PORT=<THE EMAIL SERVER PORT>
EMAIL_HOST_USER=<THE EMAIL SERVER USER>
EMAIL_HOST_PASSWORD=<THE EMAIL SERVER APP PASSWORD>
CONTACT_EMAIL=<THE CONTACT EMAIL ADDRESS>
DEFAULT_FROM_USER=<DEFAULT EXPEDITOR EMAIL (WILL PROBABLY BE THE CONTACT EMAIL ADDRESS)>
```

### Database
The database used in our backend is a **postgresql** database, it needs to support **GIS** and **raster** functionalities. Make sure to install **postgis** and **raster** libraries in the database created (you can contact your database administrator to add the plugins).
After creating and configuring the database you need to configure the backend to connect to it. Fill the variables in the **.env** file to allow the connection:
```env
DB_HOST=<DATABASE URL/HOST>
DB_USERNAME=<USERNAME>
DB_PASSWORD=<PASSWORD>
DB_NAME=<DATABASE NAME>
DB_PORT=<DATABASE PORT>
```

### Debug and Security
For security purposes change the environment variables in the **.env** file to restrict/allow network access to the backend:
```env
# The generated DJANGO SECRET KEY
SECRET_KEY=django-insecure-mt009#ax-n)0o@$=o31pg)e5$!xvx(^7cjdl(kf^6#yeo-tl)%
# Set to True to have more details about the errors, or False when put to production
DEBUG=False
# the allowed DNS that have authorization to connect to the backend (it should be the Database DNS and Frontend DNS)
ALLOWED_HOSTS=[<Front_DNS:PORT>, <DataBase_DNS:PORT>]
# The allowed origins that can send form data to our backend (should be the Frontend DNS)
ALLOWED_CRSF_ORIGINS=<Front_DNS:PORT>
```
## Run the server
### Init the Data
> [!WARNING]
> Don't run this section if you are using the always database pre-configured in this repository, only run it if you are using a new empty database !
To run the server we need to first migrate the data base:
```shell
python manage.py makemigrations
python manage.py migrate
```
Then we have to create a super admin, run this command and follow the instructions:
```shell
python manage-py createsuperuser <USER>
```
We then have to populate the data base with the required data. We first open a django shell:
```shell
python manage.py shell
```
Then we can use the built in script to create the data:
```python
from files import script
script.add_data_type()
script.add_drone_fields()
```
We then have to attribute to our user the ADMIN permissions and set an email address to him. 
in the opened shell enter the commands:
```python
from user.models import User
user = User.objects.get(username=<USER>)
user.email = '<EMAIL>'
user.user_type = 'ADMIN'
user.save()
```
You can now exit the shell.

### Run
To run the backend app you need to run the **Django server**, there **RabbitMQ server** and **Celery**.
in the `/intheair/` directory run this 3 commands in separate terminals:

1- Start Django server:
```shell
python manage.py runserver
```
2- Start RabbitMQ:
```shell
sudo rabbitmq-server start
```
3- Start Celery:
```shell
celery -A intheair worker
```
I hope the installation, configuration and running of the server went smoothly. Contact us if you ever have any problem or issue with the installation that the documentation didn't help you solve.
