# Couchbase sandbox
Exploration of Couchbase DB

## 1. Running
First, ensure [Docker Desktop](https://www.docker.com/get-docker) is installed in your machine.

```cmd
docker -v
```

### Run as docker container
**Windows/Mac OS/Linux:**
```cmd
docker run -t --name db -p 8091-8096:8091-8096 -p 11210-11211:11210-11211 couchbase/server:enterprise-7.2.0
```

Then, go to `http://localhost:8091` to configure the database for the first time.

Finally, to open shell in running container:
```cmd
docker exec -it db /bin/bash
```

### Run with docker compose
1. Create + configure `.env` file

    **Windows:**
    ```cmd
    cp .env.template .env
    notepad .env
    ```

    **Mac OS/Linux:**
    ```bash
    cp .env.template .env
    nano .env
    ```

    * `WORKING_DIR` : The filesystem path to your project (this directory).
    * `DB_SERVER_NAME` : The name you want to assign to the docker container (can use default).

2. Run with docker compose

    **Windows/Mac OS/Linux:**
    ```cmd
    docker-compose --verbose --env-file .env -f docker/docker-compose.yml up 
    ```

    Then, go to `http://localhost:8091` to configure the database for the first time.

3. Open shell in running container

    ```cmd
    docker exec -it my-couchbase /bin/bash
    ```

    Note that you should change `my-couchbase` for the name you assigned to the docker container in `DB_SERVER_NAME`.

### Run interactive SQL++ command-line `cbq` 
From a shell in running container, run:

```bash
cd /opt/couchbase/bin
./cbq -u Administrator -p password -engine=http://127.0.0.1:8091
```

