# Concourse Docker 

This is a setup of concourse.ci running in docker containers.  It consists of the following 3 docker containers: councourse-db, concourse-web, concourse-worker.  The creation of the containers is orchestrated with docker-compose.  The containers are split up into 2 networks: web-tier and service-tier.  For completeness this docker-compose file creates all the necessary networks. 

## Requirements

This project uses:

- docker 
- docker-compose 

## Initialize

Before running concourse for the first time there is some setup that needs to be done.  

### Copy default.env to .env

The default.env file needs to be copied to the .env file and have the permissions set to 0600.  This file contains all the variables and secrets used by concourse.  Copy the file and then change the variables.

```bash
cp default.env .env
chmod 0600 .env
```
### Setup security keys

Before running the concourse for the first time you need to generate some keys for concourse to use.  The init script will run the necassary commands to create the needed directoried and generate the keys using openssl.

```./init.sh```

The init script runs the following commnands:

```bash
mkdir database

mkdir -p keys/web keys/worker

ssh-keygen -t rsa -f ./keys/web/tsa_host_key -N ''
ssh-keygen -t rsa -f ./keys/web/session_signing_key -N ''

ssh-keygen -t rsa -f ./keys/worker/worker_key -N ''

cp ./keys/worker/worker_key.pub ./keys/web/authorized_worker_keys
cp ./keys/web/tsa_host_key.pub ./keys/worker
```

## Start the Containers

This project uses docker-compose to orchestrate the docker containers.  The following command will start up all the containers in order.

```docker-compose up```

If you would like to start is in detached mode add the -d option

```docker-compose up -d```

## Access concourse

Your concourse.ci instance should be available at http://localhost:8080

## Advanced Setup

For completeness this project creates all the necassry networks needed for the containers to communicate.  If you are fronting this with nginx or another reverse proxy you will need to have the web-tier network created outside of this project and then joined.

### TODO - create example

