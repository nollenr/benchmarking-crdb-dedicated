# Create Single Region TPC-C Test Cluster

- [Create Single Region TPC-C Test Cluster](#create-single-region-tpc-c-test-cluster)
  - [Create a CockroachDB cluster in CockroachDB Dedicated.](#create-a-cockroachdb-cluster-in-cockroachdb-dedicated)
    - [Create Cluster](#create-cluster)
    - [Check Cluster Status](#check-cluster-status)
    - [Add an item to the IP Allow List](#add-an-item-to-the-ip-allow-list)
    - [Create a New SQL User](#create-a-new-sql-user)
    - [Retrieve the Cert (for SSL)](#retrieve-the-cert-for-ssl)
    - [Connect to the Cluster](#connect-to-the-cluster)
    - [Delete Cluster](#delete-cluster)



## Create a CockroachDB cluster in CockroachDB Dedicated.  
- Cloud Provider: AWS
- Region: us-west-2
- Number of Nodes: 9
- Number of vCPU per node: 8vCPU
- Storage in GB: 300
- Name of cluster: nollen-twilio-clstr


### Create Cluster
HTTP Request
```
curl --request POST \
  --url https://cockroachlabs.cloud/api/v1/clusters \
  --header 'Authorization: Bearer {API KEY} ' \
  --data '{"name":"nollen-twilio-clstr","provider":"AWS","spec":{"dedicated":{"region_nodes":{"us-west-2":9},"hardware":{"machine_spec":{"num_virtual_cpus":8},"storage_gib":300}}}}'
```

Response:
```

{
  "id": "728133a5-6351-4bdb-b939-578fd516c9ac",
  "name": "nollen-twilio-clstr",
  "cockroach_version": "v22.1.1",
  "plan": "DEDICATED",
  "cloud_provider": "AWS",
  "account_id": "456288520688",
  "state": "CREATING",
  "creator_id": "05de9bfb-098c-4179-b80f-f250838c7797",
  "operation_status": "CLUSTER_STATUS_UNSPECIFIED",
  "config": {
    "dedicated": {
      "machine_type": "m5.2xlarge",
      "num_virtual_cpus": 8,
      "storage_gib": 300,
      "memory_gib": 32,
      "disk_iops": 4500
    }
  },
  "regions": [
    {
      "name": "us-west-2",
      "sql_dns": "nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "ui_dns": "admin-nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "node_count": 9
    }
  ],
  "created_at": "2022-06-16T20:22:40.715402Z",
  "updated_at": "2022-06-16T20:22:41.422739Z",
  "deleted_at": null

```

### Check Cluster Status
HTTP Request
```
curl --request GET \
  --url https://cockroachlabs.cloud/api/v1/clusters/728133a5-6351-4bdb-b939-578fd516c9ac \
  --header 'Authorization: Bearer {API KEY}'
```

Response
```
{
  "id": "728133a5-6351-4bdb-b939-578fd516c9ac",
  "name": "nollen-twilio-clstr",
  "cockroach_version": "v22.1.1",
  "plan": "DEDICATED",
  "cloud_provider": "AWS",
  "account_id": "456288520688",
  "state": "CREATING",
  "creator_id": "05de9bfb-098c-4179-b80f-f250838c7797",
  "operation_status": "CLUSTER_STATUS_UNSPECIFIED",
  "config": {
    "dedicated": {
      "machine_type": "m5.2xlarge",
      "num_virtual_cpus": 8,
      "storage_gib": 300,
      "memory_gib": 32,
      "disk_iops": 4500
    }
  },
  "regions": [
    {
      "name": "us-west-2",
      "sql_dns": "nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "ui_dns": "admin-nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "node_count": 9
    }
  ],
  "created_at": "2022-06-16T20:22:40.715402Z",
  "updated_at": "2022-06-16T20:22:43.186586Z",
  "deleted_at": null
```

Note that the state is "CREATING".  When cluster creation is complete the state will be "CREATED".

### Add an item to the IP Allow List
HTTP Request
```
curl --request POST \
  --url https://cockroachlabs.cloud/api/v1/clusters/728133a5-6351-4bdb-b939-578fd516c9ac/networking/allowlist \
  --header 'Authorization: Bearer {API KEY} ' \
  --data '{"cidr_ip":"72.132.195.192","cidr_mask":32,"ui":true,"sql":true,"name":"Ron"}'
```

Response
```
{
  "cidr_ip": "72.132.195.192",
  "cidr_mask": 32,
  "ui": true,
  "sql": true,
  "name": "Ron"
}
```

### Create a New SQL User
HTTP Request
```
curl --request POST \
  --url https://cockroachlabs.cloud/api/v1/clusters/728133a5-6351-4bdb-b939-578fd516c9ac/sql-users \
  --header 'Authorization: Bearer {API KEY}' \
  --header 'content-type: application/json' \
  --data '{"name":"ron","password":"example_password"}'
```

Response
```
{
  "name": "ron"
}
```

### Retrieve the Cert (for SSL)
```
curl --create-dirs -o $HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt -O https://cockroachlabs.cloud/clusters/728133a5-6351-4bdb-b939-578fd516c9ac/cert
```

### Connect to the Cluster
```
cockroach sql --url "postgresql://ron:<ENTER-PASSWORD>@nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud:26257/defaultdb?sslmode=verify-full&sslrootcert=$HOME/Library/CockroachCloud/certs/nollen-twilio-clstr-ca.crt"
```


### Delete Cluster
HTTP Request
```
curl --request DELETE \
  --url https://cockroachlabs.cloud/api/v1/clusters/728133a5-6351-4bdb-b939-578fd516c9ac \
  --header 'Authorization: Bearer {API KEY}'
```

Response
```
{
  "id": "728133a5-6351-4bdb-b939-578fd516c9ac",
  "name": "nollen-twilio-clstr",
  "cockroach_version": "v22.1.1",
  "plan": "DEDICATED",
  "cloud_provider": "AWS",
  "account_id": "456288520688",
  "state": "DELETED",
  "creator_id": "05de9bfb-098c-4179-b80f-f250838c7797",
  "operation_status": "CLUSTER_STATUS_UNSPECIFIED",
  "config": {
    "dedicated": {
      "machine_type": "m5.2xlarge",
      "num_virtual_cpus": 8,
      "storage_gib": 300,
      "memory_gib": 32,
      "disk_iops": 4500
    }
  },
  "regions": [
    {
      "name": "us-west-2",
      "sql_dns": "nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "ui_dns": "admin-nollen-twilio-clstr-76s.aws-us-west-2.cockroachlabs.cloud",
      "node_count": 9
    }
  ],
  "created_at": "2022-06-16T20:22:40.715402Z",
  "updated_at": "2022-06-16T21:22:21.243459Z",
  "deleted_at": "2022-06-16T21:22:21.243277Z"
}
```
