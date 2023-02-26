To set up Odoo on Ubuntu server using Docker, you can follow these general steps:

1. Install Docker and Docker Compose on your Ubuntu server:

```
sudo apt-get update
sudo apt-get install docker.io
sudo apt-get install docker-compose
```

2. Create a new directory for the Odoo Docker files:

```
mkdir ~/odoo-docker && cd ~/odoo-docker
```

3. Create a new Docker Compose file for Odoo:

```
nano docker-compose.yml
```

4. Paste the following configuration into the file:

```
version: "3"
services:
  web:
    image: odoo:latest
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
    environment:
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
  db:
    image: postgres:10
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
volumes:
  odoo-web-data:
  odoo-db-data:
```

5. Modify the environment variables as necessary. For example, you can change the POSTGRES_USER and POSTGRES_PASSWORD values to something else.

6. Create a new directory for the Odoo configuration files:

```
mkdir config && cd config
```

7. Create a new configuration file for Odoo:

```
nano odoo.conf
```

8. Paste the following configuration into the file:

```
[options]
addons_path = /usr/lib/python3/dist-packages/odoo/addons
data_dir = /var/lib/odoo/.local/share/Odoo
```

9. Save and close the file.

10. Start the Odoo service using Docker Compose:

```
docker-compose up -d
```

11. Wait for Docker to download and start the Odoo and PostgreSQL containers.
12. Open a web browser and navigate to http://your_server_ip:8069 to access the Odoo application.

13. You can stop the Odoo service using Docker Compose:

```
docker-compose down
```

14. Note that this is a basic setup for Odoo using Docker. You may need to modify the configuration files to suit your specific needs.

