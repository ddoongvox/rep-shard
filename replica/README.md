Demo Mongo Sharded Cluster with Docker Compose
=========================================

### Setup
- **Step 1: Start all of the containers**

```bash
sudo docker-compose up -d
```

- **Step 2: Initialize the replica set**

```bash
sudo docker-compose exec mongodb1 sh -c "mongo < /scripts/initiate_replica.js"
```

### Verify

- **Verify the status**

```bash
sudo docker-compose exec mongodb1 mongo
rs.status()
```
