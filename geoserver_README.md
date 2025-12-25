GeoServer data volume

The Compose setup creates a Docker volume named `geoserver_data` and mounts it at `/var/lib/geoserver` inside the container. This stores GeoServer configuration and data directory so that restarts don't lose work.

To inspect the volume locally (on Windows with Docker Desktop):

1. List volumes:

   docker volume ls

2. Create a temporary container to view files:

   docker run --rm -it -v nwp_pgsql_geoserver_data:/data alpine sh

Replace `nwp_pgsql_geoserver_data` with the actual volume name shown by `docker volume ls` if Docker Compose prefixes it.

Admin credentials

- Username: admin
- Password: geoserver

Change the password by logging into the GeoServer admin UI and updating it, or set a new password via the `GEOSERVER_ADMIN_PASSWORD` environment variable in `docker-compose.yml` before starting the service.
