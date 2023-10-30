# Couchbase sandbox
Exploration of Couchbase DB

## 1. Running
First, ensure [Docker Desktop](https://www.docker.com/get-docker) is installed in your machine.

```cmd
docker -v
```

### Run single node as docker container
**Windows/Mac OS/Linux:**
```cmd
docker run -t --name db -p 8091-8096:8091-8096 -p 11210-11211:11210-11211 couchbase/server:enterprise-7.2.0
```

Then, go to `http://localhost:8091` to configure the database for the first time.

Finally, to open shell in running container:
```cmd
docker exec -it db /bin/bash
```

### Run 3-node cluster with docker compose
1. Create + configure `.env` file

    **Mac OS/Linux:**
    ```bash
    cp .env.template .env
    nano .env
    ```

    **Windows:**
    ```cmd
    copy .env.template .env
    notepad .env

    copy .env.bat.template .env.bat
    notepad .env.bat
    ```

    * `WORKING_DIR` : The filesystem path to your project (this directory).
    * `NETWORK_NAME` : Name for the docker network (can use default).
    * `NODE_1_NAME` : Name for the cluster node 1 docker container (can use default).
    * `NODE_2_NAME` : Name for the cluster node 2 docker container (can use default).
    * `NODE_3_NAME` : Name for the cluster node 3 docker container (can use default).

2. Create docker network

    **Mac OS/Linux:**
    ```bash
    source .env && docker network create --driver bridge ${NETWORK_NAME}
    ```

    **Windows:**
    ```cmd
    call .env.bat
    docker network create --driver bridge %NETWORK_NAME% 
    ```

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

    Note that you should change `my-couchbase` for the name you assigned to the docker container in either `NODE_1_NAME`, `NODE_2_NAME` or `NODE_3_NAME`.

### Setup 3-node cluster
Once all cluster nodes are running with docker compose and the master node is configured for the first time, follow these steps for each additional node in the cluster (skip the master node).
Note that the docker containers that host the nodes are named as follows:
* `couchbase-node-1` (master)
* `couchbase-node-2`
* `couchbase-node-3`

1. Identify the node IP address

    **Windows/Mac OS/Linux:**
    ```cmd
    docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container name>
    ```
    Make sure to substitute `<container-name>` for the docker container name of the current node. 

2. Go to admin panel at `http://127.0.0.1:8091` and select option `Servers`.

3. Select option `ADD SERVER` and enter the following node information before clicking on `Add Server`:
    * Hostname/IP Address : Use the address identified in step 1.
    * Username : Use the administrator username set when configuring the master node for the first time.
    * Password : Use the administrator password set when configuring the master node for the first time.

4. Finally, once all nodes are added click on `Rebalance` to make the new nodes active in the cluster. The master node data will be distributed evenly among all 3 nodes.

### Run interactive SQL++ command-line `cbq` 
From a shell in running container, run:

```bash
cd /opt/couchbase/bin
./cbq -u Administrator -p password -engine=http://127.0.0.1:8091
```