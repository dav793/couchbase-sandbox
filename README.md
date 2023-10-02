# Couchbase sandbox
Exploration of Couchbase DB

## Run

### As docker container
Windows/Mac OS/Linux:
```cmd
docker run -t --name db -p 8091-8096:8091-8096 -p 11210-11211:11210-11211 couchbase/server:enterprise-7.2.0
```

Then, to open shell in running container:
```cmd
docker exec -it db /bin/bash
```

### Using docker compose
1. Create + configure `.env` file
```cmd
cp .env.template .env
notepad .env
```

* `WORKING_DIR` : The filesystem path to your project (this directory).
* `DB_SERVER_NAME` : The name you want to assign to the docker container (can use default).

2. Run with docker compose
Windows/Mac OS/Linux: 
```cmd
docker-compose --verbose --env-file .env -f docker/docker-compose.yml up 
```

Then, to open shell in running container:
```cmd
docker exec -it my-couchbase /bin/bash
```

Note that you should change `my-couchbase` for the name you assigned to the docker container in `DB_SERVER_NAME`.

