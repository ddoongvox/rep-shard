Demo Mongo Sharded Cluster with Docker Compose
=========================================

### Setup
- **Step 1: Start all of the containers**

```bash
sudo docker-compose up -d
```

- **Step 2: Initialize the replica sets (config servers and shards)**

```bash
sudo sh init.sh
```

- **Step 3: Initializing the router**

```bash
sudo docker-compose exec router01 sh -c "mongo < /scripts/init-router.js"
```

- **Step 4: Enable sharding and setup sharding-key**
```bash
sudo docker-compose exec router01 mongo

// Enable sharding for database `MyDatabase`
sh.enableSharding("MyDatabase")

// Setup shardingKey for collection `MyCollection`**
db.adminCommand( { shardCollection: "MyDatabase.MyCollection", key: { supplierId: "hashed" } } )

```

### Verify

- **Verify the status of the sharded cluster**

```bash
docker-compose exec router01 mongo --port 27017
sh.status()
```

- **Verify status of replica set for each shard**
```bash
docker exec -it shard-01-node-a bash -c "echo 'rs.status()' | mongo --port 27017" 
docker exec -it shard-02-node-a bash -c "echo 'rs.status()' | mongo --port 27017" 
docker exec -it shard-03-node-a bash -c "echo 'rs.status()' | mongo --port 27017" 
```

- **Check database status**
```bash
docker-compose exec router01 mongo --port 27017
use MyDatabase
db.stats()
db.MyCollection.getShardDistribution()
```
