odoo on ubantu server

To set up Odoo on an Ubuntu server, you can follow these general steps:

1. Update the Ubuntu package index and install the required dependencies:

```
sudo apt update
```

```
sudo apt install git python3-pip build-essential wget python3-dev python3-venv libxml2-dev libxslt1-dev libevent-dev libsasl2-dev libssl-dev libpq-dev
```

2. Create a new system user and group for Odoo:

```
sudo adduser --system --home=/opt/odoo --group odoo
```

3. Clone the Odoo source code from the official repository:

```
sudo su - odoo -s /bin/bash
```

```
git clone https://www.github.com/odoo/odoo --depth 1 --branch 14.0 /opt/odoo/odoo
exit
```

4. Create a new virtual environment for Odoo and install its dependencies:

```
sudo su - odoo -s /bin/bash
cd /opt/odoo
python3 -m venv odoo-venv
source odoo-venv/bin/activate
pip install wheel
pip install -r odoo/requirements.txt
deactivate
exit
```

5. Create a new systemd service file for Odoo:

```
sudo nano /etc/systemd/system/odoo.service
```

6. Paste the following configuration into the file:

```
[Unit]
Description=Odoo
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=odoo
PermissionsStartOnly=true
User=odoo
Group=odoo
ExecStart=/opt/odoo/odoo-venv/bin/python3 /opt/odoo/odoo/odoo-bin -c /etc/odoo.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```

7. Save and close the file.

8. Create a new configuration file for Odoo:

```
sudo nano /etc/odoo.conf
```

9. Paste the following configuration into the file:

```
[options]
; Admin password to access Odoo
admin_passwd = admin_password

; Set the default database to create for the first time installation
db_name = odoo_database

; Odoo server port (default: 8069)
http_port = 80

; Odoo server IP address (default: 0.0.0.0)
; Listen on all addresses.
; Set to a specific IP address to restrict access to that address.
; Set to localhost to listen only on the local machine.
; Set to a specific IP address to listen on a specific interface.
; Example: http://192.168.0.10:8069/
http_interface = 0.0.0.0

; Set the log level (debug, info, warn, error, critical)
logfile = /var/log/odoo/odoo.log
log_level = info
```

10. Save and close the file.
11. Create the log directory and set the correct permissions:

```
sudo mkdir /var/log/odoo
```

```
sudo chown odoo:root /var/log/odoo
```

12. Reload the systemd daemon and start the Odoo service:
```
sudo systemctl daemon-reload
sudo systemctl start odoo
```

13. Check the status of the Odoo service to make sure it is running:

```
sudo systemctl status odoo
```

14. If the service is running, you should see output similar to the following:

```
● odoo.service - Odoo
     Loaded: loaded (/etc/systemd/system/od
```
Note: odoo store path on ubantu:

```
/usr/lib/python3/dist-packages/odoo/addons
```
