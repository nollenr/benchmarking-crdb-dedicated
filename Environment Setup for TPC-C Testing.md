# Environment Setup for TPC-C Testing

- [Environment Setup for TPC-C Testing](#environment-setup-for-tpc-c-testing)
  - [Single Region](#single-region)
    - [Initialize the Workload](#initialize-the-workload)
    - [Time to Generate Workload](#time-to-generate-workload)
    - [Backup the Workload](#backup-the-workload)
    - [Test Workload Restore](#test-workload-restore)
    - [Run the Workload](#run-the-workload)
  - [3 Region, TPC-C Run in Same Region as Lease Holders](#3-region-tpc-c-run-in-same-region-as-lease-holders)
  - [3 Region, TPC-C Run in Region Closest to Lease Holders](#3-region-tpc-c-run-in-region-closest-to-lease-holders)
  - [3 Region, TPC-C Run in Region Farthest from Lease Holders](#3-region-tpc-c-run-in-region-farthest-from-lease-holders)

## Single Region

The topology for the single region TPC-C test is:
- 9 m5.2xlarge Nodes (the standard instance type used as of this writing for an 8vCPU dedicated cluster)
- 300gb per node
- CockroachDB running 21.1.1 (the standard version used as of this writing for a dedicated cluster)
- an c5a.2xlarge to connect to the cluster and execute the tpc-c benchmark running Amazon Linux 2
- a VPC PrivateLink between the CockroachDB Dedicated VPC and my instance
![single-region-topology](jpg/twilio-tpc-c-topology-single-region.jpg)

### Initialize the Workload
Workoad initialized in 9 node 8vCPU Cluster
```
nohup cockroach workload init tpcc \
--warehouses 7650 \
--concurrency 36 \
"postgresql://ron:${mypass}@internal-nollen-twilio-clstr-778.aws-us-west-2.cockroachlabs.cloud:26257/tpcc?sslmode=verify-full&sslrootcert=$HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt" &
```

### Time to Generate Workload
|Data|Time|Rows|
|----------------------|---------------|----------------------|
|imported warehouse| 0s| 7650 rows|
|imported district| 1s| 76500 rows|
|imported customer| 2h1m41s| 229500000 rows|
|imported history| 22m52s| 229500000 rows|
|imported order| 44m10s| 229500000 rows|
|imported new_order| 3m50s| 68850000 rows|
|imported item| 1s| 100000 rows|
|imported stock| 2h24m58s| 765000000 rows|
|imported order_line| 3h26m5s| 2295039403 rows|


<br/>

### Backup the Workload
```
BACKUP DATABASE tpcc INTO 's3://nollen-cluster-backup-bucket/tpcc-backup-7650/?AWS_ACCESS_KEY_ID={KeyID}&AWS_SECRET_ACCESS_KEY={Secret}' AS OF SYSTEM TIME '-10s';
```

### Test Workload Restore
```
RESTORE DATABASE tpcc FROM LATEST IN 's3://nollen-cluster-backup-bucket/tpcc-backup-7650/?AWS_ACCESS_KEY_ID=A{KeyID}&AWS_SECRET_ACCESS_KEY={Secret}'
WITH new_db_name = tpccx;
```

### Run the Workload
```
cockroach workload run tpcc \
--warehouses=7650 \
--duration=15m \
"postgresql://ron:${mypass}@internal-nollen-twilio-clstr-778.aws-us-west-2.cockroachlabs.cloud:26257/tpcc?sslmode=verify-full&sslrootcert=$HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt"
```


## 3 Region, TPC-C Run in Same Region as Lease Holders
The multi-region topology is generally the same as the single region workload.  The major difference being that the tables were created as **regional tables** (primary region us-east-1, secondary regions us-east-2 and us-west-2).

The cluster topology is:
us-east-1: 9 nodes 8vCPU per Node
us-east-2: 9 nodes 8vCPU per Node
us-west-2: 9 nodes 8vCPU per Node
300GB per node (4500 IOPS)

The database (tpcc) primary region is us-east-1 and the database survivability goal is regional.  

```
-- put lease holders in the primary region
ALTER DATABASE system CONFIGURE ZONE USING lease_preferences = '[[+region=aws-us-east-1]]';

ALTER DATABASE tpcc PRIMARY REGION "aws-us-east-1";
ALTER DATABASE tpcc ADD REGION "aws-us-east-2";
ALTER DATABASE tpcc ADD REGION "aws-us-west-2";
ALTER DATABASE tpcc SURVIVE REGION FAILURE;

USE tpcc;
SHOW SURVIVAL GOAL FROM DATABASE;
```

![multi-region-topology](jpg/twilio-tpc-c-topology-multi-region.jpg)

## 3 Region, TPC-C Run in Region Closest to Lease Holders
In this test, the "Application Node" running the TPC workload attached to the region closest to the lease holders of the regional tables.  In this case, the lease holders were in the primary region (us-east-1) and application node was in the us-east-2 region and connected to nodes of the cluster in us-east-2.  

![multi-region-topology-2](jpg/twilio-tpc-c-topology-multi-region-2.jpg)

## 3 Region, TPC-C Run in Region Farthest from Lease Holders
In the final test, the "Application Node" running the TPC workload attached to the region farthest from the lease holders or the regional tables.  In this case, the lease holders were in the primary region (us-east-1) and the application node was in the us-west-2 region and connected to nodes of the cluster in us-west-2.

![multi-region-topology-3](jpg/twilio-tpc-c-topology-multi-region-3.jpg)


