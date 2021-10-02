# MongoDB + Mongo Express

This project is used to have **MongoDB** available on server and
**Mongo Express** available on the internet by HTTPS.

## Prerequisites

Make sure you have installed these:
- [Docker and Docker Compose](https://phoenixnap.com/kb/install-docker-compose-on-ubuntu-20-04) - Will install all the required packages and software.
- (Optional) [Dockerized Nginx with SSL](https://github.com/madrigals1/nginx) - Will generate SSL certificates and make the app accessible through `SSL_DOMAIN`, that is set inside `.env`.

## Installation

Make a copy of `.env.example` file named `.env`

```shell script
cp .env.example .env
```

---

Environment variables:
- `MONGO_PORT` - port for MongoDB.
- `MONGO_INITDB_ROOT_USERNAME` - default username for MongoDB.
- `MONGO_INITDB_ROOT_PASSWORD` - default password for MongoDB.
- `ME_PORT` - port for Mongo Express.
- `ME_CONFIG_BASICAUTH_USERNAME` - default username for Mongo Exporess.
- `ME_CONFIG_BASICAUTH_PASSWORD` - default password for Mongo Exporess.
- SSL settings (Not needed without **Dockerized Nginx**):
    - `MONGO_EXPRESS_SSL_DOMAIN` - website domain for Mongo Express, available through internet.
    - `HTTPS_NETWORK` - network, in which our HTTPS server will be running.
    - `DATABASE_NETWORK` - network, on which our database will be accessible.

```dotenv
# Mongo Settings
MONGO_PORT=27017
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=password

# Mongo Express Settings
ME_PORT=8081
ME_CONFIG_BASICAUTH_USERNAME=admin
ME_CONFIG_BASICAUTH_PASSWORD=password

# Docker settings
HTTPS_NETWORK=https_network
DATABASE_NETWORK=database_network

# SSL Settings
MONGO_EXPRESS_SSL_DOMAIN=mongo.example.com
```

---

Create network with the name, that we have in `HTTPS_NETWORK` environment variable.

```shell script
docker network create https_network
```

Create network with the name, that we have in `DATABASE_NETWORK` environment variable.

```shell script
docker network create database_network
```

---

Build the Docker image

```shell script
docker-compose build
```

## Running

Start
```
docker-compose up
```

**MongoDB** will be available on:
- `mongodb://localhost:27017` for non-Docker environments.
- `mongodb://mongo:27017` inside Docker containers with correct networking set up.

**Mongo Express** will be available on:
- `http://localhost:8081` for local.
- `https://${MONGO_EXPRESS_SSL_DOMAIN}` over the internet.

Stop
```
docker-compose down
```
