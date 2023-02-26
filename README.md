# Docker Database

## Setup Environment

Create `private/mariadb.env` with following contents :

```
MARIADB_ROOT_PASSWORD=changeme
```

Then, create `private/pma.env` with following contents :

```
MYSQL_ROOT_PASSWORD=changeme
```

Then, Run this command :

```
docker compose -f compose.yaml up -d --build
```

## Using Nginx Proxy Manager

This assume you have followed tutorials on [Docker Environments Repository](https://github.com/mochrira/docker-environments.git). Add following lines at the end of `private/pma.env` file:

```
PMA_ABSOLUTE_URI=http://localhost/phpmyadmin/
```

Then, restart stack

```
docker compose -f compose.yaml down
docker compose -f compose.yaml up -d --build
```

Then, execute this command

```
docker exec nginx /bin/sh -c "mkdir /data/nginx/custom"
docker cp ./config/nginx-proxy-manager/http.conf nginx:/data/nginx/custom/http.conf
docker network connect database nginx
docker restart nginx
```