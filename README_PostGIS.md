# PostGIS Docker setup (nwp_pgsql)

This folder contains a minimal Docker Compose setup to run PostGIS and example init scripts.

Files added:
- `docker-compose.yml` — starts a PostGIS container (`postgis/postgis:15-3.3`).
- `docker/postgres/initdb/init-postgis.sql` — enables PostGIS, creates sample tables, and example users (`officer1`, `officer2`).
- `docker/postgres/initdb/create_user_template.sql` — template to create additional DB users.

Quick start

1. From `e:\#docker_projects\nwp_pgsql` run:

```powershell
docker compose up -d
```

2. Connect with `psql` or QGIS to `localhost:5433`, database `nwpgisdb`, user `gisadmin` / `changeme`.

Add users


- To add a user, copy `create_user_template.sql` to `create_user.sql`, replace `__USERNAME__` and `__PASSWORD__`, then run:

```powershell
psql -h localhost -p 5433 -U gisadmin -d nwpgisdb -f docker\postgres\initdb\create_user.sql
```

Security notes

- Change `POSTGRES_PASSWORD` before exposing the service.
- Do NOT expose Postgres directly to the internet in production. Use a reverse proxy, VPN, or mTLS.
- Consider using Docker secrets or an external secret manager for production credentials.

Next steps I can do for you

- Generate a script to bulk-create the 40 officer users from a CSV.
- Add sample RLS policies and example SQL to enforce GN-based access control.
- Add a backup/restore example (pg_dump/pg_basebackup + WAL).

GeoServer

This compose file can also start a GeoServer instance pre-configured to connect to the PostGIS database.

Key points:
- GeoServer admin URL: http://localhost:8080/geoserver
- Default admin username: admin
- Default admin password: geoserver (change before production)

To start PostGIS + GeoServer:

```powershell
docker compose up -d
```

Add PostGIS store in GeoServer (from GeoServer admin web UI -> Stores -> Add new Store -> PostGIS):

- Host: db
- Port: 5432
- Database: nwpgisdb
- Schema: public
- User: gisadmin
- Password: DigitalDivision
- JDBC connection URL (if required): jdbc:postgresql://db:5432/nwpgisdb

Note: When accessing from your host machine use host `localhost` and mapped port `5433` for direct DB clients; inside the Docker network GeoServer should use `db:5432` as the host:port. Change passwords before exposing services.
