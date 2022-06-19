# Environment Setup for TPC-C Testing

## Single Region

The topology for the single region TPC-C test is:
- 9 m5.2xlarge Nodes (the standard instance type used as of this writing for an 8vCPU dedicated cluster)
- 300gb per node
- CockroachDB running 21.1.1 (the standard version used as of this writing for a dedicated cluster)
- an c5a.2xlarge to connect to the cluster and execute the tpc-c benchmark running Amazon Linux 2
- a VPC PrivateLink between the CockroachDB Dedicated VPC and my instance
![single-region-topology](twilio-tpc-c-topology-single-region.jpg)

### Initialize the Workload
```
nohup cockroach workload init tpcc \
--warehouses 7650 \
--concurrency 36 \
"postgresql://ron:${mypass}@internal-nollen-twilio-clstr-778.aws-us-west-2.cockroachlabs.cloud:26257/tpcc?sslmode=verify-full&sslrootcert=$HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt" &
```

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
--duration=10m \
"postgresql://ron:${mypass}@internal-nollen-twilio-clstr-778.aws-us-west-2.cockroachlabs.cloud:26257/tpcc?sslmode=verify-full&sslrootcert=$HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt"
```

### Single Region Workload Results



## 3 Region, TPC-C Run in Same Region as Lease Holders
The multi-region topology is generally the same as the single region workload.  The major difference being that the tables were created as regional tables (primary region us-east-1).

![multi-region-topology](twilio-tpc-c-topology-multi-region.jpg)

## 3 Region, TPC-C Run in Region Closest to Lease Holders

## 3 Region, TPC-C Run in Region Farthest from Lease Holders

