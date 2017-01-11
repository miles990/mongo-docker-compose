# mongo-docker-compose
Deploy a sharded cluster in mongodb using docker-compose

The MongoDB environment consistes of the following docker containers

 - **mongosrs(1-3)n(1-3)**: Mongod data server with three replica sets containing 3 nodes each (9 containers)
 - **mongocfg(1-3)**: Stores metadata for sharded data distribution (3 containers)
 - **mongos(1-2)**: Mongo routing service to connect to the cluster through (1 container)

### Required
[Docker](https://www.docker.com/), test version: 1.12.5
  ```
    curl -sSL https://get.docker.com/ | sh
  ```
[Docker Compose](https://docs.docker.com/compose/overview/), test version: 1.9.0
  ```
    sudo su
    curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    exit
  ```

### MongoDB Version
3.4

### Setup Cluster
This will pull all the images from [Docker index](https://index.docker.io/u/jacksoncage/mongo/) and run all the containers.

    docker-compose up

Please note that you will need docker-compose 1.4.2 or better for this to work due to circular references between cluster members.
You will need to run the following *once* only to initialize all replica sets and shard data across them

    ./initiate

You should now be able connect to mongos1 and the new sharded cluster from the mongos container itself using the mongo shell to connect to the running mongos process

    docker exec -it mongodockercompose_mongos1_1 mongo --port 21017

## Persistent storage
Data is stored at `./data/` and are excluded from version control. Data will be persistent between container runs. To remove all data `./reset`

## TODO

 - Add local Ops Mananger
 - Implement authentication across cluster.  http://docs.mongodb.org/manual/tutorial/deploy-replica-set-with-auth/

## Built upon
 - [Docker-compose wait to start](http://brunorocha.org/python/dealing-with-linked-containers-dependency-in-docker-compose.html)
 - [Bi directional docker commuication](http://abdelrahmanhosny.com/2015/07/01/3-solutions-to-bi-directional-linking-problem-in-docker-compose/)
 - [MongoDB Sharded Cluster by Sebastian Voss](https://github.com/sebastianvoss/docker)
 - [MongoDB](http://www.mongodb.org/)
 - [Mongo Docker ](https://github.com/jacksoncage/mongo-docker)
 - [DnsDock](https://github.com/tonistiigi/dnsdock)
 - [Docker](https://github.com/dotcloud/docker/)
