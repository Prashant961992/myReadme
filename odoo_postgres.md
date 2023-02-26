As Odoo’s databases are handled by the Postgresql database server. Therefore,  next, we need to set-up Postgresql properly. 
Install Postgres from the Debian repository.

```
sudo apt-get install postgresql
```

Then we have to create a database user for managing Odoo databases. Therefore, initially, we need to switch users to Postgres

```
sudo su - postgres
```

Now create a user odoo14 with the following command. Furthermore, at the time you must have to provide a new password for the user odoo14 and that password is needed to provide in the Odoo configuration file at the last step of the installation.

```
createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo14
```

Next, we have to assign this user as a superuser for getting more privileges.

```
psql
ALTER USER odoo14 WITH SUPERUSER;
```

This will print a success message in the terminal. In addition,  exit from the psql and Postgres. With the following commands.

```
\q
exit
```

Seocnd Answer:

PostgreSQL is the default database server used by Odoo. When you install Odoo, it also installs and configures PostgreSQL automatically. However, there are a few things you can do to optimize PostgreSQL for use with Odoo.

1. Increase shared memory settings: Odoo requires a lot of shared memory to operate efficiently. To increase the shared memory settings, open the PostgreSQL configuration file (/etc/postgresql/<version>/main/postgresql.conf) and modify the following settings:
  
```
shared_buffers = 256MB
work_mem = 16MB
max_connections = 200
```
  
2. Configure PostgreSQL to use UTF-8 encoding: Odoo uses UTF-8 encoding for its database, so you should configure PostgreSQL to use UTF-8 encoding as well. To do this, open the PostgreSQL configuration file (/etc/postgresql/<version>/main/postgresql.conf) and modify the following setting:
  
```
  client_encoding = utf8
```
  
3. Create a new PostgreSQL user for Odoo: By default, Odoo uses the PostgreSQL user odoo to connect to the database. However, it's a good practice to create a new PostgreSQL user specifically for Odoo. To create a new PostgreSQL user, run the following commands:
  
  ```
  sudo su - postgres
  createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo_user
  ```
  
  Replace odoo_user with the username you want to use for the Odoo user.

  4. Create a new PostgreSQL database for Odoo: By default, Odoo uses the PostgreSQL database odoo to store its data. However, it's a good practice to create a new PostgreSQL database specifically for Odoo. To create a new PostgreSQL database, run the following command:
  
  ```
  sudo su - postgres
  createdb --encoding=UTF8 --owner=odoo_user odoo_database
  ```
  
  Replace odoo_database with the name you want to use for the Odoo database, and odoo_user with the username you created in step 3.
  
  5. Configure Odoo to use the new PostgreSQL user and database: To configure Odoo to use the new PostgreSQL user and database, edit the Odoo configuration file (/etc/odoo.conf) and modify the following settings:
  
  ```
  db_user = odoo_user
  db_password = <password>
  db_name = odoo_database
  ```
  
  Replace <password> with the password you set for the odoo_user PostgreSQL user in step 3.

  6. Restart PostgreSQL and Odoo: After making changes to the PostgreSQL and Odoo configuration files, restart both PostgreSQL and Odoo to apply the changes:
  
  ```
  sudo service postgresql restart
  sudo service odoo restart
  ```
  
  With these steps, you can set up PostgreSQL for use with Odoo.

