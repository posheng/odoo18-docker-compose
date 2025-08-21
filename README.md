# This is a Docker Compose setup for an Odoo server environment, including PostgreSQL as the database, PgAdmin as the database management tool, and support for plugins and custom modules storage.

# What is [Odoo](https://www.odoo.com/)?
Odoo, formerly known as OpenERP, is a suite of open-source business apps written in Python and released under the LGPL license. This suite of applications covers all business needs, from Website/Ecommerce down to manufacturing, inventory and accounting, all seamlessly integrated. It is the first time ever a software editor managed to reach such a functional coverage. Odoo is the most installed business software in the world. Odoo is used by 2.000.000 users worldwide ranging from very small companies (1 user) to very large ones (300 000 users).

# Project Structure
```bash
odoo18-docker-compose/
├── odoo-addons/
├── odoo-config/
├── odoo-db-data/
├── odoo-web-data/
├── pgadmin-data/
├── .env
├── .gitignore
├── docker-compose.yml
└── README.md
```

# Docker Compose (Odoo)
The simplest compose.yaml file would be:
```yaml
services:
  odoo:
    image: odoo:latest
    container_name: odoo_web
    restart: always
    depends_on:
      - postgres
    ports:
      - "8069:8069"
    volumes:
      - ./odoo-web-data:/var/lib/odoo
      - ./odoo-config:/etc/odoo
      - ./odoo-addons:/mnt/extra-addons
    environment:
      - HOST=postgres
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}

  postgres:
    image: postgres:17
    container_name: odoo_postgres
    restart: always
    volumes:
      - ./odoo-db-data:/var/lib/postgresql/data/pgdata
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - PGDATA=/var/lib/postgresql/data/pgdata

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: odoo_pgadmin
    restart: always
    depends_on:
      - postgres
    ports:
      - "8080:80"
    volumes:
      - ./pgadmin-data:/var/lib/pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: "example@gmail.com"
      PGADMIN_DEFAULT_PASSWORD: "${PGADMIN_PASSWORD}"
      PGADMIN_LISTEN_PORT: 80
```

# How to Use
## To start your Odoo instance, go in the directory of the compose.yaml file you created from the previous examples and type:

```bash
docker-compose up -d
```

This command will start all the services defined in your docker-compose.yml file in detached mode.

## To stop your Odoo instance, you can run:

```bash
docker-compose down
```

This command will stop and remove all containers defined in your docker-compose.yml file.

## To clean up your volumes (if you want to remove all data), you can run:

```bash
docker-compose down -v
```

# 服務與 URL
|服務名稱	|說明	|URL
|--	|--	|--
|Odoo	|Odoo Web 介面	|http://localhost:8069
|PgAdmin	|PostgreSQL 管理工具	|http://localhost:8080