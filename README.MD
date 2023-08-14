# Superset

<img
  src="https://github.com/apache/superset/raw/master/superset-frontend/src/assets/branding/superset-logo-horiz-apache.png"
  alt="Superset"
  width="500"
/>

## Configure Virtual Environment
We will recommend to use Virtual environment for installing Superset.
You may use any editor/IDE as your wish. Here we use pycharm. Go to terminal in pycharm and create a virtual environment.

```shell
$ python3 -m venv venv
$ source venv/bin/activate
```
Once you setup virtual environment, better to install the packages mentioned in the requirements.txt.
```shell
$ pip install -r requirements.txt
```


## Installing and Initializing Superset

First, start by installing apache-superset:

```shell
$ pip install apache-superset
```

Read the documentation before going to next step https://superset.apache.org/docs/installation/configuring-superset/ .
Because you may need to set Secret key in the upcoming steps.

Run this line before upgrading the DB. This is because you may encounter an error saying "Could not locate a Flask application. You did not provide the "FLASK_APP" environment variable, and a "wsgi.py" or "app.py" module was not found in the current directory".
To avoid the error we are doing it ahead.

````shell
$ export FLASK_APP=superset
````

Go to "venv/lib/python3.10/site-packages/superset/" and copy _config.py_ file.
Paste it in your PYTHON_PATH (the place where the python.exe is located, you may type ````$ which python```` in your terminal to find path). 
It would be mostly venv/bin/ for those who use virtual environment.<br>
Rename the newly pasted "config.py" file to "superset_config.py". Superset will look for _superset_config.py_ for all configurations from here on.
Search for "SECRET_KEY".
Change the default SECRET_KEY. Try to give a good strong secure key value as it is not recommended to change once we start using application, which may result in loss of data.<br>
If you want to generate a strong key run this in your terminal. 
```sh 
$ openssl rand -base64 42
```
Copy the generated key and paste against SECRET_KEY.
```bash
SECRET_KEY = 'your_high_secure_key'
```

Now it's time to initialize the DB.
````shell
$ superset db upgrade
````

Create an admin user in your metadata database (use `admin` as username to be able to load the examples)
````shell
$ export FLASK_APP=superset
$ superset fab create-admin
````

Load some data to play with

````shell
$ superset load_examples
````
Create default roles and permissions
````shell
$ superset init
````

Before going to next step you need to download the source code as zip from "https://github.com/apache/superset".
Extract the zip file and copy the folder named "superset-frontend" to the project folder.

Build javascript assets
````shell
$ cd superset-frontend
$ npm ci
$ npm run build
$ cd ..
````

For certain installations, the npm run build may not complete successfully. You may ignore and proceed to next step.

To start a development web server on port 8088, use -p to bind to another port
````shell
$ superset run -p 8088 --with-threads --reload --debugger
````
## Using Application
Login with the admin credentials which you created while configuring.

You can start connecting database with some sample sqlite file.
SQLALCHEMY_DATABASE_URI should be like 'sqlite:////path/to/superset.db'. 
Example - "sqlite:////home/frappe/Downloads/test_db.sqlite3".

Tip: You can connect Mariadb with superset with an additional python lib.
Example - SQLALCHEMY_DATABASE_URI = 'mysql://USER:PASSWORD@localhost:3306/SCHEMA'
OR 'mysql+pymysql://user:password@host/dbname'

If you encounter an error saying cannot be used as a data source for security reason, then go to config.py file.
Search for "PREVENT_UNSAFE_DB_CONNECTIONS" change the value from True to False.

To use mysql database, install the driver -
````shell
$ pip install mysqlclient
````

In certain installation there is a chance to get an error of "cannot find pkg-config".
Run the below command in your terminal and try install mysqlclient again. 
````shell
$ sudo apt install pkg-config
````




